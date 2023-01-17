## 1. 下载或者手动导出onnx 
直接现在onnx [weiyun](https://share.weiyun.com/3T3mZKBm) or [google driver](https://drive.google.com/drive/folders/1-8phZHkx_Z274UVqgw6Ma-6u5AKmqCOv) or export onnx:
```bash
# 🔥 yolov8 官方仓库: https://github.com/ultralytics/ultralytics
# 🔥 yolov8 官方教程: https://docs.ultralytics.com/quickstart/
# 🚀TensorRT-Alpha will be updated synchronously as soon as possible!

# 安装 yolov8 环境
conda create -n yolov8 python==3.8 -y
conda activate yolov8
pip install ultralytics==8.0.5

# 下载官方权重(".pt" 格式文件)
https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8n.pt
https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8s.pt
https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8m.pt
https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8l.pt
https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8x.pt
https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8x6.pt
```

导出onnx:
```bash
# 640
yolo mode=export model=yolov8n.pt format=onnx dynamic=True    #simplify=True
yolo mode=export model=yolov8s.pt format=onnx dynamic=True    #simplify=True
yolo mode=export model=yolov8m.pt format=onnx dynamic=True    #simplify=True
yolo mode=export model=yolov8l.pt format=onnx dynamic=True    #simplify=True
yolo mode=export model=yolov8x.pt format=onnx dynamic=True    #simplify=True
# 1280
yolo mode=export model=yolov8x6.pt format=onnx dynamic=True   #simplify=True
```
## 2.编译 onnx文件
```bash
# 把你下载或者导出的onnx放到这个路径:yolov8-tensorrt/data/yolov8
cd tensorrt-alpha/data/yolov8
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/TensorRT-8.4.2.4/lib
# 640
../../../../TensorRT-8.4.2.4/bin/trtexec   --onnx=yolov8n.onnx  --saveEngine=yolov8n.trt  --buildOnly --minShapes=images:1x3x640x640 --optShapes=images:4x3x640x640 --maxShapes=images:8x3x640x640
../../../../TensorRT-8.4.2.4/bin/trtexec   --onnx=yolov8s.onnx  --saveEngine=yolov8s.trt  --buildOnly --minShapes=images:1x3x640x640 --optShapes=images:4x3x640x640 --maxShapes=images:8x3x640x640
../../../../TensorRT-8.4.2.4/bin/trtexec   --onnx=yolov8m.onnx  --saveEngine=yolov8m.trt  --buildOnly --minShapes=images:1x3x640x640 --optShapes=images:4x3x640x640 --maxShapes=images:8x3x640x640
../../../../TensorRT-8.4.2.4/bin/trtexec   --onnx=yolov8l.onnx  --saveEngine=yolov8l.trt  --buildOnly --minShapes=images:1x3x640x640 --optShapes=images:4x3x640x640 --maxShapes=images:8x3x640x640
../../../../TensorRT-8.4.2.4/bin/trtexec   --onnx=yolov8x.onnx  --saveEngine=yolov8x.trt  --buildOnly --minShapes=images:1x3x640x640 --optShapes=images:4x3x640x640 --maxShapes=images:8x3x640x640
# 1280
../../../../TensorRT-8.4.2.4/bin/trtexec   --onnx=yolov8x6.onnx  --saveEngine=yolov8x6.trt  --buildOnly --minShapes=images:1x3x1280x1280 --optShapes=images:4x3x1280x1280 --maxShapes=images:8x3x1280x1280
```
## 3.运行
```bash
git clone https://github.com/FeiYull/tensorrt-alpha
cd tensorrt-alpha/yolov8
mkdir build
cd build
cmake ..
make -j10
# note: 效果图默认保存在路径：yolov8-tensorrt/yolov8/build

## 640
# 推理一张图
./app_yolov8  --model=../../data/yolov8/yolov8n.trt --size=640 --batch_size=1  --img=../../data/6406407.jpg   --show --savePath
./app_yolov8  --model=../../data/yolov8/yolov8n.trt --size=640 --batch_size=8  --video=../../data/people.mp4  --show --savePath  # 视频文件需要自己准备

# 推理一个视频
./app_yolov8  --model=../../data/yolov8/yolov8n.trt     --size=640 --batch_size=8  --video=../../data/people.mp4  --show --savePath=../ # 视频文件需要自己准备

# 推理摄像头
./app_yolov8  --model=../../data/yolov8/yolov8n.trt     --size=640 --batch_size=2  --cam_id=0  --show

## 1280
# 大模型推理摄像头
./app_yolov8  --model=../../data/yolov8/yolov8x6.trt     --size=1280 --batch_size=2  --cam_id=0  --show
```