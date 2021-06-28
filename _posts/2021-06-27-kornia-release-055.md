---
toc: false
image: images/kornia_logo.png
description: Kornia - Open Source Differentiable Computer Vision Library for PyTorch.
layout: post
categories: [release]
title: Kornia 0.5.5 release
---

**Kornia team** is happy to announce the release for v0.5.5.

- Support for PyTorch 1.9.0
- Interface for Stereo Camera manipulation
- Random selection for the augmentation containers
- Bug fixes

### [0.5.5] - 2021-06-27

### Added
- Added Stereo camera class ([#1102](https://github.com/kornia/kornia/pull/1102))
- Added auto-generated images in docs ([#1105](https://github.com/kornia/kornia/pull/1105)) ([#1108](https://github.com/kornia/kornia/pull/1108)) ([#1127](https://github.com/kornia/kornia/pull/1127)) ([#1128](https://github.com/kornia/kornia/pull/1128)) ([#1129](https://github.com/kornia/kornia/pull/1129)) ([#1131](https://github.com/kornia/kornia/pull/1131))
- Added chinese version README ([#1112](https://github.com/kornia/kornia/pull/1112))
- Added random_apply to augmentaton containers ([#1125](https://github.com/kornia/kornia/pull/1125))

### Changed
- Change GaussianBlur to RandomGaussianBlur ([#1118](https://github.com/kornia/kornia/pull/1118))
- Update ci with pytorch 1.9.0 ([#1120](https://github.com/kornia/kornia/pull/1120))
- Changed option for mean and std to be tuples in normalization ([#987](https://github.com/kornia/kornia/pull/987))
- Adopt torch.testing.assert_close ([#1031](https://github.com/kornia/kornia/pull/1031)) 

### Removed
- Remove numpy import ([#1116](https://github.com/kornia/kornia/pull/1116))

### Contributors
@copaah @ducha-aiki @edgarriba @eugene87222 @JoanFM @justanhduc @pmeier @shijianjian 

In addition, we are revamping our online documentation by adding as much as visual examples to each functionality.

Visit our documentation here: [https://kornia.readthedocs.io/en/latest/enhance.html](https://kornia.readthedocs.io/en/latest/enhance.html)

------

👉 Feel free to reach us for collaborations

Visit our website 🚀 [www.kornia.org](www.kornia.org) 🚀

Give us a star in Github ⭐️ [https://github.com/kornia/kornia/stargazers](https://github.com/kornia/kornia/stargazers) ⭐️

Follow us in Twitter 🐦 [https://twitter.com/kornia_foss](https://twitter.com/kornia_foss) 🐦

Donate (@opencollective) 🙏 [https://opencollective.com/kornia](https://opencollective.com/kornia) 🙏

------

Have a happy coding day :sunglasses:

**The Kornia team**
