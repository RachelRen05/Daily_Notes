# Convert YOLOv3 Model to IR

* Dump YOLOv3 TenorFlow* Model

  - Clone the repository: 
  ```bash
	  git clone https://github.com/mystic123/tensorflow-yolo-v3.git
	  cd tensorflow-yolo-v3
  ```
  - Download [coco.names](https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names) file from the DarkNet website OR use labels that fit your task.
  ```bash
    wget https://github.com/pjreddie/darknet/blob/master/data/coco.names
  ```
  - Download the weights file: 
    * For the YOLOv3 model: [yolov3.weights](https://pjreddie.com/media/files/yolov3.weights)
    ```bash
      wget https://pjreddie.com/media/files/yolov3.weights
    ```
  - Run a converter: 
    * For YOLOv3: 
      ```bash
        python3 convert_weights_pb.py --class_names coco.names --data_format NHWC --weights_file yolov3.weights
      ```
* Convert YOLOv3 TensorFlow Model to the IR
  - To generate the IR of the YOLOv3 TensorFlow model, run:
  ```bash
    sudo python3 mo_tf.py --input_model /home/intel/Downloads/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3]
  ```
* Run sample
```bash
./object_detection_demo_yolov3_async -m /home/intel/Downloads/tensorflow-yolo-v3/output/frozen_darknet_yolov3_model.xml -d GPU -i cam
```
