# 정리

### on

이벤트의 트리거가 될 동작을 정하는 속성

```shell
on:
  pull_request:
    branches:
      - develop
      - master
    paths: "frontend/*"
```

PR이 해당 branch에 들어왔을때, paths 경로 상에 파일 변경이 있으면 트리거된다.

### working-directory

작업이 실행될 디렉토리 위치를 설정

```shell
defaults:
  run:
    working-directory: frontend
```

run의 기본값으로 frontend 디렉토리가 설정됨. (기본은 root)

### permission

GITHUB_TOKEN의 권한을 설정. 자세한건 [여기](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#permissions-for-the-github_token)를 참고

```shell
permissions:
  contents: read
  pages: write
  deployments: write
  id-token: write
  pull-requests: write
```

### jobs

트리거되면 실행될 작업들. 기본적으로는 병렬적으로 실행.

```shell
jobs:
  something:
    name: example_name
      steps:
        - name: step1
          ...
```

steps 내부의 작업은 단계별로 실행

jobs의 순서를 지정해주고 싶다면 needs 키워드 사용하기.

```shell
build:
  runs-on: ubuntu-latest
  ...
  
sonarqube:
  needs: build
  runs-on: ubuntu-latest
```

위 경우엔 sonarqube는 build 후에 실행된다.

### cache

npm 등 다운로드 작업들을 캐시 가능

```shell
uses: actions/setup-node@v3.5.1
  with:
    node-version: 16
    cache: 'npm'
    cache-dependency-path: frontend/package-lock.json
```

![image](https://user-images.githubusercontent.com/28296575/200133260-95531844-10ca-400b-bdf4-b5efe1896124.png)

캐시 사용 후 npm install & build 작업 시간 29s -> 20s 으로 감소

# 에러 노트

### 1. actions/checkout 을 해주지 않았을때

> Error: An error occurred trying to start process '/usr/bin/bash' with working directory '/home/runner/work/gitflow-test/gitflow-test/frontend'. No such file or directory


github action이 정상적인 경로를 찾지 못하는 듯 하다.

### 2. script에서 문법 오타

> Unexpected Token

```shell
[].join('\n); // ' 누락
```

```shell
core.setOutput('comment', comments); // comment -> comments 오타
```

### 3. GITHUB_TOKEN 권한 문제

> Error: Ensure GITHUB_TOKEN has permission "idToken: write".


아래처럼 권한을 설정해주어야 한다.

```shell
permissions:
  contents: read
  pages: write
  id-token: write
```

### 4. OIDC 권한 문제

> Failed to create deployment for ~~~. Invalid actions OIDC token due to sub_invalid, validate around ~~~

- https://github.com/actions/deploy-pages/issues/9


```shell
permissions:
  contents: read
  pages: write
+ deployments: write
  id-token: write

...

jobs:
  deployment:
    name: Deployment-action
    runs-on: ubuntu-latest
+   environment:
+     name: github-pages
+     url: ${{ steps.deployment.outputs.page_url }}
```

### 5. environment protection rules

> Branch "develop" is not allowed to deploy to github-pages due to environment protection rules.

특정 branch에서만 github deploy가 가능하도록 하는 protection rules를 허용해주지 않아서 생긴 문제.

![image](https://user-images.githubusercontent.com/28296575/200129254-7aede7ad-c501-49e2-b5fc-2233693a4048.png)


### 6. 기타 Warning들

> Warning: The `set-output` command is deprecated and will be disabled soon. Please upgrade to using Environment Files. For more information see: https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/

set-output이 deprecated 되었고 곧 사용할 수 없게 된다는 내용.

- https://github.com/actions/toolkit/issues/1218

> Warning: Unexpected input(s) 'github-token', valid inputs are ['header', 'append', 'recreate', 'delete', 'hide_details', 'hide', 'hide_and_recreate', 'hide_classify', 'message', 'path', 'ignore_empty', 'follow_symbolic_links', 'number', 'repo', 'GITHUB_TOKEN']

GITHUB-TOKEN을 github-token으로 적어서 생긴 warning
