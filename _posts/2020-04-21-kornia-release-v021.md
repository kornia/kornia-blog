---
toc: false
image: images/kornia_logo.png
description: Data augmentation framework, deep descriptors and more.
layout: post
categories: [release]
title: Kornia 0.2.1 release
---

In this release we support compatibility between `kornia.augmentation` and `torchvision.transforms`.

We now support all the same existing operations with torch.Tensor in the GPU with extra features such as returning for each operator the transformation matrix generated to produce such transformation.

```python
import kornia as K
import torchvision as T

# kornia

transform_fcn = torch.nn.Sequential(
  K.augmentation.RandomAffine(
    [-45., 45.], [0., 0.5], [0.5, 1.5], [0., 0.5], return_transform=True),
  K.color.Normalize(0.1307, 0.3081),
)

# torchvision

transform_fcn = T.transforms.Compose([
  T.transforms.RandomAffine(
    [-45., 45.], [0., 0.5], [0.5, 1.5], [0., 0.5]),
  T.transforms.ToTensor(),
  T.transforms.Normalize((0.1307,), (0.3081,)),
])
```

Check the online documentations with the updated API [[DOCS]](https://kornia.readthedocs.io/en/latest/augmentation.html)

Check this Google Colab to see how to reproduce same results [[Colab]](https://colab.research.google.com/drive/1T20UNAG4SdlE2n2wstuhiewve5Q81VpS#revisionId=0B4unZG1uMc-WdzZqaStjVzZ1U0hHOHphQkgvcGFCZ1RlUzJvPQ)

## `kornia.augmentation` as a framework

In addition, we have re-designed `kornia.augmentation` such in a way that users can easily contribute with more operators, or just use it as a framework to create their custom operators.

Each of the `kornia.augmentation` modules inherit from [`AugmentationBase`](https://github.com/kornia/kornia/blob/master/kornia/augmentation/augmentation.py#L23) and one can easily define a new operator by creating a subclass and overriding a couple of methods.

Let's take a look at a custom `MyRandomRotation`. The class inherits from `AugmentationBase` making it a `nn.Module` so that can be stacked in a `nn.Sequential` to compute chained transformations.

To implement a new functionality two things needed: override **get_params** and **apply**.

The `get_params` receives the shape of the input tensor and returns a dictionary with the parameters to use in the `apply` function.

The `apply` function receives as input a tensor and the dictionary defined in `get_params` and returns a tuple with the transformed input and the transformation applied to it.

```python
class MyRandomRotation(AugmentationBase):
    def __init__(self, angle: float, return_transform: bool = True) -> None:
      super(MyRandomRotation, self).__init__(self.apply, return_transform)
      self.angle = angle

    def get_params(self, batch_shape: torch.Size) -> Dict[str, torch.Tensor]:
      angles_rad torch.Tensor = torch.rand(batch_shape) * K.pi
      angles_deg = kornia.rad2deg(angles_rad) * self.angle
      return dict(angles=angles_deg)

    def apply(self, input: torch.Tensor, params: Dict[str, torch.Tensor]):
      # compute transformation
      angles: torch.Tensor = params['angles'].type_as(input)
      center = torch.tensor([[W / 2, H / 2]]).type_as(input)
      transform = K.get_rotation_matrix2d(
        center, angles, torch.ones_like(angles))

      # apply transformation
      output = K.warp_affine(input, transform, (H, W))

      return (output, transform)

# how to use it

# load an image and cast to tensor
img1: torch.Tensor = imread(...)  # BxDxHxW

# instantiate and apply the transform
aug = MyRandomRotation(45., return_transformation=True)

img2, transform = aug(img1)  # BxDxHxW - Bx3x3
```
-----

Please, do not hesitate to check the [release notes](https://github.com/kornia/kornia/releases/tag/v0.2.1) on GitHub to learn about the new library features and get more details. 

Have a happy coding day :sunglasses:

**The Kornia team**
