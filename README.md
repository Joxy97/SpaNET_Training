# SpaNET_Training - Training SpaNET with HHH->6b data

### Instructions:

#### Step 1.
**If on a local machine** skip to Step 2.  
**If on LXPLUS** choose your working folder and create _CMS Environment_ using 'cmsrel' 
```
cmsrel CMSSW_12_5_2  
cd CMSSW_12_5_2/src  
cmsenv
```
_NOTE:_ Each time you open the terminal, go to 'CMSSW_12_5_2/src' and run 'cmsenv'  

#### Step 2.
Clone GitHub repository  
```
git clone git@github.com:Joxy97/SpaNET_Training.git  
cd SpaNET_Training
```
Download and add 'hhh_training.h5' and 'hhh_testing.h5' into './data'

#### Step 3.
Install all required packages inside project directory
```
pip3 install -e .
```
_NOTE:_ If on LXPLUS use 'pip3 install -e . --user'

#### Step 3.5 (for custom training data)
Add your .root file into './data' and overwrite 'hhh_training.h5' and 'hhh_testing.h5' with commands
```
python3 src/data/cms/convert_to_h5.py data/<your_data>.root --out-file data/hhh_training.h5
python3 src/data/cms/convert_to_h5.py data/<your_data>.root --out-file data/hhh_testing.h5
```

#### Step 4.
Choose setting by editing 'hhh_v12.json' or 'hhh_v20.json' files in './options_files/cms' folder. Most importantly, choose number of epochs and number of gpus.

#### Step 5.
Run the training using command
```
python3 -m spanet.train -of options_files/cms/hhh_v12.json
```  
or
```
python3 -m spanet.train -of options_files/cms/hhh_v20.json
```
