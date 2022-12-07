# FreeYOLO-OpenVINO in C++

This tutorial includes a C++ demo for OpenVINO, as well as some converted models.

## Download FreeYOLO OpenVINO model
Main results on COCO-val:

| Model          |  Scale  |    AP    |    AP50    |  XML  |
|----------------|---------|----------|------------|----------|
| FreeYOLO-Nano  |  640    |      |        | [github](https://github.com/yjh0410/FreeYOLO/releases/download/weight/yolo_free_nano_openvino.zip) |
| FreeYOLO-Tiny  |  640    |   34.4   |   53.9     | [github](https://github.com/yjh0410/FreeYOLO/releases/download/weight/yolo_free_tiny_openvino.zip) |
| FreeYOLO-Large |  640    |      |        | [github](https://github.com/yjh0410/FreeYOLO/releases/download/weight/yolo_free_large_openvino.zip) |
| FreeYOLO-Huge  |  640    |      |        |  |

## Install OpenVINO Toolkit

Please visit [Openvino Homepage](https://docs.openvinotoolkit.org/latest/get_started_guides.html) for more details.

## Set up the Environment

### For Linux

**Option1. Set up the environment tempororally. You need to run this command everytime you start a new shell window.**

```shell
source /opt/intel/openvino_2021/bin/setupvars.sh
```

**Option2. Set up the environment permenantly.**

*Step1.* For Linux:
```shell
vim ~/.bashrc 
```

*Step2.* Add the following line into your file:

```shell
source /opt/intel/openvino_2021/bin/setupvars.sh
```

*Step3.* Save and exit the file, then run:

```shell
source ~/.bashrc
```


## Convert model

1. Export ONNX model
   
   Please refer to the [ONNX tutorial](../../ONNXRuntime). **Note that you should set --opset to 10, otherwise your next step will fail.**

2. Convert ONNX to OpenVINO 

   ``` shell
   cd <INSTSLL_DIR>/openvino_2021/deployment_tools/model_optimizer
   ```

   Install requirements for convert tool

   ```shell
   sudo ./install_prerequisites/install_prerequisites_onnx.sh
   ```

   Then convert model.
   ```shell
   python3 mo.py --input_model <ONNX_MODEL> --input_shape <INPUT_SHAPE> [--data_type FP16]
   ```
   For example:
   ```shell
   python3 mo.py --input_model yolox_tiny.onnx --input_shape [1,3,640,640] --data_type FP16
   ```  

   Make sure the input shape is consistent with [those](yolo_free_openvino.cpp#L24-L25) in cpp file. 

## Build 

### Linux
```shell
source /opt/intel/openvino_2021/bin/setupvars.sh
mkdir build
cd build
cmake ..
make
```

## Demo

### c++

```shell
./yolo_free_openvino <XML_MODEL_PATH> <IMAGE_PATH> <DEVICE>
```
