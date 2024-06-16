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

**Figure 2: Clustering of box dimensions on VOC and COCO [(Source)](https://arxiv.org/abs/1612.08242)
