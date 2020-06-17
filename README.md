# DNNTuplesAK8

## Setup
```
cmsrel CMSSW_10_2_11
cd CMSSW_10_2_11/src/
cmsenv
git clone https://github.com/DeepDoubleB/DNNTuplesAK8 DeepNTuples -b master
scram b -j8
```

## Submit jobs via CRAB

```bash
# set up CRAB env; run it after cmsenv
source /cvmfs/cms.cern.ch/crab3/crab.sh
voms-proxy-init -rfc -voms cms --valid 168:00

# create the CRAB config files
cd DeepNTuples/NtupleAK8/run
./makeCrabJobs.py -i [samples/ttbar.conf] -o [/eos/cms/store/user/$USER/DeepNtuples/output_dir] --site [T2_CH_CERN|T3_US_FNALLPC|...]
# submit jobs
./submit_[ttbar].conf
```

To check all the options of the submission script, run
```
./makeCrabJobs.py -h
```

You can set arguments to be passed to the cmsRun job in the beginning of the sample.conf file, e.g.,
~~fjKeepFlavors=2,3,4   # only keep fatjets whose labels match to the given number. 0:Light, 1:Top, 2:W, 3:Z, 4:Higgs~~
 
## Merge outputs (with random mixing of different samples)

1. Create file lists.

Appropriately modify
```bash
python Utilities/scripts/makeRemoteSampleLists.py
python Utilities/scripts/makeLocalSampleLists.py
```

2. Merge the samples (with random mixing)

```bash
mergeSamples.py [events per output file] [output dir] [path to the filelist produced in step 1]
```
e.g.,
```
mergeSamples.py 200000 /eos/uscms/store/group/lpchbb/deepdoubleb/phi_all /eos/uscms/store/group/lpchbb/20170717_ak8/QCD_Pt/qcd_*.txt /eos/uscms/store/group/lpchbb/20170717_ak8/BulkGrav/hbb_*.txt /eos/uscms/store/group/lpchbb/20170717_ak8/Phi/scalarpseudo_*.txt
``` 
