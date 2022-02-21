윈도우에서 github를 클론했을 때 종종 invalid path 에러가 발생하는 경우가 생깁니다.

이는 clone 하는 파일 이름에 특수문자가 들어가서 윈도우 파일 시스템에서 인식하지 못하는 경우에 발생합니다.

git config를 바꿈으로써 해결할 수 있습니다.

git config 변경하기
윈도우 탐색기를 열고 git clone을 수행한 로컬 디렉토리로 이동한 후 마우스 오른쪽 버튼을 누른 후 git bash here를 클릭하여 git bash 창을 엽니다.

git bash 창에 다음과 같이 입력합니다.


```java

$ git config core.protectNTFS false 
$ git checkout -f HEAD


```
인식이 불가능한 파일명이 변경된 후 repo clone이 수행됩니다.

