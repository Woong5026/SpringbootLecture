### gitAction이란?

허브 저장소 내에서 코드 프로젝트를 빌드, 테스트, 패키지, 릴리스 또는 배포하기 위해 설정할 수있는 사용자 지정 자동화 프로세스이다. 

**추가적인 ci/cd툴을 사용하지 않고 깃허브 하나로 버전관리부터 테스트 배포까지 가능**

### 깃액션 주요 개념

- workflow

![image](https://user-images.githubusercontent.com/78454649/142092383-02853473-7815-4162-b028-cb89c32d8512.png)

프로젝트를 빌드, 테스트, 패키지, 릴리스 또는 배포하기 위한 **전체적인 프로세스**입니다.

Job으로 구성되고, Event에 의해 예약되거나 트리거될 수 있는 자동화된 절차

Workflow 파일은 YAML으로 작성되고, Github Repository의 .github/workflows 폴더 아래에 저장

Github에게 YAML 파일로 정의한 자동화 동작을 전달하면, Github Actions는 해당 파일을 기반으로 그대로 실행

- job

Job은 여러 Step으로 구성되고, 단일 가상 환경에서 실행된다. 다른 Job에 의존 관계를 가질 수도 있고, 독립적으로 병렬로 실행될 수도 있다.

서로다른 두 개의 작업은 선행관계를 지정할 수 있으며, 후행작업은 선행작업이 끝난 이후에야 실행될 수 있습니다. 만약 선행관계가 설정되지 않은 경우 병렬로 실행됩니다.

- step

Step은 순차적으로 명령어를 수행합니다. 크게 Uses와 Run으로 작업 단위가 나뉘는데, Uses는 이미 다른 사람들이 정의한 명령어를 가져와 실행하는 것이고, 

Run은 npm install나 mkdir example과 같이 가상환경 내에서 실행할 수 있는 스크립트를 말합니다.

하나의 작업 내에서 액션들은 항상 선행관계가 존재하며 선행액션이 실패한 경우 후행액션은 취소되고, 덧붙여서 해당 작업은 실패

- event

워크플로우를 실행시키는 조건을 설정합니다. 

예를 들어 해당 레포지토리에 Code가 push 됐을때만, 또는 풀리퀘했을 때, 또는 master branch에 변경사항이 있었을 때 등으로 조건을 줄 수 있습니다.

- runner

Github 액션 러너 애플리케이션이 설치된 서버이다. Github에서 호스팅 하는 러너를 사용할 수도 있고 직접 호스팅 할 수도 있다. 

Github에서 호스팅 하는 러너는 Ubuntu Linux, Windows, macOS 환경을 기반으로 하며 워크 플로우의 각 작업은 새로운 가상 환경에서 실행된다.

- 이벤트목록

자주사용하는 이벤트: on push , on pull_request 

일부 이벤트에는 하위타입이 존재할 수 있습니다.

on issues
type : opened → 이슈가 새롭게 열린 경우

type : edited → 이슈가 수정된 경우

type : closed → 이슈가 닫힌 경우

...

---

### 워크플로우 작성

- 트리거 설정

on:에는 감지할 이벤트를 적습니다. 여러개를 사용할 수 있습니다.

  on:

    push:
    
    pull_request:

- 브랜치 범위 제한 

각 이벤트 하위에 branches:를 사용하여 감지할 브랜치의 범위를 좁힐 수 있습니다.

on:

    push:
    
        branches: 
          # 단일 브랜치
          - master

          # 다중 브랜치
          - [master, dev]

          # * 또는 ** 정규식 매칭
          # refs/heads/releases/ 하위의 브랜치에 적용됩니다.
          - 'releases/**'

    pull_request:

### 작업

작업은 여러개가 기술될 수 있으며 jobs:에서 시작합니다.

jobs:

    #
    # 작업 식별자가 build_and_test인 작업을 생성합니다.
    build_and_test:

    #
    # 작업 식별자가 nofity_to_slack인 작업을 생성합니다.
    notify_to_slack:
    
### 호스트 운영체제 명세

runs-on:을 사용하여 호스트 운영체제를 명세할 수 있습니다

jobs:

    build_and_test:
    
        runs-on: **ubuntu-latest**

### 선행관계 설정
jobs:

    job1:
    
        ....
        
    job2:
    
        needs: job1
        
    job3:
    
        needs: [job1, job2]

### 지속적 통합 (CI)

여러명이 개발을 하는 도중에 코드가 손상되는 경우도 있겠죠. 

이것을 방지하기 위해 대부분 개발용 브랜치를 마스터에 병합하기 전에 코드 테스트를 수행하여 검사하는 방식을 사용합니다.


이것을 구현하는 방법은 생각보다 간단합니다 마스터 브랜치에 pull_request를 날렸을 때 테스트 코드를 수행하도록 깃허브 액션을 설정하면 됩니다. 

실제 워크플로우 파일은 다음과 같습니다.

```java

#
# 워크플로우의 이름입니다.
name: Hello_CI

#
# master에 pull_request가 발생할 경우에 워크플로우를 실행합니다.
on:
    pull_request:
        branches: [master]

jobs:
    #
    # build_and_test을 이름으로 갖는 작업을 정의합니다.
    build_and_test:
        runs-on: ubuntu-latest
        strategy:
            matrix:
                node-version: [8.x, 10.x, 12.x]
                container:
                    [
                        "ubuntu-latest",
                        "centos-latest",
                        "windows-latest",
                        "macos-latest",
                    ]

        steps:
            #
            # 프로젝트의 루트(`$GITHUB_WORKSPACE`)로 이동합니다.
            - uses: actions/checkout@v2

            #
            # 프로젝트에 필요한 의존성을 설치합니다.
            - name: Install dependencies.
              run: |
                  npm ci
                  npm install

            #
            # 테스트를 실행합니다.
            - name: Execute test.
              run: npm test

```
이제 풀 리퀘스트가 발생하면, 워크플로우에서 실행했던 테스트 결과가 함께 표시됩니다. 

### 지속적 배포 (CD)

마스터에 새로운 코드가 푸시되면 바뀐 코드를 서버에 배포해야 합니다. 

이것을 매번 사람이 하기에는 번거로우므로 마스터에 코드가 푸시되면 서버에 배포하도록 깃허브 액션을 설정하면 됩니다. 

물론 테스트가 먼저 이루어져야 합니다.

여기서는 서버에 배포하는 것 대신에 슬랙 메세지 알림으로 대체하겠습니다.

```java

name: Hello_CI

on:
    push:
        branches: [master]
    pull_request:
        branches: [master]

jobs:
    build_and_test:
        runs-on: ubuntu-latest
        strategy:
            matrix:
                node-version: [8.x, 10.x, 12.x]
                container:
                    [
                        "ubuntu-latest",
                        "centos-latest",
                        "windows-latest",
                        "macos-latest",
                    ]
        steps:
            - uses: actions/checkout@v2

            - name: Install dependencies.
              run: |
                  npm ci
                  npm install

            - name: Execute test.
              run: npm test

    alert_to_slack:
        runs-on: ubuntu-latest

        #
        # push로 발생한 경우에만 해당 작업을 실행합니다.
        # 즉, pull_request로 발생했다면 아래 작업은 실행되지 않습니다.
        if: github.event_name == 'push'

        #
        # build_and_test가 성공한 뒤에 실행됩니다.
        needs: build_and_test

        steps:
            - uses: 8398a7/action-slack@v3
              with:
                  status: custom
                  fields: workflow,job,commit,repo,ref,author,took
                  custom_payload: |
                      {
                          username: 'action-slack',
                          icon_emoji: ':robot_face:',
                          attachments: [{
                              color: '${{ job.status }}' === 'success' ? 'good' : '${{ job.status }}' === 'failure' ? 'danger' : 'warning',
                              text: `${process.env.AS_WORKFLOW}\n${process.env.AS_JOB} (${process.env.AS_COMMIT}) of ${process.env.AS_REPO}@master by ${process.env.AS_AUTHOR} succeeded in ${process.env.AS_TOOK}`,
                          }]
                      }

              #
              # 레포지터리에 secrets으로 설정된 키-값을 가져올 수 있습니다.
              env:
                  SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}

```
위의 파일의 맨 마지막 줄에 주목해주세요. 슬랙에 메세지를 보내기 위해 웹 훅 URL이 사용된 것 처럼 민감한 정보가 요구될 수 있습니다. 

이 때, 워크플로우 파일에 직접 적는 것은 위험한 행동입니다. 레포지터리에 시크릿을 생성하세요.

![image](https://user-images.githubusercontent.com/78454649/142135748-1ec13467-2e0a-4074-b30e-15b87963c0b3.png)


---
### 생성방법

![image](https://user-images.githubusercontent.com/78454649/142113179-b11ca996-7647-456f-a890-f069f4948092.png)

Action > set up a workflow yourself > main.yml 자동생성

- on 

사용자가 커밋했을때 5번쨰 줄 밑에 코드가 실행된다는 뜻

여러가지 이벤트가 발생시에는 on[push, pull_request] 와 같이 써주면 된다

- runs on : 실행시 구동되는 컴퓨터가 ubuntu 라는 뜻(윈도우 , 맥 사용가능)

- steps : 실제로 벌어지는 일들을 적은 곳

스텝에 실제 동작하는 코드는 run에다가 적어주면 된다





