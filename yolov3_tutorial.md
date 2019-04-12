# Convert YOLOv3 Model to IR

* Dump YOLOv3 TenorFlow* Model

  - Clone the repository: 
  ```bash
	  git clone https://github.com/mystic123/tensorflow-yolo-v3.git
	  cd tensorflow-yolo-v3
  ```
  - Download [coco.names](https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names) file from the DarkNet website OR use labels that fit your task. </br>
  **Note**: save the content directly.

  - Download the weights file: 
    * For the YOLOv3 model: [yolov3.weights](https://pjreddie.com/media/files/yolov3.weights)
    ```bash
      wget https://pjreddie.com/media/files/yolov3.weights
    ```
    * For the YOLOv3-tiny model: [yolov3-tiny.weights](https://pjreddie.com/media/files/yolov3-tiny.weights)
    ```bash
    wget https://pjreddie.com/media/files/yolov3-tiny.weights
    ```
  - Run a converter: 
    * For YOLOv3: 
      ```bash
        python3 convert_weights_pb.py --class_names coco.names --data_format NHWC --weights_file yolov3.weights
      ```
    * For YOLOv3-tiny:
    ```bash
    	python3 convert_weights_pb.py --class_names coco.names --data_format NHWC --weights_file yolov3-tiny.weights --tiny
    ```
    * issue
    ```bash
    ValueError: cannot reshape array of size 1112015 into shape (3375,512,1,1)
    ```
    * solution
    ```bash
    problem about coco.name
    ```
* Convert YOLOv3 TensorFlow Model to the IR
  - To generate the IR of the YOLOv3 TensorFlow model, run:
  ```bash
	sudo python3 mo_tf.py --input_model /home/intel/Downloads/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3]
  ```
  - To generate the IR of the YOLOv3-tiny TensorFlow model, run:
  
  **Note**: A few minor modifications to yolo_v3.json, remove "detector/yolo-v3-tiny/Reshape_8" in entry_points filed.
  ```bash
	sudo python3 mo_tf.py --input_model /home/intel/Downloads/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3_tiny.json --input_shape=[1,416,416,3] --output_dir /home/intel/Downloads/tensorflow-yolo-v3/output/yolov3tiny
  ```
* Run sample
```bash
./object_detection_demo_yolov3_async -m /home/intel/Downloads/tensorflow-yolo-v3/output/frozen_darknet_yolov3_model.xml -d GPU -i cam
```


* issues

```bash
[ ERROR ]  List of operations that cannot be converted to IE IR:
[ ERROR ]      LeakyRelu (11)
[ ERROR ]          detector/yolo-v3-tiny/Conv/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_1/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_2/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_3/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_4/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_5/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_6/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_7/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_10/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_11/LeakyRelu
[ ERROR ]          detector/yolo-v3-tiny/Conv_8/LeakyRelu
[ ERROR ]  Part of the nodes was not translated to IE. Stopped.

# solution
sudo -H pip3 install tensorflow==1.12.0 --upgrade
```
```bash
[ ERROR ]  Cannot infer shapes or values for node "detector/yolo-v3-tiny/Conv/LeakyRelu".
[ ERROR ]  Op type not registered 'LeakyRelu' in binary running on intel-desktop. Make sure the Op and Kernel are registered in the binary running in this process. Note that if you are loading a saved graph which used ops from tf.contrib, accessing (e.g.) `tf.contrib.resampler` should be done before importing the graph, as contrib ops are lazily registered when the module is first accessed.
[ ERROR ]  
[ ERROR ]  It can happen due to bug in custom shape infer function <function tf_native_tf_node_infer at 0x7f62fff98a60>.
[ ERROR ]  Or because the node inputs have incorrect values/shapes.
[ ERROR ]  Or because input shapes are incorrect (embedded to the model or passed via --input_shape).
[ ERROR ]  Run Model Optimizer with --log_level=DEBUG for more information.
[ ERROR ]  Stopped shape/value propagation at "detector/yolo-v3-tiny/Conv/LeakyRelu" node. 
 For more information please refer to Model Optimizer FAQ (<INSTALL_DIR>/deployment_tools/documentation/docs/MO_FAQ.html), question #38. 


# solution

```

* references
	* [https://software.intel.com/en-us/forums/computer-vision/topic/800049](https://software.intel.com/en-us/forums/computer-vision/topic/800049)
	
	* [https://software.intel.com/en-us/forums/computer-vision/topic/801626](https://software.intel.com/en-us/forums/computer-vision/topic/801626)
	
	* [https://github.com/PINTO0309/OpenVINO-YoloV3](https://github.com/PINTO0309/OpenVINO-YoloV3)
	
	* [https://software.intel.com/en-us/forums/computer-vision/topic/806712](https://software.intel.com/en-us/forums/computer-vision/topic/806712)
	
	* [https://github.com/PINTO0309/OpenVINO-YoloV3/issues/19](https://github.com/PINTO0309/OpenVINO-YoloV3/issues/19)
	
