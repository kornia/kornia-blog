---
toc: false
image: images/kornia_logo.png
description: Morphological operators, Deep descriptors, Video Augmentations and more.
layout: post
categories: [release]
title: Kornia v0.5.0 release
---

In this release we have focus in bringing more classic Computer Vision functionalities to the PyTorch ecosystem, like morphological operators and more diversity with Deep Local Descriptors, color conversions and drawing functions. In addition, we have worked towards improving the integration with TPU and better support with Torchscript.

## Morphological Operators

As a highlight we include a [`kornia.morphology`](https://kornia.readthedocs.io/en/latest/morphology.html) that implements several functionalities to work with morphological operators on high-dimensional tensors and differentiability. Contributed by [@Juclique](https://github.com/Juclique)

Morphology implements the following methods: `dilation`, `erosion`, `open`, `close`, `close`, `gradient`, `top_hat` and `black_hat`.

```python
from kornia import morphology as morph

dilated_image = morph.dilation(tensor, kernel) # Dilation
plot_morph_image(dilated_image) # Plot
```
![]({{ site.baseurl }}/images/morphology_cats.png)

See a full tutorial here:
[https://github.com/kornia/tutorials/blob/master/source/morphology_101.ipynb](https://github.com/kornia/tutorials/blob/master/source/morphology_101.ipynb)

## Deep Descriptors

We have added a set of local feature-related models: `MKDDescriptor` #841 by implemented and ported to kornia by [@manyids2](https://github.com/manyids2); also we ported `TFeat`, `AffNet`, `OriNet` from authors repos #846.

Here is [notebook](https://github.com/kornia/kornia-examples/blob/master/MKD_TFeat_descriptors_in_kornia.ipynb), showing the usage and benefits of new features. We also show how to seamlessly integrate kornia and opencv code via new conversion library [`kornia_moons`](https://github.com/ducha-aiki/kornia_moons).

Also: exposed `set_laf_orientation` function #869

![]({{ site.baseurl }}/images/deep_matching.png)

## Video Augmentations

We include a new operator to perform augmentations with videos [`VideoSequential`](https://kornia.readthedocs.io/en/latest/augmentation.container.html#video-data-augmentation). The module is based in `nn.Sequential` and has the ability to concatenate our existing [`kornia.augmentations`](https://kornia.readthedocs.io/en/latest/augmentation.html) for multi-dimensional video tensors. Contributed by [@shijianjian](https://github.com/shijianjian)

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
![]({{ site.baseurl }}/images/video_sequential.png)

See a full example in the following Colab:
[https://colab.research.google.com/drive/12dmHNkvEQrG-PHElbCXT9FgCr_aAGQSI?usp=sharing](https://colab.research.google.com/drive/12dmHNkvEQrG-PHElbCXT9FgCr_aAGQSI?usp=sharing)

## Draw functions

We include an experimental functionality [`draw_rectangle`](https://kornia.readthedocs.io/en/latest/utils.html#kornia.utils.draw_rectangle) implemented in pure `torch.tensor`. Contributed by [@mmathew23](https://github.com/mmathew23)

```python
rects = torch.tensor([[[110., 50., 310., 275.], [325., 100., 435., 275.]]])
color = torch.tensor([255., 0., 0.])

x_out = K.utils.draw_rectangle(x_rgb, rects, color)
```
![]({{ site.baseurl }}/images/draw_rectangle.png)

See full example here:
[https://colab.research.google.com/drive/1me_DxgMvsHIheLh-Pao7rmrsafKO5Lg3?usp=sharing](https://colab.research.google.com/drive/1me_DxgMvsHIheLh-Pao7rmrsafKO5Lg3?usp=sharing)

## More user contrib

- `binary_focal_loss_with_logits` #830 by [@xen0f0n](https://github.com/xen0f0n)
- `rgb_to_lab` #823 by [@cceyda](https://github.com/cceyda)
- `GaussianBlur` augmentation #773 by [@ZhiyuanChen](https://github.com/ZhiyuanChen)
- `get_gaussian_erf_kernel1d`, `get_gaussian_discrete` #736 by [@wyli](https://github.com/wyli)
- `equalize_clahe` #895 by [@lferraz](https://github.com/lferraz)
- `blur_pool2d` #894 by [@shijianjian](https://github.com/shijianjian)
- `get_tps_transform`, `warp_points_tps`, `warp_image_tps` #897 by [@catalys1](https://github.com/catalys1)
- `elastic_transform_2d` #853 by [@IssamLaradji](https://github.com/IssamLaradji) [@edgarriba](https://github.com/edgarriba)

## Infrastructure

- Update CI to pytorch 1.7.x and 1.8.0 [@edgarriba](https://github.com/edgarriba)
- Improve testing matrix with different versions
- TPU support [@edgarriba](https://github.com/edgarriba) [@shijianjian](https://github.com/shijianjian)
- Better JIT support [@edgarriba](https://github.com/edgarriba) @shijianjian @ducha-aiki
- Improved and test docs [@shijianjian](https://github.com/shijianjian) [@edgarriba](https://github.com/edgarriba)

## Deprecations

- Deprecated `kornia.geometry.warp` module.
  - `DepthWarper` is now in `kornia.geometry.depth`
  - `HomographyWarper` and related functions are now inside `kornia.geometry.transform`.
- Deprecated `kornia.contrib module`.
  - `max_pool_blurd2d` is now in `kornia.filters`
- Dropped support of Pytorch 1.5.1 #854

### Warp and Crop

We refactored the interface of the functions `warp_perspective`, `warp_affine`, `center_crop`, `crop_and_resize` and `crop_by_boxes` in order to expose to the user the needed parameters by `grid_sample` [`mode`, `padding_mode`, `align_corners`]. #896

The param align_corners has been set by default to None that maps to True in case the user does not specify.
This comes from the motivation to match the behavior of the warping functions with OpenCV.

Example of `warp_perspective`:

```python
def warp_perspective(src: torch.Tensor, M: torch.Tensor, dsize: Tuple[int, int],
                     mode: str = 'bilinear', padding_mode: str = 'zeros',
                     align_corners: Optional[bool] = None) -> torch.Tensor:
```
Please review the full release notes here:
[https://github.com/kornia/kornia/blob/master/CHANGELOG.md](https://github.com/kornia/kornia/blob/master/CHANGELOG.md)

Thanks to all our contributors !!! :sunglasses:
