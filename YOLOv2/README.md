# YOLOv2

## Paper Summary
YOLOv2 was published at CVPR, in 2017, by the same YOLOv1 authors. They slightly modified the YOLOv1 architecture and improved the training process to make YOLOv2 faster and more accurate. Here are the changes that enhanced YOLOv2.

## Using High-Resolution Images:
They trained their classification network at 448×448 in ImageNet for 10 epochs. This helps the network to adjust the filters to better work with high-resolution images. Later, the resulting network is trained on detection tasks, and by doing that, they promote joint training that helps train object detectors in both object detection and classification data. Their objective mainly was improving recall and localization while maintaining classification accuracy. They use 416×416 instead of 448×448 so that feature maps are sized in odd numbers, and the object center should lie at one point instead of multiple locations. YOLO’s convolution layers downsample the image by a factor of 32, so by using an input image of 416, we get an output feature map of 13 × 13.
