# Testing
Testing is the process by which we check if our code is valid and robust. It allows developers to confidently make changes to SPM and then run the automated tests to check that their code does not break other aspects of SPM. We test on Windows, MAC and Linux. We test on Matlab releases as far back as 2020. The configuration file for the automatic testing workflow is available [in the Github repo](https://github.com/spm/spm/blob/main/.github/workflows/matlab.yml). It is not necessary to understand this file to contribute tests to SPM or run them locally on your machine. The outcomes are available from the [GitHub Actions tab](https://github.com/spm/spm/actions).

Their are two types of tests used in SPM. These are unit tests and regression tests. Unit tests are ideally short code segments that test the validity of a function. In short, does the code do what it is supposed to do? Regression tests are tests that determine if the changes in the code cause a change in the results. They do not necessarily test validity, only code stability. 


## Running tests locally
The testing framework runs unit tests every day and regression tests once a week automatically on GitHub. However, you can also run the code locally on your machine before contributing code. Some of the tests require input data that can be downloaded from [here](https://www.fil.ion.ucl.ac.uk/spm/download/data/tests/tests_data.zip). This archive has to be unpacked into a `spm/tests/data` directory before running the tests.

Once the data is downloaded the all the unit tests can be run with the following code snippet.

```matlab
spm_tests('class','unit')
```

All of the regression tests can be run with the following code snippet.

```matlab
spm_tests('class','regression')
```

A specific unit test can be run by supplying the function with the test name.
```matlab
spm_tests('class','unit','test','test_spm_opm_hfc')
```

A specific regression test can be run by supplying the function with the test name.

```matlab
spm_tests('class','regression','test','test_regress_spm_opm')
```


## Contributing Tests 
SPM testing is performed using the [MATLAB Unit Testing Framework](https://www.mathworks.com/help/matlab/matlab-unit-test-framework.html).

SPM tests are stored in the [`spm/tests`](https://github.com/spm/spm/tree/main/tests) directory. Please place any tests you write here. We'll now look at an example of a unit test to test the validity of the `spm_eeg_filter` function. The first thing to do is is write a small bit of code that will run all the tests in a given `.m` file.

```matlab
function tests = test_spm_eeg_filter
% Unit Tests for spm_eeg_filter
%__________________________________________________________________________

% Copyright (C) 2023 Wellcome Centre for Human Neuroimaging


tests = functiontests(localfunctions);
```
After this section is written we can now write multiple functions in the same file to test different aspects of the code. The first thing to note is that the test function has the same name as the top function but is suffixed with an underscore and a number(e.g. `_1`). For multiple tests of the same function just simply change the number of the suffix. In the example below we just have 1. if you have placed the test data in the appropriate folder you'll be able to access it with the code below. The data in this case is an empty M/EEE dataset of 1 second duration and 110 channels. 


```matlab
function test_spm_eeg_filter_1(testCase)

spm('defaults','eeg');

fname = fullfile(spm('Dir'),'tests','data','OPM','test_opm.mat');
D     = spm_eeg_load(fname);
D = chantype(D,1:110,'MEG');
D.save();

% create 40Hz sinusoid
test = repmat(sin(2*pi*40*(0:.001:.999)),110,1);
D(:,:,1)= test;
D.save();

% try and remove sinusoid 
S=[];
S.D=D;
S.band='low';
S.freq=4;
fD = spm_eeg_filter(S);

% check that amplitude reduced by at least 100dB
Y = mean(std(D(:,900:1000,1),[],2));
res = mean(std(fD(:,900:1000,1),[],2));

dB = 20*log10(mean(Y./res));

testCase.verifyTrue(dB>100);

```
In the example above we set the channels to 'MEG' channels and then insert a 40Hz sinusoid. We then apply a low pass filter and verify that the magnitude of the signal is reduced by at least 100dB at 40Hz. Once you establish your criteria for assessing the validity of the function you can end the function with `testCase.verifyTrue(condition)`.

## Contributing Regression Tests 
Regression tests can be contributed in the exact same way as unit tests but we prefix the test with `test_regress`. See the `test_regress_spm_opm` as an example.

## A final note
Writing tests is difficult and sometimes it might seem easier to write a test based on existing data that runs for a few minutes. However, as we run the tests on many versions of MATLAB and different operating systems your 3-minute test may take 1 hour  to run across these permutations. As such, try and keep tests to a few seconds in length. regression tests can be a bit longer as there are less of them. If you are concerned about your test or test length, reach out to someone in the methods team.
