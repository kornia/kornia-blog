---
toc: false
image: images/kornia_logo.png
description: Morphological operators, Deep descriptors, Video Augmentations and more.
layout: post
categories: [release]
title: Kornia v0.5.0 release
---

In this release we have focus in bringing more classic Computer Vision functionalities to the PyTorch ecosystem, like morphological operators and more diversity with Deep Local Descriptors, color conversions and drawing functions. In addition, we have worked towards improving the integration with TPU and better support with Torchscript.
Highlights

## Morphological Operators

As a highlight we include a kornia.morphology that implements several functionalities to work with morphological operators on high-dimensional tensors and differentiability. Contributed by @Juclique

Morphology implements the following methods: dilation, erosion, open, close, close, gradient, top_hat and black_hat.

```python
from kornia import morphology as morph

dilated_image = morph.dilation(tensor, kernel) # Dilation
plot_morph_image(dilated_image) # Plot
```
![]("https://user-images.githubusercontent.com/5157099/111034527-73b01a80-8416-11eb-9012-976db14ef826.png")

See a full tutorial here: https://github.com/kornia/tutorials/blob/master/source/morphology_101.ipynb

## Deep Descriptors

We have added a set of local feature-related models: MKDDescriptor #841 by implemented and ported to kornia by @manyids2; also we ported TFeat, AffNet, OriNet from authors repos #846.

Here is notebook, showing the usage and benefits of new features. We also show how to seamlessly integrate kornia and opencv code via new conversion library kornia_moons.

Also: exposed set_laf_orientation function #869

![]("https://user-images.githubusercontent.com/4803565/111051654-19827a00-8455-11eb-9edd-d965f3446088.png")

## Video Augmentations

We include a new operator to perform augmentations with videos VideoSequential. The module is based in nn.Sequential and has the ability to concatenate our existing kornia.augmentations for multi-dimensional video tensors. Contributed by @shijianjian

```python
import kornia
import torchvision

clip, _, _ = torchvision.io.read_video("drop.avi")
clip = clip.permute(3, 0, 1, 2)[None] / 255.  # To BCTHW
input = torch.randn(2, 3, 1, 5, 6).repeat(1, 1, 4, 1, 1)

aug_list = VideoSequential(
    kornia.augmentation.ColorJitter(0.1, 0.1, 0.1, 0.1, p=1.0),
    kornia.augmentation.RandomAffine(360, p=1.0),
    data_format="BTCHW",
    same_on_frame=False)
)

out = aug(input)
```
![]("https://user-images.githubusercontent.com/5157099/111078400-7c2b5280-84f5-11eb-854f-71c5e87bc4c3.png")

See a full example in the following Colab:
https://colab.research.google.com/drive/12dmHNkvEQrG-PHElbCXT9FgCr_aAGQSI?usp=sharing

## Draw functions

We include an experimental functionality draw rectangle implemented in pure torch.tensor. Contributed by @mmathew23

```python
rects = torch.tensor([[[110., 50., 310., 275.], [325., 100., 435., 275.]]])
color = torch.tensor([255., 0., 0.])

x_out = K.utils.draw_rectangle(x_rgb, rects, color)
```
![]("https://user-images.githubusercontent.com/5157099/111036305-e2917180-841e-11eb-8884-2e4d89177fe8.png")

See full example here: https://colab.research.google.com/drive/1me_DxgMvsHIheLh-Pao7rmrsafKO5Lg3?usp=sharing

## More user contrib

- binary_focal_loss_with_logits #830 by @xen0f0n
- rgb_to_lab #823 by @cceyda
- GaussianBlur augmentation #773 by @ZhiyuanChen
- get_gaussian_erf_kernel1d, get_gaussian_discrete #736 by @wyli
- equalize_clahe #895 by @lferraz
- blur_pool2d #894 by @shijianjian
- get_tps_transform, warp_points_tps, warp_image_tps #897 by @catalys1
- elastic transform 2d #853 by @IssamLaradji @edgarriba

## Infrastructure

- Update CI to pytorch 1.7.x and 1.8.0 @edgarriba
- Improve testing matrix with different versions
- TPU support @edgarriba @shijianjian
- Better JIT support @edgarriba @shijianjian @ducha-aiki
- Improved and test docs @shijianjian @edgarriba

## Deprecations

- Deprecated kornia.geometry.warp module.
  - DepthWarper is now in kornia.geometry.depth
  - HomographyWarper and related functions are now inside kornia.geometry.transform.
- Deprecated kornia.contrib module.
  - max_pool_blurd2d is now in kornia.filters
- Dropped support of Pytorch 1.5.1 #854

### Warp and Crop

We refactored the interface of the functions warp_perspective, warp_affine, center_crop, crop_and_resize and crop_by_boxes in order to expose to the user the needed parameters by grid_sample [mode, padding_mode, align_corners]. #896

The param align_corners has been set by default to None that maps to True in case the user does not specify.
This comes from the motivation to match the behavior of the warping functions with OpenCV.

Example of `warp_perspective`:

```python
def warp_perspective(src: torch.Tensor, M: torch.Tensor, dsize: Tuple[int, int],
                     mode: str = 'bilinear', padding_mode: str = 'zeros',
                     align_corners: Optional[bool] = None) -> torch.Tensor:
```
Please review the full release notes here: https://github.com/kornia/kornia/blob/master/CHANGELOG.md

Thanks to all our contributors !!! :sunglasses:
