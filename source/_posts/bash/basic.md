---
title: 常用的 bash shell
p: bash/basic
date: 2018-05-13 21:31:28
tags:
  - bash
---

#### 常用 shell 汇总

```shell
#!/bin/bash

DIR_HOME="${BASH_SOURCE-$0}"
DIR_HOME="$(dirname "$DIR_HOME")"
# 执行bash脚本的文件路径
PRGDIR="$(cd "${DIR_HOME}"; pwd)"

jarfile=$PRGDIR/app.jar

# 获取运行中的程序的pid
pid=$(ps -ef | grep java | grep $jarfile | awk '{print $2}')
```

```shell
#!/usr/bin/env bash

url="http://127.0.0.1:8088/heartbeat";
echo $url
while [ true ]
do
    sleep 1
    HTTP_CODE=`curl -G -m 10 -o /dev/null -s -w %{http_code} $url`
    echo "http code: ${HTTP_CODE}"
    if [ ${HTTP_CODE} -eq 200 ]
    then
        echo "server start success..."
        exit 0
    fi
done
```