Q. 여기도 변경이 있어야 작동이 되는걸까? A. **Yes**

# 에러 노트

### 1. actions/checkout 을 해주지 않았을때

```shell
Error: An error occurred trying to start process '/usr/bin/bash' with working directory '/home/runner/work/gitflow-test/gitflow-test/frontend'. No such file or directory
```

github action이 정상적인 경로를 찾지 못하는 듯 하다.

### 2. script에서 문법 오타

```shell
[].join('\n); // ' 누락
```

```shell
core.setOutput('comment', comments); // comment -> comments 오타
```

### 3. 기타 Warning들

```shell
Warning: The `set-output` command is deprecated and will be disabled soon. Please upgrade to using Environment Files. For more information see: https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/
```

set-output이 deprecated 되었고 곧 사용할 수 없게 된다는 내용.


```shell
Warning: Unexpected input(s) 'github-token', valid inputs are ['header', 'append', 'recreate', 'delete', 'hide_details', 'hide', 'hide_and_recreate', 'hide_classify', 'message', 'path', 'ignore_empty', 'follow_symbolic_links', 'number', 'repo', 'GITHUB_TOKEN']
```

GITHUB-TOKEN을 github-token으로 적어서 생긴 warning
