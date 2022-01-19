
# musrsim-sms
Geant4 package for musrSim (shanghai muon source dedicated)

# Tutorial

### Setup environment
This package based on Geant4 version `10.7.2`.

On `INPAC-cluster`, you can setup Geant4 enviroment with:

```
source  /cvmfs/sft.cern.ch/lcg/views/LCG_101/x86_64-centos7-gcc11-opt/setup.sh
```
### Download and compile the package

```
git clone https://github.com/kimsiang/musrsim-sms.git
cd musrsim-sms
mkdir build
cd build
make -j4
```
### Create working directory

```
mkdir run
cd run
cp ../../run/1000_Laser.mac ../../run/visVRML.mac .
```
### Start simulation

```
../musrsim 1000_Laser.mac test_run
```
### Run on condor
First copy condor scripts
```
cp ../../run/submit.condor ../../run/run.sh .
```
Then submit 
```
condor_submit submit.condor
```


# Updates
### 2021-11-30 (CC)
Add random seed offset, only take effect with `/musr/run/randomOption 1` in mac file.
With `randomOption=1`, Geant4 will take the current system time as the random seed. If there are mult-jobs submitted simultaneously, different jobs could have a same system time since have a same random seed. To avoid this, the random seed should be offset by a number.

Example:
```
../musrSim 1003.mac name 10
```
In this case, this random seed of this job will be offset by 10.
In practice, we submit me jobs with `condor`, each job will be offset by their job process number.


### 2021-11-25 (ML)
Enables the customization of output file name in `.mac` steering file

Example:
```
# set output file name
/musr/command SetOutputFileName myRootFile
```
And the output root file will be: `musrSim_myRootFile.root`

Moreover, you can specify the name as DEFAULT:
```
/musr/command SetOutputFileName DEFAULT
```
This is equivalent to running the macro without this command line


### 2021-11-17 (CC)
Enabled the customizing of crosssection factors on mac steering file, the default value is set to 1.0 if not specified

Example:
```
# set gmumu xsection factor to 1000.0
/musr/command G4EmExtraPhysics SetCrossSecFactor gmumuFactor 1000.0
```

Now can specify a name when launch the job:
```
../musrSim 1003.mac name
```
The output file will be `musr_1003_name.root`.

