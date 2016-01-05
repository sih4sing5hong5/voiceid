# VoiceID
VoiceID is a speaker recognition/identification system written in Python,
based on the LIUM Speaker Diarization <http://lium3.univ-lemans.fr/diarization/doku.php> framework.

VoiceID can process video or audio files to identify in which slices of 
time there is a person speaking (diarization); then it examines all those
segments to identify who is speaking. To do so uses a voice models
database.

## Install
```bash
sudo apt-get install python2.7 python-wxgtk2.8 openjdk-7-jdk gstreamer0.10-plugins-base gstreamer0.10-plugins-good gstreamer0.10-plugins-bad gstreamer0.10-plugins-ugly gstreamer-tools sox mplayer python-setuptools python-virtualenv
virtualenv venv2
source venv2/bin/activate
pip install MplayerCtrl
python setup.py install
cd venv2/local/
ln -s ../share/ 
```

## Usage

### Get the Gender of a Wav
add a `example.py`
```python
from voiceid.sr import Voiceid
from voiceid.db import GMMVoiceDB
db = GMMVoiceDB('empty-dir')

v = Voiceid(db, 'wav/01.wav', single=True)
v.extract_speakers()

cluster = v.get_cluster('S0')
print cluster.get_gender()
```
and execute it by `python example.py`

### Split a Wav and Get the Gender
add a `example2.py`
```python
from voiceid.sr import Voiceid
from voiceid.db import GMMVoiceDB
db = GMMVoiceDB('empty-dir')

v = Voiceid(db, 'wav/01.wav')
v.extract_speakers()

for c in v.get_clusters():
    cluster = v.get_cluster(c)
    print cluster
    print cluster.wave
    cluster.print_segments()
    print cluster.to_dict()
    print cluster.get_gender()
    print
```
and execute it by `python example2.py`


## Development
```bash
source venv2/bin/activate
cd src/
python -m unittest tests.test_sr
python -m unittest tests.test_fm
```
