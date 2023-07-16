### SRE
온프렘에서 클라우드 서비스로의 전환, 모놀리틱 아키텍처에서 마이크로서비스 아키텍처 전환 등의 변화로 인해, 운영 중 어떠한 문제가 발생했을 때 트러블슈팅 과정이 복잡해졌습니다.

이러한 문제를 해결하고자 DevOps, SRE, Platform Engineering 등의 키워드가 생겨났고, 그 중 SRE는 그러한 트러블슈팅 과정을 간결하게 하고자 Observability를 확보하려는 것이라고 요약할 수 있습니다.
[토스ㅣSLASH 23 - 분산 추적 체계 & 로그 중심으로 Observability 확보하기](https://www.youtube.com/@toss_official)

여러가지 데이터들 (Metric, Log, Trace) 등으로부터 운영 중 어떠한 문제가 발생했는지 파악이 가능해야 하고, 그러한 문제 파악의 과정이 최대한 간결하게 되도록 해야합니다.

### 현 상황
어플리케이션이나 데이터센터의 머신들로부터 로그와 메트릭들을 수집하는데, 파이썬 기반 서비스와 라이브러리를 통해 이를 수행합니다. 그러한 데이터들은 사용 용도와 목적에 맞게 저장될 수 있도록 API 어플리케이션을 거쳐 데이터베이스에 적재되게 됩니다.

### 문제
현재 적재하는 데이터로 운영 중 어떠한 문제가 발생했는지 파악이 가능했지만, 점차 트래픽이 증가하면서 두 가지 문제를 만나게 되었습니다.

1. 데이터베이스 리소스 부족으로 인한 데이터 손실
2. 어플리케이션 단위 데이터 관리의 부재

두 문제에 관하여 해결책으로 나온 것이, 데이터가 어디서 발생이 된 것인지 레이블을 붙이는 것이었습니다. 왜냐하면,

1. 데이터가 어느 서비스에서 발생됐는지 알게 되면, 서비스 별 데이터 중요도를 파악하며 정말 필요한 데이터만 데이터베이스에 적재할 수 있습니다. 이것으로 리소스 부족 문제를 일시적으로 해결할 수 있습니다.
2. 주기적인 데이터 삭제 정책이 있지만, 엔드유저의 입장으로 생각해보았을 때 개연성이 없는 정책이었습니다. 어플리케이션 단위 데이터 삭제 정책을 구현하게 되면, 데이터를 사용하는 엔드유저의 입장을 고려할 수 있게 됩니다.

### 해결책 - 1
레이블을 붙이는 것에는 여러 방법이 있어서 비즈니스 밸류를 고려하여 해결책을 선택해야 합니다. 글 초반에 레퍼런스를 했던 토스의 발표 영상에서, 로그에 trace_id를 붙여 opentelemetry와 연동을 시킨 것처럼 저도 그런 식으로 해결하면 좋을 것 같다는 생각을 했었습니다. 그러나 협업 비용과, 이제 막 내부 서비스들이 opentracing에서 opentelemetry로 전환해 나가고 있는 현 상황을 고려해보았을 때, 이것은 현 상황에서 바로 시도할 만한 적절한 해결책이 아니였습니다.

가장 좋은 해결책은 아닐 수 있겠지만, 하루빨리 문제를 해결하는 것이 좋다는 점을 고려했을 때 어플리케이션이 사용하는 로그 수집 라이브러리에 로직을 추가하여 레이블을 추가하는 것이 좋겠다고 생각하게 되었습니다.

레이블의 값으로는 프로젝트 레포지토리 이름이 후보였지만 모노레포의 형태를 지닌 어플리케이션들이 있었기 때문에, 레포지토리 이름보다는 구체화되지만 어플리케이션을 대표할만한 레이블이 필요했습니다.

여기서 python의 sys.modules를 생각하게 되었습니다.
[파이썬 공식 문서](https://docs.python.org/3/library/sys.html#sys.modules "Permalink to this definition")에서는 sys.modules에 대해 아래와 같이 서술하고 있습니다.
>sys.modules
>This is a dictionary that maps module names to modules which have already been loaded. This can be manipulated to force reloading of modules and other tricks. However, replacing the dictionary will not necessarily work as expected and deleting essential items from the dictionary may cause Python to fail. If you want to iterate over this global dictionary always use `sys.modules.copy()` or `tuple(sys.modules)` to avoid exceptions as its size may change during iteration as a side effect of code or activity in other threads.

파이썬 프로세스에서 가장 처음에 실행한 모듈이 \_\_main\_\_ 모듈이 되므로, 이 키로 sys.modules 딕셔너리에 접근하면 application entrypoint에 대한 데이터를 얻을 수 있습니다. application의 entrypoint 같은 경우는 application의 대표성을 띄는 형태로 구성되어있기 때문에, 아래와 같은 식으로 로직을 추가하면 모노레포 기반 어플리케이션도 레이블을 붙일 수 있게 됩니다.

```python
main_module_path = sys.modules["__main__"].__file__
entrypoint = main_module_path.split('/')[-1]
```

### 정리
sys.modules를 이용한 로직을 통해 application identifier를 로그에 추가하였습니다. 로그 수집 라이브러리 같은 경우 자식 패키지의 형태로 주로 프로젝트에서 쓰이기 때문에 부모 패키지의 메타데이터들에 대하여 접근하기 어렵지만 sys.modules를 사용한 우회방법을 발견하고 나서 글로 기록해야겠다 다짐했던 것 같습니다.

물론 이렇게 로직을 추가하는 것은 sustainable한 방안이 아닐 수 있다고 생각해서, application identifier 개선기는 앞으로 시리즈로 이어질 것 같습니다.

다음 글로 돌아오겠습니다.