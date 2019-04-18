1. Building </br>
**Note**:do some modifications in SConscript, add -std=c++11 option when compiling with gcc. refer to https://github.com/RachelRen05/Daily_Notes/blob/master/dps/SConscript
```bash
git clone https://github.com/intel/dps-for-iot.git
cd ~/dps-for-iot
scons variant=release transport=udp bindings=python
```

2. Running
```bash
export PYTHONPATH=~/dps-for-iot/build/dist/py
python simple_pub.py
python simple_sub.py
```
