# download model
```bash
intel@intel-desktop:/opt/intel/computer_vision_sdk/deployment_tools/model_downloader$ sudo python3 downloader.py --name ssd300
```

# optimize model
```bash
intel@intel-desktop:/opt/intel/computer_vision_sdk/deployment_tools/model_downloader/object_detection/common/ssd/300/caffe$  sudo python3 /opt/intel/computer_vision_sdk_2018.4.420/deployment_tools/model_optimizer/mo.py --input_model /opt/intel/computer_vision_sdk/deployment_tools/model_downloader/object_detection/common/ssd/300/caffe/ssd300.caffemodel --output_dir /opt/intel/computer_vision_sdk/deployment_tools/model_downloader/object_detection/common/ssd/300/caffe/output/
```

# check model
```bash
cd /opt/intel/computer_vision_sdk/deployment_tools/model_downloader/object_detection/common/ssd/300/caffe/output
```
