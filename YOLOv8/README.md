# YOLOv8

## Paper Summary

In January 2023, Ultralytics came up with YOLOv8, which was the **SOTA** at that time. The model was open-sourced, but the maintainers didn’t publish any paper. Looking at the architecture of the YOLOv8 model, the previous model seemed **over-engineered**.

![yolov8-architecture-scaled-1-768x804](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/18ebe7ce-d6ad-4205-893a-8b4128b68f69)

**Figure 1: YOLOv8 Architecture**

The YOLOv8 architecture follows the same architecture as YOLOv5, with a few slight adjustments, such as the use of the **c2f module** instead of CSPNet module, which is just a variant of CSPNet, (CSPNet followed by two convolutional networks). They designed YOLOv8 **anchor-free** using **YOLOv5 as a based model**, which YOLOX authors tried but ended up using YOLOv3 as they found YOLOv5 over-optimized for anchor-based training. They also introduced **Decoupled heads** to independently process objectness, classification and bounding box prediction tasks. They used the sigmoid layer as the last layer for objectness score prediction and softmax for classification.

YOLOv8 uses **CIoU and DFL loss** functions for bounding box loss and binary cross-entropy for classification loss. These losses have improved object detection performance, particularly when dealing with smaller objects. They also introduced the **YOLOv8-Seg model** for semantic segmentation by applying minimal changes to the original model.

The absence of an official paper for YOLOv8 poses challenges in understanding the intricacies of the model through code or GitHub discussions alone.

## Training YOLOv8



