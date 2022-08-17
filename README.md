参考RK给出的Yolov5仓库(https://github.com/airockchip/yolov5.git)
环境：ubuntu18.04、pytorch 1.8.0 、rknn_toolkit2-1.2.0
训练前需要修改两处yolo代码：
1. moders/yolo.py-----def _initialize_biases(self, cf=None):
![image](https://user-images.githubusercontent.com/42601033/185025037-8902b293-66ac-41b5-8ee7-95945ce4fa38.png)
2. utils/general.py-----def output_to_target(output, width, height):
![image](https://user-images.githubusercontent.com/42601033/185025099-932a327d-5341-426b-926d-87b30a3fcae9.png)
训练：
    python train.py --img 640 --batch 16 --epochs 100--data /content/demo.yaml --cfg ./models/yolov5s.yaml  --weights yolov5s.pt #开始训练
导出ONNX模型：
1. 修改models/export.py
![image](https://user-images.githubusercontent.com/42601033/185025310-af82af0c-f0c9-4a56-b282-f85cafe19a1b.png)
2. python models/export.py --weights /content/yolov5/runs/train/exp0/weights/best.pt --img 640 --batch 1
3. 简化ONNX模型：python -m onnxsim /content/yolov5/runs/train/exp0/weights/best.onnx   best_cj.onnx
RKNN环境搭建：
1. apt-get install python3.6 python3-pip
2. python3.6 -m pip install  --upgrade pip
3. python3.6 -m pip  install numpy
4. wget  https://github.com/rockchip-linux/rknn-toolkit2/raw/58fb25e1e3bfd88781c85720c0c11d77f078079b/packages/rknn_toolkit2-1.2.0_f7bb160f-cp36-cp36m-linux_x86_64.whl
5. python3.6 -m pip install rknn_toolkit2-1.2.0_f7bb160f-cp36-cp36m-linux_x86_64.whl
6. git clone https://github.com/rockchip-linux/rknn-toolkit2.git
RKNN模型导出：
1. Netron查看三个对应输出节点。
2. 参考 rknn-toolkit2/examples/onnx/yolov5 ，修改对应输出节点，并且可以通过模拟器测试模型。
![Uploading image.png…]()
