# YOLO-YouOnlyLookOnce

### What is YOLO?
**You Only Look Once (YOLO):** Unified, Real-Time Object Detction is a single-stage object detection model published at CVPR 2016, by Joseph Redmon, famous for having low latency and high accuracy. The entire YOLO series of models is a collection of pioneering concepts that have shaped today's object detection methods.

YOLO Models have emerged as an industry de facto, achieving high detection precision with minimal computational demands. Some `YOLO models` are tailored to align with specific processing capabilities of the device, whether it's a CPU or a GPU. Most YOLO models are designed to cater to different scales, such as small, medium, and large, which can be easily serialized in ONNX, TensorRT, OpenVINO, etc. This gives users the liberty to choose which is best suited for their application.

![65b2c94b8ee9390ea6f25867_Wide 1](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/c226ca4a-0918-469f-98b7-7a61f6ac2678)


There are limited resources on the  internet that go through all the YOLO models, from their inner workings to how to train every model on the data of your choice end-to-end. However in this repo, we will go through all the different versions of YOLO, from the original  YOLO to YOLOv8 and YOLO-NAS, and understand their internal workings, architecture, design choices, improvements, and custom training.


## Introduction to Object Detection
The task of classifying and localizing multiples objects in an image is called `Object Detection`. Until a few years ago, a common approach was to take a CNN that was trained to classify and locate a single object roughly centered in the image, then slide this CNN across the image and make predictions at each step. The CNN was generally trained to predict not only class probabilities and a bounding box, but also an `objectness score` this is the estimated probability that the image does indeed contain an object centered near the middle. This is a binary classification output; it can be produced by a dense output lsyer with single unit, using the sigmoid activation function and trained using binary cross-entropy loss. In layman’s terms, Object detection is defined as Object Localization + Object Classification. Object Localization is the method of locating an object in the image using a bounding box and Object Classification is the method that tells what is in that bounding box. 

![th](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/599c0a34-6e4d-48d9-a0c1-3ecb9369da7b)

**Fig: Difference between Object Classification, localization and detection**
![Source](https://www.geeksforgeeks.org/object-detection-vs-object-recognition-vs-image-segmentation/)
