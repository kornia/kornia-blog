---
toc: false
image: images/kornia_logo.png
description: Data augmentation API, color conversion improvements, GPU tests and more.
layout: post
categories: [release]
title: Kornia 0.2.0 release
---

Kornia v0.2.0 release is now available.

The release contains over 50 commits and updates support to PyTorch 1.4. This is the result of a huge effort in the desing of the new data augmentation module, improvements in the set of the color space conversion algorithms and a refactor of the testing framework that allows to test the library using the cuda backend.


## Data Augmentation API

From this point forward, we will give support to the new data augmentation API. The [`kornia.augmentation`](https://kornia.readthedocs.io/en/latest/augmentation.html) module mimics the best of the existing data augmentation frameworks such `torchvision` or `albumentations` all re-implemented assuming as input `torch.Tensor` data structures that will allowing to run the standard transformations (geometric and color) in batch mode in the GPU and backprop through it.

In addition, a very interesting feature we are very proud to include, is the ability to return the transformation matrix for each of the transform which will make easier to concatenate and optimize the transforms process.

A quick overview of its usage:

```python
import torch
import kornia

input: torch.Tensor = load_tensor_data(....)  # BxCxHxW

transforms = torch.nn.Sequential(
    kornia.augmentation.RandomGrayscale(),
    kornia.augmentation.RandomAffine(degrees=(-15, 15)),
)

out: torch.Tensor = transforms(input)         # CPU
out: torch.Tensor = transforms(input.cuda())  # GPU

# same returning the transformation matrix

transforms = torch.nn.Sequential(
    kornia.augmentation.RandomGrayscale(return_transformation=True),
    kornia.augmentation.RandomAffine(degrees=(-15, 15), return_transformation=True),
)

out, transform = transforms(input) # BxCxHxW , Bx3x3
```

## GPU Test

We have refactored our testing framework and we can now easily integrate GPU tests within our library. At this moment, this features is only available to run locally but very soon we will integrate with CircleCI and AWS infrastructure so that we can automate the process.

From root one just have to run: `make test-gpu`

Tests look like this:

```python
import torch
from test.common import device

def test_rgb_to_grayscale(self, device):
        channels, height, width = 3, 4, 5
        img = torch.ones(channels, height, width).to(device)
        assert kornia.rgb_to_grayscale(img).shape == (1, height, width)
```

-----

Please, do not hesitate to check the [release notes](https://github.com/kornia/kornia/releases/tag/v0.2.0) on GitHub to learn about the new library features and get more details. 

Have a happy coding day :sunglasses:

**The Kornia team**
