---
toc: false
image: images/medical_registration.gif
description: Learn how to align automatically with kornia Image Registration API
layout: post
categories: [announcement]
title:  Image Registration with Kornia and PyTorch in the GPU 
---

Dear Computer Vision community,

As previously announced, we are starting to include more high level APIs in [Kornia](https://kornia.readthedocs.io/en/latest/tasks.html)
to help developers and researchers create innovative applicationsfor Computer Vision.

![]({{ site.baseurl }}/images/kornia_logo.png)

The first release in that direction is our [ImageRegistration](https://kornia.readthedocs.io/en/latest/geometry.transform.html#kornia.geometry.transform.image_registrator.ImageRegistrator) 
PI that leverages PyTorch Autograd engine and the GPU power to solve by direct optimization the problem of aligning two images on the fly.

```python
import kornia.geometry as KG

registrator = KG.ImageRegistrator('similarity')
model, interm = registrator.register(img1, img2, output_intermediate_models=True)
```

Image registration is the process of transforming different sets of data into one coordinate system. Data may be multiple photographs,
data from different sensors, times, depths, or viewpoints. It is used in computer vision, medical imaging, and compiling and analyzing
images and data from satellites. Registration is necessary in order to be able to compare or integrate the data obtained from these different measurements.

![]({{ site.baseurl }}/images/medical_registration.gif)

Learn more @ Papers with Code: [https://paperswithcode.com/task/image-registration](https://paperswithcode.com/task/image-registration)

Finally, in order to create a more advanced application e.g. for computational photography you can refer to our [documentation](https://kornia.readthedocs.io/en/latest/tasks.html)
and [tutorials](https://kornia-tutorials.readthedocs.io/en/latest/image_registration.html).

> youtube: https://youtu.be/I-g3EhBIsDs

Stay tuned for upcoming news and projects around the #kornia universe.

-------------------

Visit our website 🚀 [www.kornia.org](www.kornia.org) 🚀
Give us a star in Github ⭐️ [https://github.com/kornia/kornia/stargazers](https://github.com/kornia/kornia/stargazers) ⭐️
Follow us in Twitter 🐦 [https://twitter.com/kornia_foss](https://twitter.com/kornia_foss) 🐦
Donate (@opencollective) 🙏 [https://opencollective.com/kornia](https://opencollective.com/kornia) 🙏
Join our community: [www.librecv.org](www.librecv.org)

#computervision #opensource #deeplearning #pytorch #ai
