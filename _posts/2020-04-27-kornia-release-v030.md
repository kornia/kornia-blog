---
toc: false
image: images/kornia_logo.png
description: PyTorch 1.5 support, accelerated Data Augmentation, stable GPU testing and easy ecosystem integration.
layout: post
categories: [release]
title: Kornia 0.3.0 release
---

Today we released 0.3.0 which aligns with PyTorch releases cycle and includes:

- Full support to PyTorch v1.5.
- Semi-automated GPU tests coverage.
- Documentation has been reorganized [[docs]](https://kornia.readthedocs.io/en/latest/).
- Data augmentation API compatible with torchvision v0.6.0.
- Well integration with ecosystem e.g. Pytorch-Lightning.

For more detailed changes check out [v0.2.1](https://github.com/kornia/kornia/releases/tag/v0.2.1) and [v0.2.2](https://github.com/kornia/kornia/releases/tag/v0.2.2).

# Highlights

## Data Augmentation

We provide [`kornia.augmentation`](https://kornia.readthedocs.io/en/latest/augmentation.html#) a high-level framework that implements kornia-core functionalities and is fully compatible with torchvision supporting batched mode, multi device cpu, gpu, and xla/tpu (comming), auto differentiable and able to retrieve (and chain) applied geometric transforms. To check how to reproduce torchvision in kornia refer to this [Colab: Kornia vs. Torchvision](https://colab.research.google.com/drive/1T20UNAG4SdlE2n2wstuhiewve5Q81VpS#revisionId=0B4unZG1uMc-WdzZqaStjVzZ1U0hHOHphQkgvcGFCZ1RlUzJvPQ/) [**@shijianjian**](https://github.com/shijianjian)

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

## Ecosystem compatibility

Kornia has been designed to be very flexible in order to be integrated in other existing frameworks. See the example below about how easy you can define a custom data augmentation pipeline to later be integrated into any training framework such as [Pytorch-Lighting](https://pytorch-lightning.readthedocs.io/en/latest/). We provide examples in [[here]](https://github.com/kornia/kornia-examples/blob/master/kornia_lightning_mnist_gpu.ipynb) and [[here]](https://github.com/PyTorchLightning/lightning-Covid19/blob/master/lightning_covid19/model/densenet.py#L44).

```python
class DataAugmentatonPipeline(nn.Module):
    """Module to perform data augmentation using Kornia on torch tensors."""
    def __init__(self, apply_color_jitter: bool = False) -> None:
        super().__init__()
        self._apply_color_jitter = apply_color_jitter

        self._max_val: float = 1024.

        self.transforms = nn.Sequential(
            K.augmentation.Normalize(0., self._max_val),
            K.augmentation.RandomHorizontalFlip(p=0.5)
        )

        self.jitter = K.augmentation.ColorJitter(0.5, 0.5, 0.5, 0.5)

    @torch.no_grad()  # disable gradients for effiency
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        x_out = self.transforms(x)
        if self._apply_color_jitter:
            x_out = self.jitter(x_out)
        return x_out
```

## GPU tests

Now easy to run GPU tests with `pytest --typetest cuda`

-----

Please, do not hesitate to check the [release notes](https://github.com/kornia/kornia/releases/tag/v0.3.0) on GitHub to learn about the new library features and get more details. 

Have a happy coding day :sunglasses:

**The Kornia team**
