# YOLOv2

## Paper Summary
YOLOv2 was published at CVPR, in 2017, by the same YOLOv1 authors. They slightly modified the YOLOv1 architecture and improved the training process to make YOLOv2 faster and more accurate. Here are the changes that enhanced YOLOv2.

## Using High-Resolution Images:
They trained their classification network at 448×448 in ImageNet for 10 epochs. This helps the network to adjust the filters to better work with high-resolution images. Later, the resulting network is trained on detection tasks, and by doing that, they promote joint training that helps train object detectors in both object detection and classification data. Their objective mainly was improving recall and localization while maintaining classification accuracy. They use 416×416 instead of 448×448 so that feature maps are sized in odd numbers, and the object center should lie at one point instead of multiple locations. YOLO’s convolution layers `downsample the image by a factor of 32`, so by using an input image of 416, we get an output feature map of 13 × 13.

## Introduction to Anchor Boxes:
Generally, `the box dimensions are hand-picked`, which is only sometimes a good prior. To solve this, the authors proposed using k-means clustering on all the bounding boxes of the dataset. This gives them the most dominant sizes of the bounding boxes from the dataset. But, in k-means, using Euclidean distance will produce a higher error(distance) for large boxes and a more minor error(distance) for small boxes. But, as the IOU score is independent of the size of the box, the distance measurements needed to be changed. They proposed, 

![image-20231207180717793](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/09fce721-52bb-4268-b244-953dbdde99eb)

**Figure 1: YOLOv2 K-Means Clustering Distance Metric for bounding box prior** [(Source)](https://arxiv.org/abs/1612.08242)

This part of the equation implies that the distance is inversely proportional to the IOU. Subtracting the IOU from 1 means that the distance decreases as the IOU increases (indicates higher overlap or similarity), and vice versa. We run k-means for various values of k and plot the average IOU with the closest centroid, see Figure 6. We choose k = 5 as a good tradeoff between model complexity and high recall.

![Screenshot-from-2023-12-26-13-15-24](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/96477243-e744-4d50-bc0d-75559bb8819a)

**Figure 2: Clustering of box dimensions on VOC and COCO** [(Source)](https://arxiv.org/abs/1612.08242)

## Fine-grained features:
Like YOLOV1, YOLOV2 also predicts the bounding box coordinates relative to the grid cell. The modified YOLO predicts a 13×13 feature map, and while this helps detect large objects, having a fine-grained feature map might help detect small objects. Many detection models have different approaches, but in YOLOv2, the authors proposed a passthrough layer that concatenates features from a higher resolution layer to a lower resolution layer. When concatenating a higher-resolution image with a lower-resolution image, it internally restructures the higher-resolution image such that the special dimension reduces, but the depth increases. This means that if given a 26x26x512 feature map, it gets resized as 13x13x2048; here, the channel dimension 2048 comes from 512 x 2 x 2 = 2048. 

## Location Prediction:
The diagram below explained as,

![Screenshot-from-2023-12-26-13-18-53](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/5f38cf97-64d4-4f7e-9fde-0bb08becd746)

**Figure 3: bound box(relative to grid cell) parameter equations** [(Source)](https://arxiv.org/abs/1612.08242)

Here, $c_x$, $c_y$ is the grid cell center. $b_x$, $b_y$ are the coordinates of the center of the predicted bound box relative to the grid cell. $b_w$ and $b_h$ are the width and height of the predicted bounding box, which is an offset of the prior anchor box associated with the grid. σ ensures the center point ($b_x​$, $by$​) stays within the grid cell by restricting the values between 0 and 1. $e^{t_w}$​ and $e^{t_h}$​ represent the exponential transformation of predicted width and height offsets $t_w​$ and $t_h$​, ensuring that the width and height stay positive, although using an exponential transform is mathematically unstable.

## Multiscale training:
They introduced multiscale training to make the model more robust. After every 10 batches, the model takes a randomly selected image dimension and continues training. Since their model downsamples by a factor of 32, they pull from the following multiples of 32: {320; 352; …; 608}.

## Hierarchial Classification:
The model is first trained for classification on ImageNet before even training detection. For that, they hierarchically prepare their data. Hierarchical Classification improves the model accuracy by leveraging the structure of class label’s hierarchically, the model can better understand the relationships between different classes. 
