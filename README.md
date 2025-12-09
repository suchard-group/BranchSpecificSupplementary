# Detecting Evolutionary Change-Points with Branch-Specific Substitution Models and Shrinkage Priors
This repository contains the instructions and files to reproduce the analyses performed in the "Detecting Evolutionary Change-Points with Branch-Specific Substitution Models and Shrinkage Priors" paper by Ji et al.

We provide two ways for users to setup software to reproduce our results.

1. Docker image

	Please kindly refer to the section `Docker image` if you'd like to run BEAST within a docker container which is local and should not disturb your other BEAST installations.

2. Global installation

	Please kindly refer to the section `Global installation` if you'd like to install BEAST and BEAGLE globaly which could potentially interfere with your other BEAST installations.

You can find a separate `Reproducing the analyses` section under each installation method to follow.

### Docker image
We provide a docker image to streamline software setup.
You can either use the provided image directly, or build it from the docker file.


#### Load pre-built docker image from tar file

For `x86_64` computer systems (e.g., Intel/AMD chips), please load the x86 docker image by

```
cd where_this_repository_is_stored
docker load -i beast_x86.tar
```
and start an interactive bash session by
```
docker run --mount type=bind,src="$(pwd)",target=/tmp -it beast_x86 bash
```


For `arm_64` computer systems (e.g., Apple silicon chips), please load the arm docker image (e.g., in a Docker Desktop command line environment) by

```
cd where_this_repository_is_stored
docker load -i beast_arm.tar
```
and start an interactive bash session by
```
docker run --mount type=bind,src="$(pwd)",target=/tmp -it beast_arm bash
```

You can now jump to the "Reproducing the analyses" section (i.e., skipping the next section on building these docker images).


#### Build image from dockerfile

To build the docker image for `x86_64` systems (e.g., Intel/AMD chips), please run (e.g., in a terminal window)

```
cd where_this_repository_is_stored
docker build --platform x86_64 -t beast docker
```

To build the docker image for `arm_64` systems (e.g., Apple silicon chips), please run (e.g., in a Docker Desktop command line environment)

```
cd where_this_repository_is_stored
docker build --platform ARM64 -t beast docker
```

You can now open an interactive bash session under docker container.

```
docker run --mount type=bind,src="$(pwd)",target=/tmp -it beast bash
```

#### Reproducing the analyses

You can use the following commands to reproduce the two data examples as described in the manuscript.



To run each of the analyses, please use the corresponding command below in the interactive bash shell.

#### BRCA1

* Benchmarking
	* HMC

	```
	beast -load_state xmls/BRCA1/BRCA1_hmc_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/BRCA1/BRCA1_serial_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_Serial.xml
	```

* Optimization
	* Analytic

	```
	beast -load_state xmls/BRCA1/BRCA1_hmc_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_MLE_analytic.xml
	```

	* Numeric

	```
	beast -load_state xmls/BRCA1/BRCA1_hmc_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_MLE_numeric.xml	```

* Full analysis

	```
	beast -seed 666 -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates.xml
	```


#### MPXV

* Benchmarking
	* HMC

	```
	beast -load_state xmls/MPXV/apobec_HMC_save -overwrite xmls/MPXV/apobec.shrinkage_HMC.xml
	```

	* Univariable

	```
	beast -load_state xmls/MPXV/apobec_Serial_save -overwrite xmls/MPXV/apobec.shrinkage_Serial.xml
	```

* Optimization
	* Analytic

	```
	beast -load_state xmls/MPXV/apobec_HMC_save -overwrite xmls/MPXV/apobec.shrinkage_MLE_analytic.xml
	```

	* Numeric

	```
	beast -load_state xmls/MPXV/apobec_HMC_save -overwrite xmls/MPXV/apobec.shrinkage_MLE_numeric.xml	```

* Full analysis

	```
	beast -seed 666 -overwrite xmls/MPXV/apobec.shrinkage.xml
	```



### Global installations

#### Setting up BEAGLE

Please follow the [BEAGLE installation instructions](https://github.com/beagle-dev/beagle-lib).
But check-out the `hmc-clock` branch.

For Mac users, the following commands will compile the CPU version of BEAGLE.
Follow the [instructions](https://github.com/beagle-dev/beagle-lib) if you need to install any other dependent software.

```
xcode-select --install
brew install libtool autoconf automake
git clone -b hmc-clock https://github.com/beagle-dev/beagle-lib.git
git checkout edfb106eb12efae798945ed3dd0a2f918e8c1a28
cd beagle-lib
mkdir build
cd build
cmake -DBUILD_CUDA=OFF -DBUILD_OPENCL=OFF ..
sudo make install
```


For Linux users, the commands are similar.

```
sudo apt-get install build-essential autoconf automake libtool git pkg-config openjdk-9-jdk
git clone -b hmc-clock https://github.com/beagle-dev/beagle-lib.git
git checkout edfb106eb12efae798945ed3dd0a2f918e8c1a28
cd beagle-lib
mkdir build
cd build
cmake -DBUILD_CUDA=OFF -DBUILD_OPENCL=OFF ..
sudo make install
```


The libraries are installed into `/usr/local/lib`.
You can find them by `ls /usr/local/lib/*beagle*`.


#### Setting up BEAST

The following commands will compile the `hmc-clock` branch of BEAST.

```
git clone -b hmc-clock https://github.com/beast-dev/beast-mcmc.git
cd beast-mcmc
git checkout 215bf75e51d465b1f56153836a43c0ebdbd8890e
ant
```

For Mac users, you may need to install `ant` using `brew install ant` through [Homebrew](https://brew.sh/).

For Linux users, you can install `ant` using `sudo apt-get install ant`.

This will compile the `jar` files under `beast-mcmc/build/dist/` where you can find `beast.jar`, `beauti.jar` and `trace.jar`.

#### Reproducing the analyses

You can use the following commands for each of the three data examples as described in the manuscript.

Change your working directory to where you want to store the resulting log files first.

```
cd where_you_want_to_save_results
```


#### BRCA1

* Benchmarking
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/BRCA1/BRCA1_hmc_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/BRCA1/BRCA1_serial_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_Serial.xml
	```

* Optimization
	* Analytic

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/BRCA1/BRCA1_hmc_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_MLE_analytic.xml
	```

	* Numeric

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/BRCA1/BRCA1_hmc_save -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates_MLE_numeric.xml	```

* Full analysis

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -seed 666 -overwrite xmls/BRCA1/BRCA1_shrinkage_fixedRates.xml
	```


#### MPXV

* Benchmarking
	* HMC

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/MPXV/apobec_HMC_save -overwrite xmls/MPXV/apobec.shrinkage_HMC.xml
	```

	* Univariable

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/MPXV/apobec_Serial_save -overwrite xmls/MPXV/apobec.shrinkage_Serial.xml
	```

* Optimization
	* Analytic

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/MPXV/apobec_HMC_save -overwrite xmls/MPXV/apobec.shrinkage_MLE_analytic.xml
	```

	* Numeric

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -force_resume -load_state xmls/MPXV/apobec_HMC_save -overwrite xmls/MPXV/apobec.shrinkage_MLE_numeric.xml	```

* Full analysis

	```
	java -jar -Djava.library.path=/usr/local/lib where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -seed 666 -overwrite xmls/MPXV/apobec.shrinkage.xml
	```

