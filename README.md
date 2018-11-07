# ros2_openvino_toolkit

## 1. Introduction
The [OpenVINO™](https://software.intel.com/en-us/openvino-toolkit) toolkit quickly deploys applications and solutions that emulate human vision. Based on Convolutional Neural Networks (CNN), the Toolkit extends computer vision (CV) workloads across Intel® hardware, maximizing performance.

This project is a ROS2 wrapper for CV API of [OpenVINO™](https://software.intel.com/en-us/openvino-toolkit), providing the following features:
* Support CPU and GPU platforms
* Support standard USB camera and Intel® RealSense™ camera
* Support Video or Image file as detection source
* Face detection
* Emotion recognition
* Age and gender recognition
* Head pose recognition
* Demo application to show above detection and recognitions

## 2. Prerequisite
- An x86_64 computer running Ubuntu 16.04 and Ubuntu 18.04. Below processors are supported:
	* 6th-8th Generation Intel® Core™
	* Intel® Xeon® v5 family
	* Intel®  Xeon® v6 family
- ROS2 [Bouncy](https://github.com/ros2/ros2/wiki)
- OpenVINO™ Toolkit
	* [OpenVINO™ Toolkit](https://software.intel.com/en-us/openvino-toolkit)
	* OpenVINO™ Toolkit Open Source<br>
  		* The [Deep Learning Deployment Toolkit](https://github.com/opencv/dldt) that helps to enable fast, heterogeneous deep learning inferencing for Intel® processors (CPU and GPU/Intel® Processor Graphics), and supports more than 100 public and custom models.<br>
		* [Open Model Zoo](https://github.com/opencv/open_model_zoo) includes 20+ pre-trained deep learning models to expedite development and improve deep learning inference on Intel® processors (CPU, Intel Processor Graphics, FPGA, VPU), along with many samples to easily get started.
- RGB Camera, e.g. RealSense D400 Series or standard USB camera or Video/Image File
- Graphics are required only if you use a GPU. The official system requirements for GPU are:
	* 6th to 8th generation Intel® Core™ processors with Iris® Pro graphics and Intel® HD Graphics
	* 6th to 8th generation Intel® Xeon® processors with Iris Pro graphics and Intel HD Graphics (excluding the e5 product family, which does not have graphics)
	* Intel® Pentium® processors N4200/5, N3350/5, N3450/5 with Intel HD Graphics

- Use one of the following methods to determine the GPU on your hardware:
	* [lspci] command: GPU info may lie in the [VGA compatible controller] line.
	* Ubuntu system: Menu [System Settings] --> [Details] may help you find the graphics information.
	* Openvino: Download the install package, install_GUI.sh inside will check the GPU information before installation.

## 3. Environment Setup
<<<<<<< HEAD
* Install ROS2 [Bouncy](https://github.com/ros2/ros2/wiki) ([guide](https://github.com/ros2/ros2/wiki/Linux-Development-Setup))<br>
* Install Intel® RealSense™ SDK 2.0 [(tag v2.14.1)](https://github.com/IntelRealSense/librealsense/tree/v2.14.1)<br>
	* [Install from source code](https://github.com/IntelRealSense/librealsense/blob/v2.14.1/doc/installation.md)(Recommended)<br>
	* [Install from package](https://github.com/IntelRealSense/librealsense/blob/v2.14.1/doc/distribution_linux.md)<br>
* Install OpenVINO™ Toolkit<br>
	* From OpenVINO™ Toolkit tar ball
		* [OpenVINO™ Toolkit](https://software.intel.com/en-us/openvino-toolkit) ([guide](https://software.intel.com/en-us/articles/OpenVINO-Install-Linux))<br>
    			**Note**: Please use  *root privileges* to run the installer when installing the core components.
		* Install OpenCL Driver for GPU
			```bash
			cd /opt/intel/computer_vision_sdk/install_dependencies
			sudo ./install_NEO_OCL_driver.sh
			```
	* From OpenVINO™ Toolkit Open Source
		* Install [OpenCV 3.3 or later](https://docs.opencv.org/master/d9/df8/tutorial_root.html)([guide](https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html))
			```bash
			[compiler] sudo apt-get install build-essential
			[required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
			[optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev 
			cd ~/code
			git clone https://github.com/opencv/opencv.git
			git clone https://github.com/opencv/opencv_contrib.git
			cd opencv && git checkout 3.3.1 && cd ..
			cd opencv_contrib && git checkout 3.3.1 && cd ..
			cd opencv
			mkdir build && cd build
			cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=/home/<hostname>/code/opencv_contrib/modules/ ..
			make -j8
			sudo make install
			```
			* Additional steps are required on ubuntu 18.04
				```bash
				sudo add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"
				sudo apt update
				sudo apt install libjasper1 libjasper-dev
				```
		* Install OpenCL Driver for GPU([guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/11396/intel-opencl-4.1-installation.pdf))<br>
			```bash
			cd ~/Downloads
			wget http://registrationcenter-download.intel.com/akdlm/irc_nas/11396/SRB5.0_linux64.zip
			unzip SRB5.0_linux64.zip -d SRB5.0_linux64
			cd SRB5.0_linux64
			sudo apt-get install xz-utils
			mkdir intel-opencl
			tar -C intel-opencl -Jxf intel-opencl-r5.0-63503.x86_64.tar.xz
			tar -C intel-opencl -Jxf intel-opencl-devel-r5.0-63503.x86_64.tar.xz
			tar -C intel-opencl -Jxf intel-opencl-cpu-r5.0-63503.x86_64.tar.xz
			sudo cp -R intel-opencl/* /
			sudo ldconfig
			```
		* Install [Deep Learning Deployment Toolkit](https://github.com/opencv/dldt)([guide](https://github.com/opencv/dldt/tree/2018/inference-engine))<br>
			```bash
			mkdir ~/code && cd ~/code
			git clone https://github.com/opencv/dldt.git
			cd dldt/inference-engine/
			./install_dependencies.sh
			mkdir build && cd build
			cmake -DCMAKE_BUILD_TYPE=Release ..
			make -j8
			```
		* Install [Open Model Zoo](https://github.com/opencv/open_model_zoo)([guide](https://github.com/opencv/open_model_zoo/tree/2018/demos))<br>
			```bash
			cd ~/code
			git clone https://github.com/opencv/open_model_zoo.git
			cd open_model_zoo/demos/
			mkdir build && cd build
			cmake -DCMAKE_BUILD_TYPE=Release <path_to_inference_engine_demos_directory>
			make -j8
			```
			**Note**:<path_to_inference_engine_demos_directory> is /home/\<hostname\>/code/dldt/inference-engine,After that you can find binaries for all demos applications in the intel64/Release subfolder.<br><br>

=======
- Install ROS2 [Bouncy](https://github.com/ros2/ros2/wiki) ([guide](https://github.com/ros2/ros2/wiki/Linux-Development-Setup))

- Install [OpenVINO™ Toolkit](https://software.intel.com/en-us/openvino-toolkit) ([guide](https://software.intel.com/en-us/articles/OpenVINO-Install-Linux))

	**Note**: Please use  *root privileges* to run the installer when installing the core components.
- Install OpenCL Driver for GPU
```bash
cd /opt/intel/computer_vision_sdk/install_dependencies
sudo ./install_NEO_OCL_driver.sh
```

- Install Intel® RealSense™ SDK 2.0 [(tag v2.14.1)](https://github.com/IntelRealSense/librealsense/tree/v2.14.1)
	* [Install from source code](https://github.com/IntelRealSense/librealsense/blob/v2.14.1/doc/installation.md)(Recommended)
	* [Install from package](https://github.com/IntelRealSense/librealsense/blob/v2.14.1/doc/distribution_linux.md)

>>>>>>> upstream/master
- Other Dependencies
	```bash
	# numpy
	pip3 install numpy
	```
	* Ubuntu 16.04
		```bash
		# libboost
		sudo apt-get install -y --no-install-recommends libboost-all-dev
		cd /usr/lib/x86_64-linux-gnu
		sudo ln -s libboost_python-py35.so libboost_python3.so
		```
	* Ubuntu 18.04
		```bash
		#libboost
		sudo apt-get install -y --no-install-recommends libboost-all-dev
		sudo apt install libboost-python1.62.0
		```
## 4. Building and Installation
<<<<<<< HEAD
* From OpenVINO™ Toolkit tar ball
	* Build sample code under openvino toolkit
        ```bash
        # root is required instead of sudo
        source /opt/intel/computer_vision_sdk/bin/setupvars.sh
        cd /opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples/
        mdkir build
        cd build
        cmake ..
        make
        ```
  	* set ENV CPU_EXTENSION_LIB and GFLAGS_LIB
        ```bash
        export CPU_EXTENSION_LIB=/opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples/build/intel64/Release            /lib/libcpu_extension.so
        export GFLAGS_LIB=/opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples/build/intel64/Release/lib/libgflags_nothreads.a
        ```
* From OpenVINO™ Toolkit Open Source
	* set ENV InferenceEngine_DIR, CPU_EXTENSION_LIB and GFLAGS_LIB
        ```bash
        export InferenceEngine_DIR=/home/<hostname>/code/dldt/inference-engine/build/
        export CPU_EXTENSION_LIB=/home/<hostname>/code/dldt/inference-engine/bin/intel64/Release/lib/libcpu_extension.so
        export GFLAGS_LIB=/home/<hostname>/code/dldt/inference-engine/bin/intel64/Release/lib/libgflags_nothreads.a
        ```
* Install ROS2_OpenVINO packages
	```bash
	mkdir -p ~/ros2_overlay_ws/src
	cd ~/ros2_overlay_ws/src
	git clone https://github.com/intel/ros2_openvino_toolkit
	git clone https://github.com/intel/ros2_object_msgs
	git clone https://github.com/ros-perception/vision_opencv -b ros2
	```

* Build package
	```
	source ~/ros2_ws/install/local_setup.bash
	cd ~/ros2_overlay_ws
	colcon build --symlink-install
	```
=======
- Build sample code under openvino toolkit
```bash
# root is required instead of sudo
source /opt/intel/computer_vision_sdk/bin/setupvars.sh
cd /opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples/
mkdir build
cd build
cmake ..
make
```

- Install ROS2_OpenVINO packages
```bash
mkdir -p ~/ros2_overlay_ws/src
cd ~/ros2_overlay_ws/src
git clone https://github.com/intel/ros2_openvino_toolkit
git clone https://github.com/intel/ros2_object_msgs
git clone https://github.com/ros-perception/vision_opencv -b ros2
```

- Build package
```
source ~/ros2_ws/install/local_setup.bash
source /opt/intel/computer_vision_sdk/bin/setupvars.sh
cd ~/ros2_overlay_ws
colcon build --symlink-install
```
>>>>>>> upstream/master

## 5. Running the Demo
* From OpenVINO™ Toolkit tar ball
	* Preparation
		*  copy label files (excute _once_)
			```bash
			sudo cp ~/ros2_overlay_ws/src/ros2_openvino_toolkit/data/labels/emotions-recognition/FP32/emotions-recognition-retail-0003.labels /opt/intel/computer_vision_sdk/deployment_tools/intel_models/emotions-recognition-retail-0003/FP32
			```
		* set OpenVINO toolkit ENV
			```bash
			source /opt/intel/computer_vision_sdk/bin/setupvars.sh
			```
		* set ENV LD_LIBRARY_PATH
			```bash
			export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples/build/intel64/Release/lib
			```
	* run sample code with command-line arguments for <b>image or video</b> detection
		```bash
		ros2 run dynamic_vino_sample dynamic_vino_sample -m <model_path_for_face_detection> -m_em <model_path_for_emotion_detection> -m_ag <model_path_for_age_and_gender_detection> -m-hp <model_path_for_headpose_detection> -i <input_type> -i_path <file_directory> -d <device> -d_em <device> -d_ag <device> -d_hp <device>
		```
	>Options for &lt;input_type&gt;: _Image_ or _Video_.<br>
	>Options for &lt;device&gt;: _CPU_ or _GPU_. Default is _CPU_. <br>
	
	For example,
	```bash
	ros2 run dynamic_vino_sample dynamic_vino_sample -m /opt/intel/computer_vision_sdk/deployment_tools/intel_models/face-detection-adas-0001/FP32/face-detection-adas-0001.xml -m_em /opt/intel/computer_vision_sdk/deployment_tools/intel_models/emotions-recognition-retail-0003/FP32/emotions-recognition-retail-0003.xml -m_ag /opt/intel/computer_vision_sdk/deployment_tools/intel_models/age-gender-recognition-retail-0013/FP32/age-gender-recognition-retail-0013.xml -m-hp /opt/intel/computer_vision_sdk/deployment_tools/intel_models/head-pose-estimation-adas-0001/FP32/head-pose-estimation-adas-0001.xml -i Image -i_path <file_directory> -d CPU -d_em CPU -d_ag GPU -d_hp GPU
	```

	* run sample code with command-line arguments for <b>video stream from camera</b> detection
		```bash
		ros2 run dynamic_vino_sample dynamic_vino_sample -m <model_path_for_face_detection> -m_em <model_path_for_emotion_detection> -m_ag <model_path_for_age_and_gender_detection> -m-hp <model_path_for_headpose_detection> -i <input_type> -d <device> -d_em <device> -d_ag <device> -d_hp <device>
		```

	>Options for &lt;input_type&gt;: _StandardCamera_ or _RealSenseCamera_. Default is _StandardCamera_.<br>
	>Options for &lt;device&gt;: _CPU_ or _GPU_. Default is _CPU_. <br>
    
	For example, 
	```bash
	ros2 run dynamic_vino_sample dynamic_vino_sample -m /opt/intel/computer_vision_sdk/deployment_tools/intel_models/face-detection-adas-0001/FP32/face-detection-adas-0001.xml -m_em /opt/intel/computer_vision_sdk/deployment_tools/intel_models/emotions-recognition-retail-0003/FP32/emotions-recognition-retail-0003.xml -m_ag /opt/intel/computer_vision_sdk/deployment_tools/intel_models/age-gender-recognition-retail-0013/FP32/age-gender-recognition-retail-0013.xml -m-hp /opt/intel/computer_vision_sdk/deployment_tools/intel_models/head-pose-estimation-adas-0001/FP32/head-pose-estimation-adas-0001.xml -i StandardCamera -d CPU -d_em CPU -d_ag GPU -d_hp GPU
	```
	* run sample code with parameters extracted from [yaml](https://github.com/intel/ros2_openvino_toolkit/blob/master/vino_param_lib/param/pipeline.yaml).
		```bash
		ros2 run dynamic_vino_sample pipeline_with_params
		```
* From OpenVINO™ Toolkit Open Source
	* Preparation
		* download model file<br>
        	```bash
        	cd ~/code/open_model_zoo/model_downloader
        	python3 downloader.py --name face-detection-adas-0001
        	python3 downloader.py --name age-gender-recognition-retail-0013
        	python3 downloader.py --name emotions-recognition-retail-0003
        	python3 downloader.py --name head-pose-estimation-adas-0001
        	```
		* copy label files (excute _once_)<br>
			```bash
			sudo cp ~/ros2_overlay_ws/src/ros2_openvino_toolkit/data/labels/emotions-recognition/FP32/emotions-recognition-retail-0003.labels /home/<hostname>/code/open_model_zoo/model_downloader/Retail/object_attributes/emotions_recognition/0003/dldt
			```
		* set ENV LD_LIBRARY_PATH<br>
			```bash
			export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/<hostname>/code/dldt/inference-engine/bin/intel64/Release/lib
			```
	* run sample code with command-line arguments for <b>image or video</b> detection
		```bash
		ros2 run dynamic_vino_sample dynamic_vino_sample -m <model_path_for_face_detection> -m_em <model_path_for_emotion_detection> -m_ag <model_path_for_age_and_gender_detection> -m-hp <model_path_for_headpose_detection> -i <input_type> -i_path <file_directory> -d <device> -d_em <device> -d_ag <device> -d_hp <device>
		```
	>Options for &lt;input_type&gt;: _Image_ or _Video_.<br>
	>Options for &lt;device&gt;: _CPU_ or _GPU_. Default is _CPU_. <br>
	
	For example,
	```bash
	ros2 run dynamic_vino_sample dynamic_vino_sample -m /home/<hostname>/code/open_model_zoo/model_downloader/Transportation/object_detection/face/pruned_mobilenet_reduced_ssd_shared_weights/dldt/face-detection-adas-0001.xml -m_hp /home/<hostname>/code/open_model_zoo/model_downloader/Transportation/object_attributes/headpose/vanilla_cnn/dldt/head-pose-estimation-adas-0001.xml -m_em /home/<hostname>/code/open_model_zoo/model_downloader/Retail/object_attributes/emotions_recognition/0003/dldt/emotions-recognition-retail-0003.xml -m_ag /home/<hostname>/code/open_model_zoo/model_downloader/Retail/object_attributes/age_gender/dldt/age-gender-recognition-retail-0013.xml -i Image -i_path <file_directory> -d CPU -d_hp CPU -d_em CPU -d_ag CPU
	```
	* run sample code with command-line arguments for <b>video stream from camera</b> detection
		```bash
		ros2 run dynamic_vino_sample dynamic_vino_sample -m <model_path_for_face_detection> -m_em <model_path_for_emotion_detection> -m_ag <model_path_for_age_and_gender_detection> -m-hp <model_path_for_headpose_detection> -i <input_type> -d <device> -d_em <device> -d_ag <device> -d_hp <device>
		```

	>Options for &lt;input_type&gt;: _StandardCamera_ or _RealSenseCamera_. Default is _StandardCamera_.<br>
	>Options for &lt;device&gt;: _CPU_ or _GPU_. Default is _CPU_.<br>
    
<<<<<<< HEAD
	For example, 
	```bash
	ros2 run dynamic_vino_sample dynamic_vino_sample -m /home/<hostname>/code/open_model_zoo/model_downloader/Transportation/object_detection/face/pruned_mobilenet_reduced_ssd_shared_weights/dldt/face-detection-adas-0001.xml -m_hp /home/<hostname>/code/open_model_zoo/model_downloader/Transportation/object_attributes/headpose/vanilla_cnn/dldt/head-pose-estimation-adas-0001.xml -m_em /home/<hostname>/code/open_model_zoo/model_downloader/Retail/object_attributes/emotions_recognition/0003/dldt/emotions-recognition-retail-0003.xml -m_ag /home/<hostname>/code/open_model_zoo/model_downloader/Retail/object_attributes/age_gender/dldt/age-gender-recognition-retail-0013.xml -i StandardCamera -d CPU -d_hp CPU -d_em CPU -d_ag CPU
	```
=======
For example, 
```bash
ros2 run dynamic_vino_sample dynamic_vino_sample -m /opt/intel/computer_vision_sdk/deployment_tools/intel_models/face-detection-adas-0001/FP32/face-detection-adas-0001.xml -m_em /opt/intel/computer_vision_sdk/deployment_tools/intel_models/emotions-recognition-retail-0003/FP32/emotions-recognition-retail-0003.xml -m_ag /opt/intel/computer_vision_sdk/deployment_tools/intel_models/age-gender-recognition-retail-0013/FP32/age-gender-recognition-retail-0013.xml -m-hp /opt/intel/computer_vision_sdk/deployment_tools/intel_models/head-pose-estimation-adas-0001/FP32/head-pose-estimation-adas-0001.xml -i StandardCamera -d CPU -d_em CPU -d_ag GPU -d_hp GPU
```
4. run sample code with parameters extracted from [yaml](https://github.com/intel/ros2_openvino_toolkit/blob/master/sample/param/pipeline_people.yaml).
```bash
ros2 run dynamic_vino_sample pipeline_with_params
```
5. run object detection sample code with paramters extracted from [yaml](https://github.com/intel/ros2_openvino_toolkit/blob/master/sample/param/pipeline_object.yaml).
```bash
ros2 run dynamic_vino_sample object_detection_with_params
```

>>>>>>> upstream/master
## 6. Interfaces
### 6.1 Topic
- Face Detection:
```/openvino_toolkit/faces```([object_msgs:msg:ObjectsInBoxes](https://github.com/intel/ros2_object_msgs/blob/master/msg/ObjectsInBoxes.msg))
- Emotion Detection:
```/openvino_toolkit/emotions```([people_msgs:msg:EmotionsStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/EmotionsStamped.msg))
- Age and Gender Detection:
```/openvino_toolkit/age_genders```([people_msgs:msg:AgeGenderStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/AgeGenderStamped.msg))
- Head Pose:
```/openvino_toolkit/headposes```([people_msgs:msg:HeadPoseStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/HeadPoseStamped.msg))

## 7. Known Issues
- Parameters "-m_ag, -m_hp, -m_em" should be optional, but samples throw exception without them.
- Parameters "-n_ag, -n_hp, -n_em" doesn't work. The maximum number of face/age/headpose/emotion is always 16.
- Standard USB camera can be unexpected launched with input parameter "-i RealSenseCamera". 

###### *Any security issue should be reported using process at https://01.org/security*
