1. Prerequisites
```bash
git clone https://github.com/intel/dps-for-iot.git
```
2. Building
  
  **Note**:do some modifications in SConscript, refer to https://github.com/RachelRen05/Daily_Notes/blob/master/dps/SConscript
```bash
cd ~/dps-for-iot
scons variant=release transport=udp bindings=python
```
3. Running
```bash
export PYTHONPATH=~/dps-for-iot/build/dist/py
python simple_pub.py
python simple_sub.py
```
