  ## YOLOv3
* Dump YOLOv3 TenorFlow* Model
  - Clone the repository: 
  ```bash
  mkdir -p ~/code && cd ~/code
  git clone https://github.com/mystic123/tensorflow-yolo-v3.git
  sudo ln -sf ~/code/tensorflow-yolo-v3 /opt/openvino_toolkit/
  ```
  - Download [coco.names](https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names) file from the DarkNet website OR use labels that fit your task. </br>
  ```bash
  cd ~/code/tensorflow-yolo-v3
  wget https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names
  ```
  - Download the weights file: 
  ```bash
  cd ~/code/tensorflow-yolo-v3
  wget https://pjreddie.com/media/files/yolov3.weights
  ```
  - Run a converter: 
  ```bash
  cd ~/code/tensorflow-yolo-v3
  python3 convert_weights_pb.py --class_names coco.names --data_format NHWC --weights_file yolov3.weights.1
  ```
* Convert YOLOv3 TensorFlow Model to the IR
```bash
#FP32 precision model
sudo python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model ~/code/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3] --data_type=FP32 --output_dir /opt/openvino_toolkit/tensorflow-yolo-v3/output/FP32   
#FP16 precision model
sudo python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model ~/code/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3] --data_type=FP16 --output_dir /opt/openvino_toolkit/tensorflow-yolo-v3/output/FP16   
```



