KUNLUN_OPS 开发手册

# 1.功能及优势介绍
KUNLUN_OPS功能：
- 基于算子库产出公开的头文件进行新的算子开发；
- 通过单测验证自定义算子正确性及性能；

KUNLUN_OPS优势：
- 定制化高性能算子开发
- moe类优化算子和量化算子

# 2. 开发环境
镜像选择：iregistry.baidu-int.com/xmlir/xmlir_ubuntu_2004_x86_64:v0.32
```
#!/bin/bash
#
# Usage: bash start_xmlir_docker.sh
# or env KLXLAKE=/klxlake TAG=v0.32 bash start_xmlir_docker.sh

readonly PROJECT="xmlir"
readonly REPO="xmlir_ubuntu_2004_x86_64"
readonly TAG=${TAG:-v0.32}

readonly KLXLAKE=${KLXLAKE:-"/klxlake"}
if [[ ! -d ${KLXLAKE} ]]; then
  echo "[ERROR] klxlake not found! Please mount klxlake to ${KLXLAKE} or export KLXLAKE"
  exit 1
fi

XPU_NUM=8
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
        DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi

docker run -it ${DOCKER_DEVICE_CONFIG}                 \
        --net=host                                     \
        --cap-add=SYS_PTRACE --security-opt seccomp=unconfined \
        --tmpfs /dev/shm:rw,nosuid,nodev,exec,size=32g \
        --cap-add=SYS_PTRACE                           \
        -v ${PWD}/:/workspace/          \
        -v ${KLXLAKE}:${KLXLAKE}        \
        --name ${YOUR_DOCKER_NAME}      \
        -w /workspace \
        iregistry.baidu-int.com/${PROJECT}/${REPO}:${TAG} /bin/bash
```

安装依赖
* 安装最新版本 xdnn pytorch

```
stable版本（警告ci测试）
bash tools/init_pytorch_eager.sh

latest版本
bash tools/init_pytorch_eager.sh latest
```

xpytorch: https://klx-sdk-release-public.su.bcebos.com/kunlun2aiak_output/20260122/xpytorch-cp310-torch251-ubuntu2004-x64.run

# 3.编译xpuop算子库
注意：可以不依赖 xpytorch 环境
```
bash build.sh optimized_ops
```
测试
```
./output/optimized_ops/test/test_kunlun --gtest_filter=*.function_xpu3
```

# 3.编译xopsblock算子模块
注意：可以不依赖 xpytorch 环境
```
bash build.sh xops_blocks
```

# 3.编译kunlun op算子模块
必须依赖 xpytorch 环境
```
bash build.sh kunlun_ops
```
版本产看
```
python -m kunlun_ops --doctor
```
测试
```
export XPUAPI_DEBUG=0x1
export XPURT_DISPATCH_MODE=PROFILING
python3 test/easy_test_xdnn_activation.py
```
