---
layout: post
title: "tensorflow-gpu-dev: libtensorflow_framework.so: undefined reference to cuMemcpyHtoD_v2"
description: ""
category: misc
tags: [tensorflow]
modify: 2018-04-27 06:39:05
---

update: 2018-04-27


按照tensorflow官方指南，用Docker装了tensorflow-gpu-dev镜像，编译源码时出现大量报错：

```bash
ERROR: /build/tensorflow-git/src/tensorflow-cuda/tensorflow/contrib/factorization/BUILD:106:1: Linking of rule '//tensorflow/contrib/factorization:gen_gen_clustering_ops_py_wrappers_cc' failed (Exit 1).
/usr/bin/ld: warning: libcuda.so.1, needed by bazel-out/host/bin/_solib_local/_U_S_Stensorflow_Scontrib_Sfactorization_Cgen_Ugen_Uclustering_Uops_Upy_Uwrappers_Ucc___Utensorflow/libtensorflow_framework.so, not found (try using -rpath or -rpath-link)
bazel-out/host/bin/_solib_local/_U_S_Stensorflow_Scontrib_Sfactorization_Cgen_Ugen_Uclustering_Uops_Upy_Uwrappers_Ucc___Utensorflow/libtensorflow_framework.so: undefined reference to `cuMemFree_v2'
bazel-out/host/bin/_solib_local/_U_S_Stensorflow_Scontrib_Sfactorization_Cgen_Ugen_Uclustering_Uops_Upy_Uwrappers_Ucc___Utensorflow/libtensorflow_framework.so: undefined reference to `cuMemsetD32Async'
......
```

搜了一圈，发现似乎是链接不上cuda库的原因，解决方案如下：

```
echo "/usr/local/cuda-9.0/targets/x86_64-linux/lib/stubs" > /etc/ld.so.conf.d/cuda-9.0-stubs.conf
ldconfig
```


参考：

+ https://github.com/tensorflow/tensorflow/issues/13243
+ https://github.com/tensorflow/tensorflow/issues/15889
+ https://stackoverflow.com/questions/13428910/how-to-set-the-environmental-variable-ld-library-path-in-linux
