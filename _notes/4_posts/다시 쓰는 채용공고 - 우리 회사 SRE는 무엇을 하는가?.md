---
title: "다시 쓰는 채용공고 - 우리 회사 SRE는 무엇을 하는가?"
---
#SRE

[다시 쓰는 이력서 - 다이나믹 듀오](https://youtu.be/2Kzw76husJA)

## 서론
---

> _... SRE 엔지니어가 잡무만 하는 것처럼 보일 수 있겠지만, ..._

X사의 AWS 리인벤트 참석 후기 세미나를 듣던 중 SRE 엔지니어에 대해 나온 이야기이다. 

지인이 AWS 리인벤트를 갔다왔다고 해서 그저 호기심으로 기대없이 신청하고 들었던 세미나였는데, SRE팀에서 대부분 발표를 해주셔서 타사의 SRE분들의 업무에 대해서 조금이나마 들을 수 있었다. 그리고 위의 멘트는 당시 한창 꽤나 고된 프로젝트를 진행중이던 나에게 이 글을 써야겠다는 동기가 되었다. 나 역시 누군가에게 내가 하는 일을 설명하고자 하면, 그저 잡무로 통칭할 수 있는 일을 하고 있지 않나 하는 생각이 들었었기 때문이다.

내가 하는 일을 잡무로 통칭하지 않고, 어떻게 정의 및 정리하면 좋을지 생각을 하다가, 가장 맨 처음 이 회사에 들어올 때 접했던 채용공고[^1]를 다시 보면서 정리해보기로 하였다.

## 본론
---

채용공고 핵심내용들을 간략히 해석한 후 지금까지 했던 일을 바탕으로 추가 설명하며 정리해보자. 
Observability와 Montioring 이라는 단어가 자주 나올텐데 [이 글](https://circleci.com/blog/observability-vs-monitoring/?utm_source=google&utm_medium=sem&utm_campaign=sem-google-dg--japac-en-dsa-tROAS-auth-nb&utm_term=g_-_c__dsa_&utm_content=&gclid=Cj0KCQiAo-yfBhD_ARIsANr56g7d9VAwa7Ja4vmG7frZW0VWqhobb_86b-9yEFNPCB2CEbTkad3btxQaAtLhEALw_wcB)을 보면 두 단어를 이해하는데 도움이 된다.

추가로
```cpp
class SRE implements DevOps
```
[라는 말이 있는데](https://youtu.be/uTEL8Ff1Zvk), 채용공고를 보기 전 명심하고 읽는다면 SRE가 하는 일을 이해하는데 도움이 될 것이다.<br> ==SRE는 DevOps를 실현하는 하나의 방법론이라고 생각한다.==

### **EXPECTATIONS AND TASKS**

> As a Developer you will be working in the global ${MAIN PRODUCT} QA Infrastructure team in ${COMPANY} ${MAIN PRODUCT} & Analytics Cross-Engineering together with a group of exceptionally talented and motivated colleagues. You will be part of SRE (Site Reliability Engineer) team and will be responsible for maintaining and implementing observability stack of ${MAIN PRODUCT} QA infrastructure to help service operations and support teams.

- #### 해석
	- 하는 일과 기대하는 것
		- 회사 메인 제품의 QA(Quality Assurance, 품질 보증) 인프라팀에 SRE(Site Reliability Engineer, 사이트 신뢰성 엔지니어)로 인프라팀의 서비스 운영과 팀원들을 observability stack을 유지, 개발, 보수함으로써 도와주게 될 것이다.
- #### 추가 설명
	- 회사 메인 제품은 **인메모리 RDB**로, 규모가 크고 특수한 feature들을 테스트해야 하기 때문에 자체 CI/CD 파이프라인 위에서 QA 과정이 이루어진다. 자체 CI/CD 파이프라인을 구성하는 다양한 서비스들을 QA Infrastructure팀에서 개발 및 운영하고 있다. 그러한 서비스들이 잘 운영되고 있는 지 확인을 하기 위한 observability stack들로 오픈소스/써드파티 솔루션(Sentry, Jaeger, Grafana)이나 자체 제작한 툴들을 이용한다.

### **Roles & Responsibilities For SRE**

> - Implement/operate observability stack
> - Operate ${MAIN PRODUCT} QA Infrastructure services
> - Enhance operational qualities of services through development
> - Handle incidents and be On-Call to keep services reliable and available

- #### 해석
	- SRE의 역할과 책임
		1. observability stack 개발 및 실행
		2. 메인 제품의 QA(품질 보증) 인프라 서비스 운영
		3. 개발을 통한 서비스 운영 퀄리티 향상
		4. 장애 대응 및 신뢰성있고 가용성있는 서비스 유지를 위한 응답
- #### 추가 설명
	1. Grafana OSS 버전에서 Enterprise 버전으로 이전 작업중에 있고 인프라팀이 운영하는 서비스에 대한 로그를 적재하는 데이터베이스를 관리 및 운영한다. 데이터베이스에 들어가는 분당 트래픽은 1M~2M 정도 나오는데, 재밌는 점은 반정형 데이터인 로그를 인메모리 정형 데이터베이스(RDB)에 적재하고 있다는 점이다.
	2. 인프라 서비스들이 로그 데이터베이스를 접근할 수 있게 하는 flask기반 HTTP REST API를 제공하는 서비스를 운영 중이다.
	3. 2번에서 말한 서비스에서 더 나은 observability와 성능 향상을 위해 기능을 추가하거나 보수한다. 예를 들면 로그 조회 성능이 떨어지는 이유를 찾기 위해 Jaeger trace를 붙인다던지, 혹은 Grafana Loki의 LogQL을 SQL로 바꾸어 쿼리를 보내기 위해 대응되는 REST API Endpoint를 개발하는 일을 했다.
	4. 운영중인 로그 데이터베이스에 문제가 생기면 장애 대응 및 사후 조치 문서를 만들고 대응한다. Grafana에서 Slack으로 alert을 내보내며 ChatOps 형태로 장애 대응이 이루어지고 있다.

>    We are looking for SRE Developer Associates/Developers who are committed to a technology career path. Your set of application documents should contain a cover letter and tabular CV, copies of the obtained degrees and references (if available).

- #### 해석
	- 열정적인 신입/경력 찾아요, 제출 문서는 ~ 내주세요.

### **Education And Qualifications/ Skills And Competencies** 
> We require that you meet the core competencies and skills listed below.
> 
> **Minimum**  
> - Bachelor’s degree or equivalent in Computer Science, Computer Engineering, related field or related technical discipline
> - Basic Python programming skills
> - Basic SQL skills
> - Linux experience and bash scripting skills
> - Excellent communication skills in English 

- #### 해석
	- 학력 및 요구 사항/ 스킬과 경쟁력
	- 최소 요건
		1. 컴공 관련 전공 대졸
		2. 파이썬 기본
		3. SQL 기본
		4. 리눅스 경험과 쉘 스크립트
		5. 유창한 영어
- #### 추가 설명
	1. -
	2. Rust / Go 도 다른 서비스에서 쓰긴 하지만 대부분 파이썬을 사용하고, 특히나 우리가 책임지고 있는 서비스나 배치 작업들은 모두 파이썬으로 이루어져 있기 때문에 기본적인 파이썬 지식은 갖추어야 한다.
	3. 또한 RDB를 운영하고 Grafana에서의 SQL 쿼리를 통해 Monitoring을 구현하기 때문에 기본적인 SQL은 다룰 줄 알아야 한다.
	4. 자체 데이터센터 내 머신에서 SUSELinux를 운영체제로 사용하고 있기 때문에 몇몇 리눅스 명령어들을 알아야하고 쉘 스크립트를 해석하고 작성할 줄 알아야 한다.
	5. 독일이나 중국 팀과 업무를 진행하므로 **영어를 유창하게 해야한다.**

>**Preferred**
> - 1+ years' experience as SRE/DevOps Engineer
> - Experience in Docker and container orchestration technologies (Mesos, Kubernetes)
> - Experience in Cloud technologies (AWS/GCP/Azure)
> - Hands-on experience in large-scale databases
> - Experience in Web Server programming
> - Advanced Python debugging skills
> - Experience with IaC (Terraform, Ansible)
> - Experience in monitoring and analyzing infrastructure issues
> - Experience in implementing and operating an observability stack (Grafana)

- #### 해석
	- 우대사항
		1. 1년 이상의 SRE/DevOps 엔지니어 경험
		2. Docker & container orchestration(Mesos/k8s) 경험
		3. 클라우드 3사 경험
		4. 대규모 데이터베이스 운영 경험
		5. 웹 서버 프로그래밍 경험
		6. 우수한 파이썬 디버깅
		7. IaC(Terraform, Ansible) 경험
		8. 인프라 장애 모니터링 및 분석 경험
		9. Observability stack(Grafana) 운영 및 실행 경험
- #### 추가 설명
	1. 새로 꾸려진 팀이어서 경력이 있다면 좋다.
	2. 인프라팀의 기본 소양이며, 자체 데이터센터 머신 관리로는 Mesos + Marathon UI를 사용하고 있다. k8s 도입을 PoC 프로젝트로 건의했지만 5000대 이상 노드의 클러스터에서의 성능 증명과 당시 PoC를 pod를 spawn하는 것이 아니라 JobScheduler를 통해 구현했어야 적합했다는 이유로 적극적으로 받아들여지진 않았다. 그러나 모든 서비스가 Mesos를 사용하는 것은 아니고, 몇몇 서비스는 자체 k8s 클러스터를 운영중이긴 하다.
	3. Hybrid cloud 형태로, 즉 자체 데이터센터와 클라우드 3사 제공 인프라에서 CI/CD 파이프라인을 운영하고 있기 때문에 클라우드 경험이 있으면 좋다.
	4. 35TB 규모의 replication된 데이터베이스를 운영하므로 대규모 데이터베이스 운영 경험은 소중하다.
	5. HTTP REST API 서비스도 개발 및 운영하므로 웹 서버 프로그래밍 경험이 있으면 좋다.
	6. 거의 대부분의 인프라 서비스들이 파이썬 기반이므로 파이썬 기반 디버깅을 잘 할 줄 알아야 한다.
	7. 서비스 배포시 Terraform을 사용한다.
	8. 장애가 발생한다 == 문제가 주어진다 라고 생각하는데, 이러한 문제들을 주어진 정보(모니터링)를 토대로 많이 풀어본(분석) 경험이 있다면 좋다.
	9. Monitoring stack으로 Grafana를 사용중이다.

## 결론
---

회사 SRE 채용공고를 통해 내가 지금까지 했던 일을 되돌아 보고 앞으로 어떤 일을 할지 정리해보았다. 글을 쓰고 나니 앞으로 다른 이들에게 내가 하는 일을 소개할 때 '스크립트 짜기'나 '잡무'라는 간략한 설명보다는 조금 더 잘 설명할 수 있을 것 같다.
이 글이 SRE에 관심을 가지고 있지만 철학적으로만 설명이 되어 있어 실질적으로 이해하기 어려웠던 사람들에게 도움이 되었으면 좋겠다.

[^1]: https://www.linkedin.com/jobs/view/3014631573