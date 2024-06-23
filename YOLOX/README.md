# YOLOX

## Paper Summary

  - In July 2021, **YOLOX** was published by Megvii Technology, China. This model was the first-place winner of the Streaming Perception Challenge of the WAD workshop at CVPR 2021. It was published on the same timeline as YOLO-X. When the authors of YOLOR tried to explore multi-task learning, the authors of YOLOX focused on integrating the recent advances into the YOLO model. All YOLO models before YOLOX were using Anchor boxes. However, during that period, most state-of-the-art models used Anchor-free detectors. Hence, the authors incorporated this approach into their model, along with the advanced label assignment strategy simOTA, Decoupled Head, and Strong Augmentations. 

  - The YOLOX model features the DarkNet53 from YOLOv3 with **spatial pyramid pooling(SPP)** as the backbone, FPN as the Neck, and a customized decoupled head. The YOLOX model was trained from scratch, the authors excluded ImageNet Pre-training because using that with strong augmentations was no longer useful. At that time, YOLOv3 was still one of the best detectors to which a large part of the industry could turn because of limited computation. YOLOv5, on the other hand, was optimized, keeping the anchor-based method in mind. Although the Backbone and Neck of the YOLO detectors improved over time, the head part was untouched, and decoupling the head also resolved the long ongoing conflict of classification, and regression tasks. This helped them in faster convergence.

  - It starts with one 1×1 conv layer, two 3×3 conv layers again, and a 1×1 conv layer (for each branch). They had a very good training regimen, 300 epochs of training with SGD, cosine scheduler, multi-scale training, **EMA weight updation**, BCE loss for the classification branch, and IoU loss for the regression branch.

  - The authors used heavy augmentations to improve the model performance, they explicitly added the famous **Mosaic and MixUp** in their augmentation pipeline. They found out that for large models, strong augmentation was useful. They used MixUp with scale jittering in their pipeline after exterminating with MixUp and Copypaster.

  - Inspired by end-to-end (NMS-free) detectors, the authors added two additional convolution layers, one-to-one label assignment and stop gradient. Still, unfortunately, that slightly decreased the performance and the inference speed, so they dropped it.


# Training YOLOX
YOLOX, a model meticulously engineered for object detection, has been effectively utilized for YOLOX custom training on the Drone Detection dataset, featuring 4014 labeled images. The training was conducted using the official YOLOX repository, with the model being tested in various configurations to analyze its performance. The outcomes of these tests are shared to provide insights into the model’s capabilities.


