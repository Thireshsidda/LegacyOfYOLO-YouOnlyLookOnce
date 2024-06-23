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
[Source](https://www.geeksforgeeks.org/object-detection-vs-object-recognition-vs-image-segmentation/)

There are numerous real-life applications for object detection. For example, in the field of Autonomous Vehicles, it is used for detecting vehicles, pedestrians, road delimiter prediction, HD-Map creation, traffic lights, and traffic signs, etc. In surveillance and monitoring, it is used in detecting trespassers, vehicle license plates, face mask detection, weapon detection etc. in biometric attendance systems. In medical imaging, its used for detecting certain cells, cancer detection, tumor detection, etc. and the list goes on. 


## Object Detection using Classical Computer Vision and Deep Learning
Earlier, detection algorithms used to be based on classical computer vision techniques such as template matching, Haar Cascade, Feature detection and matching using SIFT or SURF, HOG Detector, **Deformable Part-based Model (DPM)**, etc. However, most of these methods are very specific to the use case and don't have generalization capabilities, which redirected the research toward deep learning-based methods. The Overfeat paper was a pioneer in deep learning-based object detection as it was a single network model employed to perform object classification and localization.

The Overfeat architecture closely resembles the AlexNet architecture. It does image classification on different scales in a sliding window fashion and carries out bounding box regression on the same CNN layer. Later other models like RCNN, FastRCNN, SPPNet, YOLO etc. emerged in this field.  


## One-Stage Vs. Two-Stage Detectors
Initial models were two-stage detectors, which means there was a step for region proposals and finding out the region of interest. It was also a step for predicting the bounding box coordinates and class using the proposed region. This was very innovative, yet it demanded substantial computational resources and resulted in considerable delays during the inference phase. Later the YOLO authors proposed a single-stage strategy that overlooked the Region Proposal step and ran detection directly over the entire image. This helped speed up the training and inference, preserve global context and keep the accuracy on par.

## Challenges of Object Detection
**1. Occlusion and Small Objects**
Small objects are always difficult to detect because models get little information about them, or the dataset might not have many instances. This issue comes under the scope of shape invariance problem. Additionally, occlusion and partially visible objects make it hard for the model to detect small objects.

**2.Global Context and Local Contexts**
Global context is as important as local context for a model. Global context means the usual surroundings of the object, e.g. traffic light is mostly seen on the side of the road, or it might have cars or pedestrians nearby. The meaning of Local context is the object’s geometrical structure, texture, and colors. For instance, traffic lights typically appear rectangular, containing lights of various colors, red, green, or yellow. For the shape invariance property, a well-trained model doesn’t confuse a red light in a car’s rear as a red light from a traffic light. However, if the data is not properly curated, this issue might occur.



# Introduction to YOLO
## Chronology Of YOLO Models: Evolution and Milestones
At CVPR 2016, Joseph Redmon, along with researchers from FAIR (Facebook AI Research) and the Allen Institute for AI, published the seminal paper on YOLO (You Only Look Once). It was state-of-the-art, single-stage object detector at that time. Using the same concept of single-stage detection along with some significant changes, people published their own models, such as SSD, RetinaNet, etc. 

At the 2017 CVPR, Joseph Redmon and Ali Farhadi published the 2nd iteration of the YOLO model as YOLOv2, built on top of YOLOv1, integrating some advancements of that time to make it faster and more accurate. Redmon improved the architecture by adding a Neck and using a bigger backbone and published a very casually written paper in ArXiv in 2018.

Two years later, in April 2020, other authors used the YOLO name to publish version 4 of the model, and a lot of significant changes were made. Two months later, Ultralytics open-sourced the YOLOv5 model but didn’t publish any paper. YOLOv5 contained a lot of improvements from the YOLOV4 model, no paper was published, and it marked the first instance of a YOLO model that did not use the DarkNet framework for its development. In the same year, YOLOv4 authors published another paper named Scaled-YOLOv4 which contained further improvements on YOLOv4. In the following year, 2021, YOLOR and YOLOX were published. Skipping version 6, in 2022, the authors of YOLOv4 published the YOLOv7, which was the state of the art at that time in terms of speed and accuracy. In the same year, researchers from Meituan Vision published the YOLOv6, which was better than version 7.

In January 2023, Ultralytics open-sourced the YOLOv8 with semantic segmentation capabilities alongside detection. In May 2023, Deci AI came up with YOLO-NAS, an algorithm-generated architecture that surpassed all the predecessors of YOLO.

**This is the timeline of YOLO Models**

![th](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/432260bb-fa27-4a45-9ada-3144cf3c0326)

# YOLO LEGACY

# Summary and Conclusion

The whole YOLO series is full of engineering innovations and breakthroughs, It offers numerous applied machine learning ideas that can be used in specific scenarios. As Convolution Blocks are the basis for the entire YOLO series of models and as YOLO is mainly designed for edge devices, by going through all the YOLO models, one can learn different CNN optimization techniques.
  
  - When YOLOv1 was first introduced, it was revolutionary as it pioneered the concept of a single-stage detection model. It stood out for not incorporating any region proposal mechanisms, allowing it to analyze the entire image at once and efficiently capture both global and local context.
  
  - Later, the YOLOv2 and YOLOv3 models integrated advanced techniques that emerged at that time, such as the concept of **Feature Pyramid Networks (FPN)**, multi- scale training, and anchor boxes.
  
  - But, YOLOv4 was different, the authors went above and beyond to bring some ground-breaking changes to YOLO. They introduced changes in the architecture, the       Darknet53, which became **CSPDarknet53-PANet-SPP**, techniques such as bag of specials and freebies, Genetic Evolution algorithms, Attention modules, and so on.
  
  - YOLOv5 brought changes that were very minimal and included most of the techniques from YOLOv4, what made YOLOv5, YOLOv5 is its Pytorch Implementation and how easy it is to train a Model using the Ultralytics. 
  
  - In YOLO-R, the authors experimented with a novel approach and worked toward evolving the model to support **multi-task learning**.
  
  - But, in YOLO-X, it was again back to integrating new advances to the old YOLOv3 model, such as **Anchor-free, Decoupled heads**, Lable assignment and strong       augmentations etc. 
  
  - In YOLOv7 and YOLOv6 authors experimented with the model architecture, YOLOv7 first brought the concept of re-parameterization in YOLO and model scaling, and  YOLOv6 contributed by adding distillation and quantization techniques.  
  
  - Looking at YOLOv8 and its performance, previous models might seem over-engineered because it was built on top of YOLOv5 with minimal changes, such as replacing    CSPLayer with C2f module, adding better loss functions that help the model to deal with small objects. 
  
  - YOLO-NAS was an algorithm-generated model with quantization blocks that made the model faster and more accurate.


## References

[1] [You Only Look Once: Unified, Real-Time Object Detection](https://arxiv.org/abs/1506.02640)

[2] [YOLO9000: Better, Faster, Stronger](https://arxiv.org/abs/1612.08242)

[3] [YOLOv3: An Incremental Improvement](https://arxiv.org/abs/1804.02767)

[4] [YOLOv4: Optimal Speed and Accuracy of Object Detection](https://arxiv.org/abs/2004.10934)

[5] [YOLOv6: A Single-Stage Object Detection Framework for Industrial Applications](https://arxiv.org/abs/2209.02976)

[6] [YOLOv7: Trainable bag-of-freebies sets new state-of-the-art for real-time object detectors](https://arxiv.org/abs/2207.02696)

[7] [YOLO-NAS by Deci Achieves SOTA Performance on Object Detection Using Neural Architecture Search](https://deci.ai/blog/yolo-nas-object-detection-foundation-model/)

[8] [A Comprehensive Review of YOLO: From YOLOv1 and Beyond](https://arxiv.org/abs/2304.00501)



## Author

👤 **Thiresh Sidda**

* LinkedIn: [@ThireshSidda](https://www.linkedin.com/in/thiresh-sidda)
* GitHub: [@ThireshSidda](https://github.com/Thireshsidda)
