여기도 변경이 있어야 작동이 되는걸까?

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
