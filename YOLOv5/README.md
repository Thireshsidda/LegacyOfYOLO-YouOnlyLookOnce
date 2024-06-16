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

## PANet (Path Aggregation Network):
PANet was used in YOLOv4. The aim of using PANet was to create a rich multi-stage feature hierarchy for robust object detection. The only modification is that PANet concatenates the feature maps instead of adding them, as mentioned in the original paper. CSP-PANet is the same as PANet. Instead, they add a few `CSP layers in between the PANet`. This change results in a processing speed that is more than twice as fast.

## AutoAnchors:
YOLOv5 also incorporated another improvement which is called `AutoAnchor`. In AutoAnchor, they change the shape of the anchors during training. First, the model starts with the prior anchor boxes generated from running k-means on the ground truth bounding boxes. Later these bounding boxes get updated using the `genetic evolution (GE) algorithm`. GE algorithm develops these anchors across 1,000 generations, employing CIoU loss and the Best Possible Recall for its fitness evaluation.

## Bag of freebies:
Apart from this bag of special methods, they also used the bag of freebies methods and data augmentations such as `Mosaic, copy-paste, random affine, MixUp, HSV` augmentation, etc. I remember from the TensorFlow – Help Protect the Great Barrier Reef Kaggle competition, where people suddenly started training YOLOv5 on high-resolution images, which resulted in higher Leader Board scores. However, it was later tested that using a larger input size of 1536 pixels and test-time augmentation (TTA), YOLOv5 achieves an AP of 55.8%.


## Training Strategies
YOLOv5 applies several sophisticated training strategies to enhance the model's performance. They include:

- **Multiscale Training:** The input images are randomly rescaled within a range of 0.5 to 1.5 times their original size during the training process.
- **AutoAnchor:** This strategy optimizes the prior anchor boxes to match the statistical characteristics of the ground truth boxes in your custom data.
- **Warmup and Cosine LR Scheduler:** A method to adjust the learning rate to enhance model performance.
- **Exponential Moving Average (EMA):** A strategy that uses the average of parameters over past steps to stabilize the training process and reduce generalization error.
- **Mixed Precision Training:** A method to perform operations in half-precision format, reducing memory usage and enhancing computational speed.
- **Hyperparameter Evolution:** A strategy to automatically tune hyperparameters to achieve optimal performance.


## Compute Losses
The loss in YOLOv5 is computed as a combination of three individual loss components:

- **Classes Loss (BCE Loss):** Binary Cross-Entropy loss, measures the error for the classification task.
- **Objectness Loss (BCE Loss):** Another Binary Cross-Entropy loss, calculates the error in detecting whether an object is present in a particular grid cell or not.
- **Location Loss (CIoU Loss):** Complete IoU loss, measures the error in localizing the object within the grid cell.

## Training YOLOv5:
YOLOv5 stands out as a top-performing version in the YOLO series, and mastering its training on custom datasets is essential. Training YOLOv5 on the vehicle OpenImage dataset serves as a practical example. The transition of the model from the darknet framework to Pytorch by Glenn Jocher has simplified the training process significantly. For those looking to delve deeper, “YOLOv5 – Custom Object Detection Training” offers comprehensive insights into custom training techniques. Additionally, Ultralytics has recently introduced the capability to train instance segmentation models using the YOLOv5 repository, a feature explored in detail in “YOLOv5 Instance Segmentation“.

## Conclusion
In conclusion, YOLOv5 represents a significant step forward in the development of real-time object detection models. By incorporating various new features, enhancements, and training strategies, it surpasses previous versions of the YOLO family in performance and efficiency.

The primary enhancements in YOLOv5 include the use of a dynamic architecture, an extensive range of data augmentation techniques, innovative training strategies, as well as important adjustments in computing losses and the process of building targets. All these innovations significantly improve the accuracy and efficiency of object detection while retaining a high degree of speed, which is the trademark of YOLO models.
