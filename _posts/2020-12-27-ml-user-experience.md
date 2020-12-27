---
title: Running Deep Learning Models in 2020
slug: running-dl-models
date_published: 2020-12-27T11:05:57.000Z
tags: Tech
layout: post
---

Over the past few months or so, I've had to work on getting pretrained deep learning (DL) models downloaded off GitHub to run for some robotics projects. In doing so, I've had to work with getting inference to run on an NVIDIA GPU and in this post, I want to highlight my experiences with working with ML modelling libraries and NVIDIA's own tools.

### What Works Well

With the growth in machine learning applications in the last ten years, it looks like there's never been a better time for NVIDIA. The [NVIDIA CUDA libraries](https://developer.nvidia.com/cuda-zone) have turned their gaming GPUs into general purpose computing monsters overnight. Vector and matrix operations are crucial to several ML algorithms, and these run several times faster on the GPU than they do on the fastest CPUs. To solidify their niche in this market, the 10- series and newer of NVIDIA GPUs come with dedicated hardware, such as the Deep Learning Accelerator (DLA) and 16-bit floating point (FP16) optimizations, for faster inference performance.

NVIDIA has developed several software libraries to run optimized ML models on their GPUs. The alphabet soup of ML frameworks and their file extensions took me some time to decipher, but the actual workflow of model optimization is surprisingly simple. Once you have a trained your PyTorch or Tensorflow model, you can convert the saved model or frozen checkpoint to the ONNX model exchange format. ONNX lets you convert your ML model from one format to another, and in the case of NVIDIA GPUs, the format of choice is a [TensorRT](https://docs.nvidia.com/deeplearning/tensorrt/developer-guide/index.html) engine plan file. TensorRT engine plan files are bespoke, optimized inference models for a given configuration of CUDA, NVIDIA ML libraries, GPU, maximum batch size, and decimal precision. As such, they are not portable among different hardware/software configurations and they can only be generated *on* the GPU you want to use for deployment. 

When the TensorRT conversion works as it's supposed to, you can expect to see a significant 1.5-3x performance boost over running the Tensorflow/PyTorch model. INT8 models might have even better performance, but this requires a calibration dataset for constructing the TensorRT engine. TensorRT is great for deploying models on the NVIDIA Xavier NX board, since you can also build TensorRT engines which run on the power efficient DLAs as opposed to the GPU. 

### What Needs Improvement

For all of NVIDIA's good work in developing high performance ML libraries for their GPUs, the user experience of getting started with their ML development software leaves some to be desired. It's *imperative* to double check all the version numbers of the libraries you need when doing a fresh installation. For instance, a particular version of `tensorflow-gpu` only works with a [specific point release](https://www.tensorflow.org/install/source#tested_build_configurations) of CUDA which in turn depends on using the right GPU driver version. All of these need to be installed independently, so there are many sources of error which can result in a broken installation. Installing the wrong version of TensorRT and cuDNN can also result in dependency hell. I am curious to know why all these libraries are so interdependent on specific point releases and why the support for backward compatibility of these libraries is so minimal. One solution around this issue is using one of NVIDIA's [several Docker images](https://ngc.nvidia.com/catalog/containers/nvidia:tensorrt) with all the libraries pre-installed. Occasionally, I also ran into issues with CUDA not initializing properly, but this was fixed by probing for the NVIDIA driver using `sudo nvidia-modprobe`.

As an aside, I also found that the accuracy of the TensorRT engine models are subjectively good, but there is noticeable loss in accuracy when compared to running the pretrained Tensorflow model directly. This can be seen here when evaluating the [LaneNet](https://github.com/MaybeShewill-CV/lanenet-lane-detection) road lane detection model:

![](/content/images/2020/0.jpeg)

Source Image

![](/content/images/2020/tf-model.png)

Tensorflow Frozen Checkpoint Inference (80 fps)

![](/content/images/2020/trt-model.png)

TensorRT engine inference in FP32 mode (144 fps) and FP16 mode (228 fps)

The results are subjectively similar, but there is a clear tradeoff in the accuracy of lane detection and inference time when running these models on the laptop flavour of the NVIDIA RTX 2060.

On the Tensorflow and ONNX side of things, the conversion from a frozen Tensorflow checkpoint file to an ONNX model was more difficult than I anticipated. The `tf2onnx` tool requires the names of the input and output nodes for the conversion, and the best way to do this according to the project's GitHub README is to use the `summarize_graph` tool which isn't included by default in a Tensorflow distribution. This means downloading and building parts of Tensorflow from source just to check the names of the input and output nodes of the model. Curiously enough, the input and output node names need to be in the format of `node:0`, though appending the `:0` is not explicitly mentioned in the documentation. I also learned that the `.pb` file extension can refer to either a Tensorflow Saved Model or Frozen Model, and both of these *cannot* be used interchangeably. Luckily, the experience becomes considerably less stressful once the ONNX file is successfully generated and Tensorflow can be removed from the picture. I also tried using the ONNX file with OpenVINO using Intel's DL Workbench Docker image and the conversion to an Intel optimized model was a cinch. That said, the inference performance on an Intel CPU with the OpenVINO model wasn't anywhere close to that on an NVIDIA GPU using a TensorRT model when using the same ONNX model.

### Conclusion

I feel it's important that the logistics of setting up a DL workstation with the libraries is made easier. The ONNX file format is a good step in the right direction to making things standardized and the optimized model generation software from a variety of hardware manufacturers has been crucial in improving inference performance. The DLAs found on the Jetson boards (known as the Neural Processing Unit (NPU) on Apple's A11+ chips) are quite capable in their own right, while using a fraction of the power a GPU would use. Intel has approached the problem at a different angle by adding AVX512 for faster vector operations using the CPU and a Gaussian Neural Accelerator (presumably comparable to a DLA?) with the 10th Gen mobile SoCs. If the last few years is anything to go by, advancements in DL inference performance per watt show no sign of slowing down and it's not hard to foresee a future where a type of DLA becomes as standardized as the CPU and GPU on most computing platforms.