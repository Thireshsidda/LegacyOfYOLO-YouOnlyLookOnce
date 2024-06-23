# YOLO-NAS

## Paper Summary
In May 2023, an Israel-based company Deci, published their latest YOLO variant called YOLO-NAS, the current state-of-the-art object detection model. YOLO-NAS is tailored for efficiently detecting small objects and boasts enhanced localization precision. It also improves the performance-to-compute ratio, making it ideal for real-time edge-device applications. 

They introduced a few quantization-aware modules, such as QSP and QCI. The architecture was generated through AutoNAC, which is Deci’s proprietary NAS technology. NAS means Neural Architecture Search. YOLO-NAS models incorporate attention mechanisms and reparameterization during inference to enhance their ability to detect objects.
- The YOLO-NAS models initially underwent pre-training on the Object365 benchmark dataset, which contains 2 million images across 365 categories. 

- They were further pre-trained using a method called pseudo-labeling on 123,000 unlabeled images from the COCO dataset. 

- Additionally, techniques like **Knowledge Distillation (KD)** and Distribution Focal Loss (DFL) were employed to refine and enhance the training process of the YOLO-NAS models.

- They generated different variants of YOLO-NAS by varying the depth and positions of the **QSP and QCI blocks**.

- YOLO-NAS and **AutoNAC** are very highly sophisticated algorithms, YOLO-NAS Object Detection Model serves as an informative resource to comprehensively understand the intricacies of this model.


## Training YOLO-NAS
YOLO-NAS, one the newest members of the YOLO family, is essential for those looking to Train YOLO-NAS on a custom dataset. We have illustrated this process using a Thermal Dataset.


## Introducing YOLO-NAS Pose: A Leap in Pose Estimatioon Technology
![YOLO-NAS-Pose](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/61cef4e1-b807-4dfe-9ebe-58175fea9111)

**YOLO-NAS Pose**

**YOLO-NAS Pose models** is the latest contribution to the field of Pose Estimation. Earlier this year, Deci garnered widespread recognition for its groundbreaking object detection foundation model, YOLO-NAS. Building upon the success of YOLO-NAS, the company has now unveiled YOLO-NAS Pose as its Pose Estimation counterpart. This Pose model offers an excellent balance between latency and accuracy.

Pose Estimation plays a crucial role in computer vision, encompassing a wide range of important applications. These applications include monitoring patient movements in healthcare, analyzing the performance of athletes in sports, creating seamless human-computer interfaces, and improving robotic systems.


## YOLO-NAS Pose Model Architecture
The traditional Pose Estimation models follow one of the two approaches:
- 1.Detect all the persons in the scene, then estimate its keypoints and create the pose. A two-stage top-down process.
  
- 2.Detect all the keypoints in the scene, then generate the pose. A two-stage bottom-up process.

YOLO-NAS Pose does things differently compared to the traditional Pose Estimation models. Instead of first detecting the person and then estimating their pose, it can detect and estimate the person and their pose all at once, in a single step.

![YOLO-NAS-Pose-Architecture-1-768x705](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/3cdc1281-c9bf-4edf-9f14-7d94b2756a78)

**Figure 1: YOLO-NAS Pose Architecture – Backbone and Neck Design**

The Pose models are built on top of the YOLO-NAS object detection architecture. Both the Object Detection models and the Pose Estimation models have the same backbone and neck design but differ in the head. The head for YOLO-NAS Pose is designed for its multi-task objective, i.e., detecting a single class object (like a person or an animal) and estimating the pose of the object

![YOLO-NAS-Pose-Architecture-Head-Design-768x432](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/ddcecf48-9e27-40a5-9508-2040e6c10051)

**Figure 2: YOLO-NAS Pose Architecture – Head Design**

This impressive combination is the result of Deci’s proprietary Neural Architecture Search (NAS) engine, AutoNAC. It navigates the vast architecture search space and returns the best architectural designs. The following are the hyperparameters for the search:
- The number of Conv-BN-Relu blocks for both pose and box regression paths.

- The number of intermediate channels for both paths

- The decision between a shared stem for pose/box regression or distinct stems

The result speaks for itself.

![YOLO-NAS-Pose-Video-Result](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/6198e256-290a-49a7-a4dc-54698fa7b5f4)

**Figure 3: YOLO-NAS Pose Estimation**


The YOLO-NAS Pose models are evaluated on the COCO Val 2017 Dataset. The model’s accuracy and latency are state-of-the-art. The nano model is the fastest and reaches inference up to 425fps on a T4 GPU. Meanwhile, the large model can reach up to 113 fps.

If we look at edge deployment, the nano and medium models will still run in real-time at 63fps and 48 fps, respectively. But when we look at the medium and large models deployed on Jetson Xavier NX, the speed starts dwindling and reaches 26fps and 20fps, respectively. These are still some of the best results available.


### How were The Pose Models Trained?

## YOLO-NAS Pose Loss Function
To make sure the model learned both tasks effectively, Deci improved the loss functions that were used in training. Instead of just considering the IoU (Intersection over Union) score for assigned boxes, we also incorporated the Object Keypoint Similarity (OKS) score, which compares predicted key points to the actual ones. This change encourages the model to make accurate predictions for both bounding boxes and pose estimation.

Moreover, a direct OKS regression technique was used, which surpasses the traditional L1/L2 loss methods. This approach offers several advantages:

- It operates within a range of 0 to 1, similar to box IoU, indicating how similar the poses are.

- It takes into account the varying levels of difficulty in annotating specific keypoints. Each keypoint is associated with a unique sigma score, which reflects the accuracy of annotation and dataset specifics. The score determines how much the model is penalized for making inaccurate predictions.

- Using a loss function that aligns with the validation metric, which in turn allows for targeting and optimization of the metric.

## Training Hyperparameters
Since, YOLO-NAS Pose employs a similar foundational structure as the YOLO-NAS model the pre-trained weights from YOLO-NAS were used to initialize the backbone and neck of our model before proceeding with the final training. Here are the training hyperparameters:

- Training Hardware: Used 8 NVIDIA GeForce RTX 3090 GPUs with PyTorch 2.0.

- Training Schedule: Training was conducted for up to 1000 epochs, with early stopping if there was no improvement in performance over the last 100 epochs.

- Optimizer: Employed AdamW with Cosine LR (Learning Rate) decay, reducing the LR by a factor of 0.05 towards the end of training.

- Weight Decay: A weight decay factor of 0.000001 was applied, excluding bias and BatchNorm layers.

- EMA (Exponential Moving Average) Decay: Used a beta factor of 50 for EMA decay.

- Image Resolution: Images were processed to have a maximum side length of 640 pixels and were padded to a resolution of 640×640 with a padding color of (127, 127, 127).

Augmentations like mosaic data augmentation, random 90-degree rotations, and color augmentations further improved AP by 2.

## Conclusion
YOLO-NAS Pose is one of the best Pose Estimation models right now. In this post, we have briefed about the various models, understood the YOLO-NAS Pose model architecture and AutoNAC, and run inference using SuperGradients.



