# Testing

## Unit testing framework

SPM testing is performed using the [MATLAB Unit Testing Framework](https://www.mathworks.com/help/matlab/matlab-unit-test-framework.html).

SPM unit tests are stored in the [`spm/tests`](https://github.com/spm/spm/tree/main/tests) directory. The can be executed by calling the [`spm_tests`](https://github.com/spm/spm/blob/main/spm_tests.m) function. Individual tests can be run as follow:

```matlab
>> spm_tests test spm_Ncdf                     

SPM12: spm_tests                                   12:00:00 - 01/01/2023
========================================================================
Running test_spm_Ncdf
.......
Done test_spm_Ncdf
__________

Totals:
   7 Passed, 0 Failed, 0 Incomplete.
   0.016786 seconds testing time.
```

Some tests require input data that can be downloaded from [here](https://www.fil.ion.ucl.ac.uk/spm/download/data/tests/tests_data.zip). This archive has to be unpacked into a `spm/tests/data` directory before running the tests.

## Continuous integration

[Continuous integration on GitHub](https://www.mathworks.com/help/matlab/matlab_prog/continuous-integration-with-matlab-on-ci-platforms.html#mw_6cb5114e-198f-48b2-9d94-e1efd7bf653c) takes place with [MATLAB GitHub Actions](https://github.com/matlab-actions). The configuration file for the testing workflow is available [here](https://github.com/spm/spm/blob/main/.github/workflows/matlab.yml) and the outcomes are available [from the GitHub Actions tag](https://github.com/spm/spm/actions).

## Adding unit tests

To add new unit tests to SPM, please follow the instructions from the [MATLAB documentation](https://www.mathworks.com/help/matlab/matlab-unit-test-framework.html). [Function-based unit tests](https://www.mathworks.com/help/matlab/function-based-unit-tests.html) are favoured for now.

To write unit tests for SPM function `spm_example.m`, create a file `spm/tests/test_spm_example.m` containing:

```matlab
function tests = test_spm_example
% Unit Tests for spm_example
%__________________________________________________________________________

% Copyright (C) 2023 Wellcome Centre for Human Neuroimaging


tests = functiontests(localfunctions);


function test_spm_example_1(testCase)
exp = pi;
act = 3.1415926535897;
tol = 1e-12;
testCase.verifyEqual(act, exp,'AbsTol',tol);

function test_spm_example_2(testCase)

```

## Regression tests

Beyond unit tests, regression tests are also performed on the SPM codebase (to be documented).
