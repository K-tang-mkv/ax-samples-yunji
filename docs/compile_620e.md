# 源码编译（AX620E）

ax-samples 的源码编译目前有两种实现路径：

- **本地编译**：由于开发板集成了完整的Linux系统，可以预装必要的 gcc、cmake 等开发环境，因此可以在开发板上直接完成源码编译；
- **交叉编译**：在 x86 PC 的常规开发环境中，通过对应的交叉编译工具链完成对源码的编译。

## 本地编译

由于 AX620Q 有且仅有 SIP 256MiB 内存，因此不会存在本地编译的需求。  

## 交叉编译

### 3rdparty 准备

- 下载预编译好的 [OpenCV 库文件](https://github.com/AXERA-TECH/ax-samples/releases/download/v0.1/opencv-arm-linux-gnueabihf-gcc-7.5.0.zip)；
- 在 ax-samples 创建 3rdparty 文件，并将下载好的 OpenCV 库文件压缩包解压到该文件夹中，文件夹目录结构如下所示

```bash
ax-samples$ tree -L 3
.
├── 3rdparty
│   └── opencv-arm-linux
│       ├── bin
│       ├── build_opencv.sh
│       ├── include
│       ├── lib
│       └── share
```

### 编译环境
- cmake 版本大于等于 3.13
- AX620E 配套的交叉编译工具链 `arm-linux-gnueabihf-gxx` 已添加到环境变量中

### 安装交叉编译工具链

- Arm32 Linux 交叉编译工具链 [获取地址](http://releases.linaro.org/components/toolchain/binaries/7.5-2019.12/arm-linux-gnueabihf/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar.xz)

### 依赖库准备

请联系 FAE 获取 AX620E BSP 开发包，执行如下操作

```
tar -zxvf AX620E_SDK_XXX.tgz
cd AX620E_SDK_XXX/package
tar -zxvf msp.tgz
```

解压完成后将文件夹内此 `msp/out` 目录设置到临时环境变量，具体操作如下：

```
export ax_bsp=${ax620e_bsp_sdk PATH}/msp/out
```

### 源码编译
git clone 下载源码，进入 ax-samples 根目录，创建 cmake 编译任务：

```bash
git clone https://github.com/AXERA-TECH/ax-samples.git
cd ax-samples
mkdir build && cd build
cmake -DCMAKE_TOOLCHAIN_FILE=../toolchains/arm-linux-gnueabihf.toolchain.cmake -DBSP_MSP_DIR=${ax_bsp}/ -DAXERA_TARGET_CHIP=ax620e ..
make -j6
make install
```

编译完成后，生成的可执行示例存放在 `ax-samples/build/install/ax620e/` 路径下：

```bash
ax-samples/build$ tree install
install
└── ax620e
    ├── ax_classification
    ├── ax_crowdcount
    ├── ax_depth_anything
    ├── ax_imgproc
    ├── ax_model_info
    ├── ax_rtdetr
    ├── ax_scrfd
    ├── ax_simcc_pose
    ├── ax_yolov5_face
    ├── ax_yolov5s
    ├── ax_yolov5s_seg
    ├── ax_yolov6
    ├── ax_yolov7
    ├── ax_yolov7_tiny_face
    ├── ax_yolov8
    ├── ax_yolov8_pose
    ├── ax_yolov8_seg
    ├── ax_yolov9
    └── ax_yolox
```
