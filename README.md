# SpaNET_Training - Training SpaNET with HHH->6b data

### Instructions:

### Step 1.
Login to LXPLUS-GPU with command `ssh -Y <username>@lxlpus-gpu.cern.ch`, then create and activate `virtual environment` for the work.
```
pip3 install virtualenv --user
virtualenv spanet-work
source spanet-work/bin/activate
```
Install PyTorch and Jupyter Notebook
```
pip3 install torch==1.10.0+cu113 torchvision==0.11.1+cu113 torchaudio==0.10.0+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html
pip3 install jupyterlab matplotlib scikit-hep
```

### Step 2.
Clone GitHub repository  
```
git clone https://github.com/Joxy97/SpaNET_Training.git
cd SpaNET_Training
```
**For our training data**
Download and add ['hhh_training.h5'](https://drive.google.com/file/d/19gxW4YMMTjQaqk8_h-R1VIs5WzUOwTWj/view?usp=share_link) and ['hhh_testing.h5'](https://drive.google.com/file/d/1J_MjkpgUgWeFQdlezKosKY7LD5H-rsSm/view?usp=share_link) into `./data`

**For your training data**
Add your .root file into `./data` then type
```
python3 src/data/cms/convert_to_h5.py data/<your_data>.root --out-file data/hhh_training.h5
python3 src/data/cms/convert_to_h5.py data/<your_data>.root --out-file data/hhh_testing.h5
```

### Step 3.
Install all required packages inside project directory
```
pip3 install -e .
pip3 install tensorboard
```
_NOTE:_ If on LXPLUS, you might need to use `pip3 install -e . --user`

### Step 4.
Choose settings by editing `training_settings.json` file in `./options_files/cms` folder. Most importantly, choose number of epochs and number of gpus.

### Step 5.
Run the training using command
```
python3 -m spanet.train -of options_files/cms/training_settings.json
```  

### Step 6.
Our current output folder is [`150_epochs_test`](https://drive.google.com/drive/folders/155y3r4fgdosE2inNTyNue4bY85FNI5Rb?usp=share_link). Output log will by default be inside `spanet_output/<version_(x)>`, where x is the ordinal number of training run (check the latest folder if not sure). To evaluate the training, type command
```
python3 -m spanet.test spanet_output/150_epochs_test -tf data/hhh_testing.h5  --gpu
```

### Step 7.
Evaluate the baseline method with
```
python3 -m src.models.test_baseline --test-file data/hhh_testing.h5
```
