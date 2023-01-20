# SpaNET_Training - Training SpaNET with HHH->6b data

### Instructions:

### Step 1.
Choose or make your working folder with `mkdir <your_workspace>`, then `cd <your_workspace>`, and inside...  
**If on a local machine** skip to Step 2.  
**If on LXPLUS** create _CMS Environment_ using `cmsrel` 
```
cmsrel CMSSW_12_5_2  
cd CMSSW_12_5_2/src  
cmsenv
```
_NOTE:_ Each time you open the terminal on LXPLUS, go to `CMSSW_12_5_2` and run `cmsenv`  

### Step 2.
Clone GitHub repository  
```
git clone git@github.com:Joxy97/SpaNET_Training.git  
cd SpaNET_Training-main
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
```
_NOTE:_ If on LXPLUS use 'pip3 install -e . --user'

### Step 4.
Choose settings by editing `hhh_v12.json` or `hhh_v20.json` files in `./options_files/cms` folder. Most importantly, choose number of epochs and number of gpus.

### Step 5.
Run the training using command
```
python3 -m spanet.train -of options_files/cms/hhh_v12.json
```  
or
```
python3 -m spanet.train -of options_files/cms/hhh_v20.json
```

### Step 6.
Output log will by default be inside `spanet_output/<version_(x)>`, where x is the ordinal number of training run (check the latest folder if not sure). To evaluate the training, type command
```
python3 -m spanet.test spanet_output/<version_(x)> -tf data/hhh_testing.h5
```

### Step 7.
Evaluate the baseline method with
```
python3 -m src.models.test_baseline --test-file data/hhh_testing.h5
```
