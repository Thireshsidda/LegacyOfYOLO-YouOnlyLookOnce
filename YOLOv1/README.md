# YOLOv1

## Paper Summary
Generally, there are two types of Deep Learning object detection models: single-stage and two-stage models. The single-stage model follows a specific design pattern, which is the Backbone-Neck-Head. However, in YOLOv1, there is no concept of a neck, only a backbone and head. YOLOv1 architecture is inspired by GoogleNet’s architecture, and has 24 convolutional layers and two fully-connected layers. In these layers, the first twenty layers act as a backbone, and rest of the layers lead up to an additional two fully-connected layers, acting as a detection head. Instead of the inception module, they used a 1×1 convolution layer with 3×3 convolutional layers in the backbone. This helped to reduce the number of channels without having to reduce spatial dimensions, and the number of parameters became relatively low. 

![Pasted-image-20231207112932-1-768x231](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/a7b6eba1-3fbd-4f9e-aa72-5ef92d73ab45)

**Figure 1: YOLOv1 model architecture**


The authors of YOLOV1  pre-trained the first 20 layers of YOLO in ImageNet at a resolution of 224×224, and the four remaining layers were finetuned in PASCAL VOC 2012 at a resolution of 448×448. This increased the information for the model to detect small objects more accurately. They trained their model for 135 epochs on the training and validation sets from VOC 2007 and 2012. They used a batch size of 64, momentum of 0.9, and learning rate decay of 0.0005. The learning rate(LR) scheduling is such that in the first few epochs the LR rate rises from $10^{-3}$ to $10^{-2}$. Initially it is trained for 75 epochs with a learning rate of $10^{-2}$ followed by $10^{-3}$ for 30 epochs, and finally the model undergoes an additional $10^{-4}$ for 30 epochs.

To avoid overfitting, the authors used a dropout rate of 0.5 and extensive augmentation. Scaling and translations of up to 20% of the total dataset were used. They randomly adjusted the exposure and saturation of the image by up to a factor of 1.5 in the HSV color space. A linear activation function was set for the final layer, and the rest of the other layers used the leaky-ReLU activation.

There is no concept of `anchor-free` boxes. The authors divided the image into an SxS grid, where `each grid predicts B bounding box` coordinates and an objectness score associated with bounding boxes and C number of class probabilities. The prediction gets encoded in a $SxSx(Bx5+C) $ tensor. It also means that One grid cell can only predict one object. As bounding box coordinates, YOLO predicts the center (x,y) of the bounding box and the width(w) and height(h) of the box. The center is relative to the grid cell, so it’s between 0 and 1, and width and height are relative to the image size, which also comes in the range of 0 and 1. Objectness score refers to the score that tells if a bounding box contains an object or not, which is denoted as $\small p(Object)$ * $IOU_{\text {pred }}^{\text {truth }}$ where $\small p(Object)$  is the probability to have an object in the cell and $IOU_{\text {pred }}^{\text {truth }}$ is the intersection over union between predicted region and the grand truth.

![yolo-768x401](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/d8ac72cf-c7fc-4b27-bea2-afd9c057e30d)

**Figure 2: YOLO Bounding anchor-free bounding box prediction**

As the detection head needs to predict the bounding box coordinates, objectness score, and object class, they have three parts to the loss function: localization loss, confidence loss, and classification loss. As the object detection was depicted as a regression problem, all losses are sum-squared errors. The first two loss terms belong to localization loss, the next two losses belong to the confidence loss, and the last one belongs to the classification loss. Often there are grids that don’t predict any bounding box, which pushes the confidence score of those cells toward zero, overpowering the cells that contain an object. Two parameters $\lambda_{coord} = 5$ and $\lambda_{noobj} = 0.5$ were proposed to resolve this issue.

![Screenshot-from-2023-12-26-13-09-32](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/61f021b7-984a-4364-95c1-68072ad6d923)

**Figure 3: YOLOv1 Loss Function** [Source](https://arxiv.org/abs/1506.02640)




