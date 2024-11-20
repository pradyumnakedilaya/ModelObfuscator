# ModelObfuscator
**Abstract**:
The use of deep learning (DL) models on edge devices and mobile applications has become increasingly prevalent due to their ability to process data locally, ensuring privacy and achieving low-latency responses. However, deploying on-device models exposes them to significant security risks, as attackers can extract and reverse-engineer these models by unpacking corresponding apps. Such vulnerabilities allow attackers to generate white-box-like attacks or even reconstruct training data. To address this, we propose Model Obfuscation, a novel technique that secures on-device models by concealing critical information such as structure, parameters, and attributes. Our approach employs methods like renaming, parameter encapsulation, neural structure obfuscation, shortcut injection, and extra layer injection. We developed ModelObfuscator, a prototype tool that automates the obfuscation of TensorFlow Lite (TFLite) models. Experimental results demonstrate that our method significantly enhances model security by complicating reverse-engineering attempts, with minimal impact on model latency. Model obfuscation represents a promising foundational technique for secure on-device model deployment.

Wo provide two options to use our prototype tool:

## 1*. Preparation A: run by Docker (recommend)

(0) Download the Docker Image:

```
docker pull pradyumnakedilaya/modelobfuscator:latest
```

Note that if it cause permission errors, please try: 

```
sudo docker pull pradyumnakedilaya/modelobfuscator:latest
```

(1) Enter the environment:

```
docker run -i -t pradyumnakedilaya/modelobfuscator:latest /bin/bash
```

Note that if it cause permission errors, please try: 

```
sudo docker run -i -t pradyumnakedilaya/modelobfuscator:latest /bin/bash
```

Enter the project:

```
cd Code275/
```

(2) Activate the conda environment: 

```
conda activate code275
```

## 1*. Preparation B: build the environment

(0) Download the code:

```
git clone https://github.com/pradyumnakedilaya/ModelObfuscator.git
cd ModelObfuscator
```

(1) The dependency can be found in `environment.yml`. To create the conda environment:

```
conda env create -f environment.yml
conda activate code275
```

Install the Flatbuffer:

```
conda install -c conda-forge flatbuffers
```

(if no npm) install the npm:

```
sudo apt-get install npm
```

Install the jsonrepair:

```
npm install -g jsonrepair
```

Note that the recommend version of gcc and g++ is 9.4.0.


(2) Download the source code of the TensorFlow. Here we test our tool on v2.9.1.

```
wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.9.1.zip
```

Unzip the file:

```
unzip v2.9.1
```

(3) Download the Bazel:

```
wget https://github.com/bazelbuild/bazelisk/releases/download/v1.14.0/bazelisk-linux-amd64
chmod +x bazelisk-linux-amd64
sudo mv bazelisk-linux-amd64 /usr/local/bin/bazel
```

You can test the Bazel:

```
which bazel
```

It should return:

```
# in ubuntu
/usr/local/bin/bazel
```

(4) Configure the build:

```
cd tensorflow-2.9.1/
./configure
cd ..
```

You can use the default setting (just type Return/Enter for every option).

(5) Copy the configurations and script to the source code:  

```
cp ./files/kernel_files/* ./tensorflow-2.9.1/tensorflow/lite/kernels/
cp ./files/build_files/build.sh ./tensorflow-2.9.1/
```

Note that you can mofify the maximal number of jobs in the 'build.sh' script. Here I set it as `--jobs=14`. 

## 2. Test

(1) Build the obfuscation model:

```
bash build_obf.sh
```

Note that you can modify the test model and obfuscation parameters in the script. The obfuscated model is saved as the 'obf_model.tflite'.
