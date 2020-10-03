---
toc: false
image: images/kornia_logo.png
description: Kornia - Open Source Differentiable Computer Vision Library for PyTorch.
layout: post
categories: [release]
title: Kornia 0.1.4 release
---

We have just released **Kornia: a differentiable computer vision library for PyTorch**.

It consists of a set of routines and differentiable modules to solve generic computer vision problems. At its core, the package uses PyTorch as its main backend both for efficiency and to take advantage of the reverse-mode auto-differentiation to define and compute the gradient of complex functions.

Inspired by OpenCV, this library is composed by a subset of packages containing operators that can be inserted within neural networks to train models to perform image transformations, epipolar geometry, depth estimation, and low level image processing such as filtering and edge detection that operate directly on tensors.

It has over 300 commits and majorly refactors the whole library including over than 100 functions to solve generic Computer Vision problems.

# Highlights

Version 0.1.4 includes a reorganization of the internal API grouping functionalities that consists of the following components:

- **kornia** | a Differentiable Computer Vision library like OpenCV, with strong GPU support.
- **kornia.color** | a set of routines to perform color space conversions.
- **kornia.contrib** | a compilation of user contrib and experimental operators.
- **kornia.feature** | a module to perform local feature detection.
- **kornia.filters** | a module to perform image filtering and edge detection.
- **kornia.geometry** | a geometric computer vision library to perform image transformations, 3D linear algebra and conversions using differen camera models.
- **kornia.losses** | a stack of loss functions to solve different vision tasks.
- **kornia.utils** | image to tensor utilities and metrics for vision problems.

Big contribution in [`kornia.features`](https://kornia.readthedocs.io/en/latest/feature.html):

- Implemented anti-aliased local patch extraction.
- Implemented classical local features cornerness functions: Harris, Hessian, Good Features To Track.
- Implemented basic functions for work with local affine features and their patches.
- Implemented convolutional soft argmax 2d and 3d operators for differentable non-maxima suppression.
- implemented second moment matrix affine shape estimation, dominant gradient orientation and SIFT patch descriptor.

-----

Please, do not hesitate to check the [release notes](https://github.com/kornia/kornia/releases/tag/v0.1.4) on GitHub to learn about the new library features and get more details. 

Have a happy coding day :sunglasses:

**The Kornia team**
