# YOLOv5
YOLOv5 (v6.0/6.1) is a powerful object detection algorithm developed by Ultralytics. This article dives deep into the YOLOv5 architecture, data augmentation strategies, training methodologies, and loss computation techniques. This comprehensive understanding will help improve your practical application of object detection in various fields, including surveillance, autonomous vehicles, and image recognition.

## Paper Summary
![image-20231219163621511-768x758](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/22841057-63f2-4291-8a27-4202cf7a38d6)
**Figure 1: YOLOv5 Model Architecture** [(Source)](https://arxiv.org/abs/2304.00501)

## Model Architecture:
The YOLOv5 model is divided into 3 parts, Backbone, Neck, and Head. Each convolution is followed by batch normalization (BN) and SiLU activation. Below is the 3000-foot overview of the YOLOv5 Model Architecture
- **Backbone:** This is the main body of the network. For YOLOv5, the backbone is designed using the `New CSP-Darknet53` structure, a modification of the Darknet architecture used in previous versions.
- **Neck:** This part connects the backbone and the head. In YOLOv5, SPPF and `New CSP-PAN ` structures are utilized.
- **Head:** This part is responsible for generating the final output. YOLOv5 uses the `YOLOv3 Head` for this purpose.

## Spatial Pyramid Pooling Fast (SPPF):
SPPF is one of the improvements that they introduced in their model. This is nothing but SPP, it’s just that instead of passing the input to three different max-pool layers, the output of one max-pool block is being fed to the subsequent max-pool layer, which makes the process much faster. Below is the Pytorch implementation of SPP and SPPF:

![Screenshot-from-2023-12-26-13-34-45-768x236](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/3337fb83-1348-444f-83b4-55332c7043e5)
**Figure 2: SPP and SPPF Pytorch implementation** [(Source)](https://docs.ultralytics.com/yolov5/tutorials/architecture_description/#1-model-structure)
