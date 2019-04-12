```bash
intel@intel-tank1:/opt/intel/computer_vision_sdk/deployment_tools/model_optimizer$ python3 mo_tf.py --input_model /home/intel/Downloads/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json --output_dir ~/Downloads/tensorflow-yolo-v3/output
Model Optimizer arguments:
Common parameters:
	- Path to the Input Model: 	/home/intel/Downloads/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb
	- Path for generated IR: 	/home/intel/Downloads/tensorflow-yolo-v3/output
	- IR output name: 	frozen_darknet_yolov3_model
	- Log level: 	ERROR
	- Batch: 	Not specified, inherited from the model
	- Input layers: 	Not specified, inherited from the model
	- Output layers: 	Not specified, inherited from the model
	- Input shapes: 	Not specified, inherited from the model
	- Mean values: 	Not specified
	- Scale values: 	Not specified
	- Scale factor: 	Not specified
	- Precision of IR: 	FP32
	- Enable fusing: 	True
	- Enable grouped convolutions fusing: 	True
	- Move mean values to preprocess section: 	False
	- Reverse input channels: 	False
TensorFlow specific parameters:
	- Input model in text protobuf format: 	False
	- Offload unsupported operations: 	False
	- Path to model dump for TensorBoard: 	None
	- List of shared libraries with TensorFlow custom layers implementation: 	None
	- Update the configuration file with input/output node names: 	None
	- Use configuration file used to generate the model with Object Detection API: 	None
	- Operations to offload: 	None
	- Patterns to offload: 	None
	- Use the config file: 	/opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json
Model Optimizer version: 	1.5.12.49d067a0
[ ERROR ]  Shape [ -1 416 416   3] is not fully defined for output 0 of "inputs". Use --input_shape with positive integers to override model input shapes.
[ ERROR ]  Cannot infer shapes or values for node "inputs".
[ ERROR ]  Not all output shapes were inferred or fully defined for node "inputs". 
 For more information please refer to Model Optimizer FAQ (<INSTALL_DIR>/deployment_tools/documentation/docs/MO_FAQ.html), question #40. 
[ ERROR ]  
[ ERROR ]  It can happen due to bug in custom shape infer function <function tf_placeholder_ext.<locals>.<lambda> at 0x7f7969663bf8>.
[ ERROR ]  Or because the node inputs have incorrect values/shapes.
[ ERROR ]  Or because input shapes are incorrect (embedded to the model or passed via --input_shape).
[ ERROR ]  Run Model Optimizer with --log_level=DEBUG for more information.
[ ERROR ]  Stopped shape/value propagation at "inputs" node. 
 For more information please refer to Model Optimizer FAQ (<INSTALL_DIR>/deployment_tools/documentation/docs/MO_FAQ.html), question #38. 
```


It seems there's an issue related to the tensorflow (v 1.13) that gets installed by OpenVINO. 

Having these done, get the things going smoothly:

```bash
    # tensorflow v1.13 also works fine
    pip3 install tensorflow==1.12.0 --upgrade
    git clone https://github.com/mystic123/tensorflow-yolo-v3.git
    cd tensorflow-yolo-v3
    wget https://pjreddie.com/media/files/yolov3.weights
    wget https://github.com/pjreddie/darknet/blob/master/data/coco.names
    python ./convert_weights_pb.py --data_format NHWC
    python c:\Intel\computer_vision_sdk\deployment_tools\model_optimizer\mo_tf.py --input_model frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config c:\Intel\computer_vision_sdk\deployment_tools\model_optimizer\extensions\front\tf\yolo_v3.json --input_shape=[1,416,416,3]
```


install tensorflow
```bash
pip3 install tensorflow
Collecting tensorflow
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/77/63/a9fa76de8dffe7455304c4ed635be4aa9c0bacef6e0633d87d5f54530c5c/tensorflow-1.13.1-cp36-cp36m-manylinux1_x86_64.whl (92.5MB)
    100% |████████████████████████████████| 92.5MB 19kB/s 
Collecting six>=1.10.0 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
Collecting keras-preprocessing>=1.0.5 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/c0/bf/0315ef6a9fd3fc2346e85b0ff1f5f83ca17073f2c31ac719ab2e4da0d4a3/Keras_Preprocessing-1.0.9-py2.py3-none-any.whl (59kB)
    100% |████████████████████████████████| 61kB 5.8MB/s 
Collecting wheel>=0.26 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/96/ba/a4702cbb6a3a485239fbe9525443446203f00771af9ac000fa3ef2788201/wheel-0.33.1-py2.py3-none-any.whl
Collecting gast>=0.2.0 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/4e/35/11749bf99b2d4e3cceb4d55ca22590b0d7c2c62b9de38ac4a4a7f4687421/gast-0.2.2.tar.gz
Collecting termcolor>=1.1.0 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/8a/48/a76be51647d0eb9f10e2a4511bf3ffb8cc1e6b14e9e4fab46173aa79f981/termcolor-1.1.0.tar.gz
Collecting absl-py>=0.1.6 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/da/3f/9b0355080b81b15ba6a9ffcf1f5ea39e307a2778b2f2dc8694724e8abd5b/absl-py-0.7.1.tar.gz (99kB)
    100% |████████████████████████████████| 102kB 4.1MB/s 
Collecting tensorboard<1.14.0,>=1.13.0 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/0f/39/bdd75b08a6fba41f098b6cb091b9e8c7a80e1b4d679a581a0ccd17b10373/tensorboard-1.13.1-py3-none-any.whl (3.2MB)
    100% |████████████████████████████████| 3.2MB 525kB/s 
Collecting numpy>=1.13.3 (from tensorflow)
  Using cached https://pypi.tuna.tsinghua.edu.cn/packages/35/d5/4f8410ac303e690144f0a0603c4b8fd3b986feb2749c435f7cdbb288f17e/numpy-1.16.2-cp36-cp36m-manylinux1_x86_64.whl
Collecting protobuf>=3.6.1 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/5a/aa/a858df367b464f5e9452e1c538aa47754d467023850c00b000287750fa77/protobuf-3.7.1-cp36-cp36m-manylinux1_x86_64.whl (1.2MB)
    100% |████████████████████████████████| 1.2MB 1.1MB/s 
Collecting astor>=0.6.0 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/35/6b/11530768cac581a12952a2aad00e1526b89d242d0b9f59534ef6e6a1752f/astor-0.7.1-py2.py3-none-any.whl
Collecting grpcio>=1.8.6 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/f4/dc/5503d89e530988eb7a1aed337dcb456ef8150f7c06132233bd9e41ec0215/grpcio-1.19.0-cp36-cp36m-manylinux1_x86_64.whl (10.8MB)
    100% |████████████████████████████████| 10.8MB 162kB/s 
Collecting keras-applications>=1.0.6 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/90/85/64c82949765cfb246bbdaf5aca2d55f400f792655927a017710a78445def/Keras_Applications-1.0.7-py2.py3-none-any.whl (51kB)
    100% |████████████████████████████████| 61kB 3.9MB/s 
Collecting tensorflow-estimator<1.14.0rc0,>=1.13.0 (from tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/bb/48/13f49fc3fa0fdf916aa1419013bb8f2ad09674c275b4046d5ee669a46873/tensorflow_estimator-1.13.0-py2.py3-none-any.whl (367kB)
    100% |████████████████████████████████| 368kB 3.2MB/s 
Collecting werkzeug>=0.11.15 (from tensorboard<1.14.0,>=1.13.0->tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/18/79/84f02539cc181cdbf5ff5a41b9f52cae870b6f632767e43ba6ac70132e92/Werkzeug-0.15.2-py2.py3-none-any.whl (328kB)
    100% |████████████████████████████████| 337kB 2.9MB/s 
Collecting markdown>=2.6.8 (from tensorboard<1.14.0,>=1.13.0->tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/f5/e4/d8c18f2555add57ff21bf25af36d827145896a07607486cc79a2aea641af/Markdown-3.1-py2.py3-none-any.whl (87kB)
    100% |████████████████████████████████| 92kB 5.5MB/s 
Collecting setuptools (from protobuf>=3.6.1->tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/c8/b0/cc6b7ba28d5fb790cf0d5946df849233e32b8872b6baca10c9e002ff5b41/setuptools-41.0.0-py2.py3-none-any.whl (575kB)
    100% |████████████████████████████████| 583kB 2.1MB/s 
Collecting h5py (from keras-applications>=1.0.6->tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/30/99/d7d4fbf2d02bb30fb76179911a250074b55b852d34e98dd452a9f394ac06/h5py-2.9.0-cp36-cp36m-manylinux1_x86_64.whl (2.8MB)
    100% |████████████████████████████████| 2.8MB 551kB/s 
Collecting mock>=2.0.0 (from tensorflow-estimator<1.14.0rc0,>=1.13.0->tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/e6/35/f187bdf23be87092bd0f1200d43d23076cee4d0dec109f195173fd3ebc79/mock-2.0.0-py2.py3-none-any.whl (56kB)
    100% |████████████████████████████████| 61kB 5.9MB/s 
Collecting pbr>=0.11 (from mock>=2.0.0->tensorflow-estimator<1.14.0rc0,>=1.13.0->tensorflow)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/14/09/12fe9a14237a6b7e0ba3a8d6fcf254bf4b10ec56a0185f73d651145e9222/pbr-5.1.3-py2.py3-none-any.whl (107kB)
    100% |████████████████████████████████| 112kB 6.6MB/s 
Building wheels for collected packages: gast, termcolor, absl-py
  Running setup.py bdist_wheel for gast ... done
  Stored in directory: /home/intel/.cache/pip/wheels/46/85/ca/e81b54f271ca53efa4212d02e67e6ac7cbf413cd8c0342f976
  Running setup.py bdist_wheel for termcolor ... done
  Stored in directory: /home/intel/.cache/pip/wheels/e3/d8/fc/50ab6e66e3dead21d5afff006dc5298913a3064be2b1105359
  Running setup.py bdist_wheel for absl-py ... done
  Stored in directory: /home/intel/.cache/pip/wheels/58/1c/db/076c6993729ee486e849354b0e65b052da7a06f7038924a715
Successfully built gast termcolor absl-py
Installing collected packages: six, numpy, keras-preprocessing, wheel, gast, termcolor, absl-py, setuptools, protobuf, werkzeug, grpcio, markdown, tensorboard, astor, h5py, keras-applications, pbr, mock, tensorflow-estimator, tensorflow
Successfully installed absl-py-0.7.1 astor-0.7.1 gast-0.2.2 grpcio-1.19.0 h5py-2.9.0 keras-applications-1.0.7 keras-preprocessing-1.0.9 markdown-3.1 mock-2.0.0 numpy-1.16.2 pbr-5.1.3 protobuf-3.7.1 setuptools-41.0.0 six-1.12.0 tensorboard-1.13.1 tensorflow-1.13.1 tensorflow-estimator-1.13.0 termcolor-1.1.0 werkzeug-0.15.2 wheel-0.33.1
```

```bash
sudo python3 mo_tf.py --input_model /home/intel/Downloads/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3]
```
