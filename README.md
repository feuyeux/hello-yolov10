# Hello YOLOv10

- PyTorch implementation of [YOLOv10: Real-Time End-to-End Object Detection](<https://arxiv.org/abs/2405.14458>)

| Model     | Test Size | #Params | FLOPs  | AP<sup>val</sup> | Latency |
| :-------- | :-------: | :-----: | :----: | :--------------: | :-----: |
| YOLOv10-N |    640    |  2.3M   |  6.7G  |      38.5%       | 1.84ms  |
| YOLOv10-S |    640    |  7.2M   | 21.6G  |      46.3%       | 2.49ms  |
| YOLOv10-M |    640    |  15.4M  | 59.1G  |      51.1%       | 4.74ms  |
| YOLOv10-B |    640    |  19.1M  | 92.0G  |      52.5%       | 5.74ms  |
| YOLOv10-L |    640    |  24.4M  | 120.3G |      53.2%       | 7.28ms  |
| YOLOv10-X |    640    |  29.5M  | 160.4G |      54.4%       | 10.70ms |

## Installation

```sh
git clone https://github.com/THU-MIG/yolov10
cd yolov10
virtualenv yolov10 --python=python3.9
source yolov10/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install -e .
```

## Validation

```sh
yolo val model=jameslahm/yolov10{n/s/m/b/l/x} data=coco.yaml batch=256
```

## Training

```sh
yolo detect train data=coco.yaml model=yolov10n/s/m/b/l/x.yaml epochs=500 batch=256 imgsz=640 device=0,1,2,3,4,5,6,7
```

## Prediction

```sh
yolo predict model=yolov10n/s/m/b/l/x.pt
```

## Export

```sh
# End-to-End ONNX
yolo export model=yolov10n/s/m/b/l/x.pt format=onnx opset=13 simplify
# Predict with ONNX
yolo predict model=yolov10n/s/m/b/l/x.onnx

# End-to-End TensorRT
yolo export model=yolov10n/s/m/b/l/x.pt format=engine half=True simplify opset=13 workspace=16
# Or
trtexec --onnx=yolov10n/s/m/b/l/x.onnx --saveEngine=yolov10n/s/m/b/l/x.engine --fp16
# Predict with TensorRT
yolo predict model=yolov10n/s/m/b/l/x.engine
```
