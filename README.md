# Detecting Shifts in Natural Selection with Branch-Specific Substitution Models and Shrinkage Priors
This repository contains the instructions and files to reproduce the analyses performed in the "Detecting Shifts in Natural Selection with Branch-Specific Substitution Models and Shrinkage Priors" paper by Ji et al.

We provide two ways for users to setup software to reproduce our results.

1. Docker image

	Please kindly refer to the section `Docker image` if you'd like to run BEAST within docker container which is local and should not disturb your other BEAST installations.

2. Global installation

	Please kindly refer to the section `Global installation` if you'd like to install BEAST and BEAGLE gloably which could potentially interfere with your other BEAST installations.

You could find a separate `Reproducing the analyses` section under each installation method to follow.

### Docker image
We provide a docker image to streamline software setup.
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

#### Reproducing the analyses

You may use the following commands for each case of the two data sets as described in the manuscript.

Let's open an interactive bash session under docker container.

```
docker run --mount type=bind,src="$(pwd)",target=/tmp -it beast bash
```

To run each of the analysis, please use the corresponding command below in the interactive bash shell.

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
But get the `hmc-clock` branch.

For Mac users, the following commands will compile the CPU version of BEAGLE.
Follow the [instructions](https://github.com/beagle-dev/beagle-lib) if you need to install any other dependent software.

```
xcode-select --install
brew install libtool autoconf automake
git clone -b hmc-clock https://github.com/beagle-dev/beagle-lib.git
git checkout f3dffd952d10b949a807d0ec03abfcc110cac02a
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
git checkout f3dffd952d10b949a807d0ec03abfcc110cac02a
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
git checkout d6cefb0b9b985f49d121317e4c1c668faf0c5c46
ant
```

For Mac users, you may need to install ant by `brew install ant` through [Homebrew](https://brew.sh/).

For Linux users, you can install ant by `sudo apt-get install ant`.

This will compile the `jar` files under `beast-mcmc/build/dist/` where you can find `beast.jar`, `beauti.jar` and `trace.jar`.

#### Reproducing the analyses

You may use the following commands for each case of the three data sets as described in the manuscript.

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

