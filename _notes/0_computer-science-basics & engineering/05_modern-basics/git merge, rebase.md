---
---

merge는 rebase를 안하고 한다면 commit의 조상이 2개 생기는 것
rebase는 patch를 새로 만들어서 적용시키는 것
따라서 rebase 후 merge를 하게 되면 fast-foward로 HEAD의 위치가 옮겨짐.
따라서 pull할 때 마다 rebase해주는 것이 안전하다.