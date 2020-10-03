---
toc: false
image: images/match_kornia.png
description: 3D augmentations, volumetric data, local features matching, homographies and epipolar geometry.
layout: post
categories: [release]
title: Kornia v0.4.0 release
---

**Kornia team** is happy to announce the release for v0.4.0.

We are releasing a new version for Kornia that includes different functionalities to work with **3D augmentations** and **volumetric data**, **local features matching**, **homographies** and **epipolar geometry**.

In short, we list the following new features:

- Support for PyTorch v1.6.0.
- Local descriptors matching, homography and epipolar geometry API.
- 3D augmentations and low-level API to work with volumetric data.

## Local features matching

We include an [kornia.feature.matching](https://kornia.readthedocs.io/en/latest/feature.html#matching) API to perform local descriptors matching such classical and derived version of the nearest neighbor (NN).

![]({{ site.baseurl }}/images/match_kornia.png)


```python
import torch
import kornia as K

desc1 = torch.rand(2500, 128)
desc2 = torch.rand(2500, 128)

dists, idxs = K.feature.matching.match_nn(desc1, desc2)  # 2500 / 2500x2
```

## Homography and epipolar geometry

We also introduce [kornia.geometry.homography](https://kornia.readthedocs.io/en/latest/geometry.homography.html) including different functionalities to work with homographies and differentiable estimators based on the DLT formulation and the iteratively-reweighted least squares (IRWLS).

![]({{ site.baseurl }}/images/epipolar_geometry.png)

```python
import torch
import kornia as K

pts1 = torch.rand(1, 8, 2)
pts2 = torch.rand(1, 8, 2)
H = K.find_homography_dlt(pts1, pts2, weights=torch.rand(1, 8))  # 1x3x3
```

In addition, we have ported some of the existing algorithms from [opencv.sfm](https://docs.opencv.org/master/d8/d8c/group__sfm.html) to PyTorch under [kornia.geometry.epipolar](https://kornia.readthedocs.io/en/latest/geometry.epipolar.html) that includes different functionalities to work with Fundamental, Essential or Projection matrices, and Triangulation methods useful for Structure from Motion problems.

## 3D augmentations and volumetric

We expand the [kornia.augmentaion](https://kornia.readthedocs.io/en/latest/augmentation.html) with a series of operators to perform 3D augmentations for volumetric data.

![]({{ site.baseurl }}/images/kornia_3daugmentations.gif)

In this release, we include the following first set of geometric 3D augmentations methods:

- RandomDepthicalFlip3D (along depth axis)
- RandomVerticalFlip3D (along height axis)
- RandomHorizontalFlip3D (along width axis)
- RandomRotation3D
- RandomAffine3D

The API for 3D augmentation work same as with 2D image augmentations:

```python
import torch
import kornia as K

x = torch.eye(3).repeat(3, 1, 1)
aug = K.augmentation.RandomVerticalFlip3D(p=1.0)

print(aug(x))

tensor([[[[[0., 0., 1.],
            [0., 1., 0.],
            [1., 0., 0.]],
<BLANKLINE>
            [[0., 0., 1.],
            [0., 1., 0.],
            [1., 0., 0.]],
<BLANKLINE>
            [[0., 0., 1.],
            [0., 1., 0.],
            [1., 0., 0.]]]]])
```

Finally, we also introduce a low level API to perform 4D features transformations [kornia.warp_projective](https://kornia.readthedocs.io/en/latest/geometry.transform.html#kornia.geometry.transform.warp_projective) and extending the filtering operators to support 3D kernels [kornia.filter3D](https://kornia.readthedocs.io/en/latest/filters.html#kornia.filters.filter3D).

## More 2d operators

We expand as well the list of the 2D image augmentations based on the paper [AutoAugment: Learning Augmentation Policies from Data](https://arxiv.org/pdf/1805.09501.pdf).

- Solarize
- Posterize
- Sharpness
- Equalize
- RandomSolarize
- RandomPosterize
- RandomShaprness
- RandomEqualize

-----

Please, do not hesitate to check the [release notes](https://github.com/kornia/kornia/releases/tag/v0.4.0) on GitHub to learn about the new library features and get more details. 

Have a happy coding day :sunglasses:

**The Kornia team**
