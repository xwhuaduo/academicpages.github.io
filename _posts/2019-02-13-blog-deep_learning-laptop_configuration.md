---
title: 'Building Deep Learning Environment'
date: 2019-2-13
permalink: /posts/2019/2/blog-deep_learning-laptop_config/
tags:
  - Deep Learning
  - Ubuntu
  - CUDA
  - Keras
  - Conda
---

If you want to run deep learning models, such as models with CNN, to conduct image classification or object detection, you gonna need a GPU to shorten the training time. If you don't have a machine with a GPU, your best option is to use the Cloud Platform. 

Following the  [instruction on Stanford course CS231n](http://cs231n.github.io/gce-tutorial/), I registered on google cloud platform(GCP) and got \$300 credit for free to spend on GCP. I trained an object detection model ([SSD 300](https://github.com/pierluigiferrari/ssd_keras)) and there are only a few credits left.

So, I got a new laptop with a GTX 1060 GPU, and I managed to install Ubuntu 18.04 LST operating system and configuring the environment for deep learning. This post summaries the main steps to build an environment for Deep Learning.

The laptop I got is [Overpowered gaming laptop 17+](https://www.walmart.com/ip/OVERPOWERED-Gaming-Laptop-17-2-Year-Warranty-144Hz-Intel-i7-8750H-NVIDIA-GeForce-GTX-1060-Mechanical-LED-Keyboard-256-SSD-2TB-HDD-32GB-RAM-Windows-10/887474519). 

#### 1. Install ubuntu. 
There are tons of tutorials about installing Ubuntu, you can just google it.
#### 2. Install GPU driver
The laptop I got has two GPU (Intel GPU and Nvidia GPU), and they are not configured properly by Ubuntu by default, and it causes the system to freeze when you first enter ubuntu. I follow this [blog](https://blog.csdn.net/ZhuJiayou/article/details/83177000) and use `sudo ubuntu-drivers autoinstall` to install the driver for my GTX 1060. 
#### 3. Install CUDA
Then I follow this [blog](https://www.pytorials.com/how-to-install-tensorflow-gpu-with-cuda-10-0-for-python-on-ubuntu/) to install CUDA. I have no problem following Step 1 to Step 10. For Step 9 and Step 10, the version of cnDNN and NCCL you got may be different, but it does not matter. At Step 11, I got some error message for installing "keras_applications==1.0.5 --no-deps". So I turn to [anaconda](https://anaconda.org).
#### 4. Install Anaconda
Download and install anaconda. The installed version is Python 3.7. Tensorflow is not working on Python3.7 yet don't worry.
#### 5. Install Tensorflow
After installation, create a new conda environment with 
```conda create --name test_gpu```
. 
Then activate it with 

```conda activate test_gpu```

,and install keras with

```conda install -c anaconda keras-gpu```
.
Tensorflow will be installed automatically.

#### 6. Install Jupyter Notebook
Install jupyter notebook to the current environment(test_gpu), then you can call Tensorflow and Keras in jupyter notebook.
