# YOLOv10: The Dual-Head OG of YOLO Series


The classy YOLO series has a new iteration, **YOLOv10**, a new object detection model. The YOLO series is one of the most used models in the computer vision industry. So, what is YOLOv10? We will explore the answer throughout this `README`. Whether you are a beginner or an expert, you will have an overview of the entire YOLOv10 architecture, workflow, and real-time inference with some cool results.


**Table of Contents**

- 1) What is YOLOv10?
- 2) YOLO Master Post – Every Model Explained
- 3) Components of YOLOv10
- 4) YOLOv10 – Range of Models
- 5) Inference using YOLOv10
- 6) YOLOv8 vs YOLOv9 vs YOLOv10
- 7) YOLOv10 - Benchmarks
- 8) Key Takeaways
- 10) Conclusion
- 11) Reference
 
### What is YOLOv10?
Three months back, Chien-Yao Wang and his team released YOLOv9, the 9th iteration of the YOLO series, which includes innovative methods such as Programmable Gradient Information (PGI) and Generalized Efficient Layer Aggregation Network (GELAN) to address issues related to information loss and computational efficiency effectively. But just like all other YOLOs, its reliance on the non-maximum suppression (NMS) for post-processing hampers the end-to-end deployment of the model and adversely impacts the inference latency. Additionally, the design of various YOLO components lacks comprehensive inspection, leading to unnecessary computation and reducing the model’s effectiveness.

![image11](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/d4afe36b-b0f9-4dc5-af0c-9cb314b6d974)

**Figure 1: YOLOv10 Inference**

So, like all other YOLOs, **Ao Wang, Hui Chen, et al.[1]** introduce the latest version of **YOLO(v10)** with some cool new features. So, what's new is YOLOv10? YOLOv10 comes with two main upgrades over previous YOLOs: a Consistent Dual Assignments for NMS-free Training and an Efficiency-Accuracy Driven Model Design to improve the overall performance. In the next section, let’s dive deep into these components to understand the root workflow of YOLOv10.


## Components of YOLOv10

YOLOv10 consists of two main components:
- 1) NMS free training and inference
- 2) Efficiency-Accuracy Driven Model Design


Let's explore both of them one by one:
**Consistent Dual Assignments for NMS-free Training**

![image18](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/7cef810a-a510-4b7b-8f1a-c758839e5715)

**Figure 2: NMS Workflow**

In traditional YOLOs, During training, they usually leverage **TAL(Task Alignment Learning)** to allocate multiple positive samples for each instance and non-maximum suppression (NMS) to remove redundant bounding boxes for the same object in post-processing. However, NMS can sometimes be a bit of a blunt instrument, potentially discarding useful predictions or failing to remove all duplicates efficiently. Also, it adds more computational cost during the time of training and inference. **DETR (DEtection TRansformer)**  introduces an elegant way to handle this issue by using the Hungarian algorithm for one-to-one matching during training, thereby eliminating the need for NMS during inference. 

![image28](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/17404472-ae4d-4e83-ae13-fd83ec4ddc42)

**Figure 3: YOLOv10 Model Workflow**

YOLOv10  authors introduced A new architecture to tackle the NMS post-processing with the help of DETR Architecture. It consists of two main components: a dual label assignment using two heads and a consistent matching metric to match both head predictions for a perfect prediction. Now, let’s move to the next section to understand both methods in detail.

## Dual Label Assignments
YOLOv10 combines the benefits of one-to-one and one-to-many matching strategies used during the training of object detection models. One-to-one matching assigns only one prediction to each ground truth, simplifying post-processing by eliminating the need for Non-Maximum Suppression (NMS). However, this approach can lead to weaker supervision, resulting in lower accuracy and slower convergence during training. To address this, YOLOv10 gives a dual-label assignment strategy that uses both one-to-many and one-to-one heads during training.

![image29](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/4e347e49-df64-4695-b7dd-e79290613068)

**Figure 4: Dual Label Assignment**

- **One-to-Many Head:** This head retains the original structure and optimization objectives and benefits the model by dense supervision. During training, it uses one-to-many assignments, which provide rich  supervisory signals(features) to the model.

-  **One-to-One Head:** This head uses a one-to-one matching strategy for label assignments, ensuring each ground truth is matched with a single prediction. It operates similarly to the Hungarian matching method but requires less training time. Using one-to-one matching eliminates the need for Non-Maximum Suppression (NMS) during inference, streamlining the prediction process.

In training, both heads are used simultaneously, allowing the backbone and neck of the model to leverage the comprehensive supervision from the one-to-many assignments. This improves the model’s learning and accuracy. The one-to-many head is discarded during inference, and only the one-to-one head is used for making predictions. This approach ensures that the model can be deployed end-to-end without additional computational costs, maintaining high efficiency in real-time object detection tasks. 

## **Consistent Matching Metric**

A consistent matching metric is used to improve YOLOv10’s dual label assignments. This metric ensures that both the one-to-one and one-to-many heads align during their training. The matching metric is defined as

![image23](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/69658330-46d7-4035-9e49-498be2cf495a)

**Figure 5: Metric Equation**

where 𝑝 is the classification score, $\hat{b}$ and 𝑏 are the prediction and instance bounding boxes, and 𝑠 shows if the prediction’s anchor point is within the instance. The parameters 𝛼 and 𝛽 balance the importance of classification and localization tasks. The one-to-many and one-to-one metrics are denoted as $m_{o2m} = m(\alpha_{o2m}, \beta_{o2m})$ and $m_{o2o} = m(\alpha_{o2o}, \beta_{o2o})$ respectively. By using the same metric for both heads, the model ensures the best samples chosen by the one-to-many head are also the best for the one-to-one head.

![image10](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/dbd9ff43-11c7-49b1-817b-c6fa0e3415b6)

**Figure 6: Matching Metric Workflow**

Using the consistent matching metric helps both heads train better together, improving the one-to-one head’s predictions during inference. This metric reduces the supervision gap between the two heads by aligning their training targets. By setting $\alpha_{o2o}$ = $\alpha_{o2m}$ and $\beta_{o2m}$ = $\beta_{o2o}$ both heads pick the same best samples. This alignment has improved performance, as seen by the number of matching pairs between the one-to-one and one-to-many heads after training. This approach leads to better model performance without needing extensive tuning.


### Efficiency-Accuracy Driven Model
In addition to post-processing, YOLO models face big challenges in balancing efficiency and accuracy. While various design strategies have been used, there has been a lack of thorough examination of all components in YOLOs. This leads to unnecessary computational load and limits the model’s capabilities. YOLOv10 focuses on balancing efficiency and accuracy in object detection. This involves a series of optimizations in both the model architecture and training processes to maximize performance while minimizing computational costs. Let’s explore both.

### Efficiency-driven Model Design
To achieve higher efficiency, YOLOv10 introduces several innovations that reduce computational overhead without sacrificing performance: A lightweight classification head, spatial channel downsampling, and rank-guided block design. Let’s understand each of the components one by one.

### Lightweight Classification Head
![image21](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/72faaaf2-c067-42bb-a549-76840328b51f)

**Figure 7: YOLO's Architecture Overview**

In traditional YOLO models, the classification and regression heads share the same architecture, resulting in high computational costs, particularly for the classification head.

- **Classification Head** -  identifies the class of each detected object, such as ‘person’ or ‘car.’ It also estimates the probability for each class, ensuring the sum of probabilities is one.
- **Regression Head** - predicts the bounding box coordinates for detected objects, including the center coordinates, width, and height. It also provides a confidence score for each prediction to help filter out low-confidence results.

By analyzing the impact of errors, researchers found that the regression head is more critical for performance. Thus, YOLOv10 adopts a lightweight classification head using two depthwise separable convolutions followed by a 1×1 convolution, significantly reducing the overhead.

## Spatial-Channel Decoupled Downsampling

![image4](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/5176efa8-4884-4495-97c2-f3aa86af9c09)

**Figure 8: Convolution Process**

Typical YOLO models use 3×3 convolutions with a stride of 2 for both spatial downsampling(from H × W to H/2 × W/2 ) and channel transformation(from C to 2C), which is computationally expensive. YOLOv10 decouples these operations for greater efficiency. First, a pointwise convolution adjusts the channel dimensions, followed by a depthwise convolution to reduce the spatial dimensions.

![image16](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/1487f012-c9bd-4653-94aa-9fa03699f5d8)

**Figure 9: Depthwise and Pointwise Conv. (source-medium)**

In short, YOLOv10 uses pointwise convolution (1×1 filter) to increase the number of channels and depthwise convolution to reduce the spatial dimensions rather than doing both simultaneously in a single convolution layer. This decoupling reduces computational costs while retaining more information during downsampling. Specifically, the computational cost decreases from $O(\frac{9}{2}HWC^{2}){t0}O(2HWC^{2}+\frac{9}{2}HWC){,}$  and the param counts are reduced from $\\
O(18C^{2})_{t0}O(2C^{2}+18C)$. This approach maximizes information retention and leads to high performance with reduced latency.


### Rank - Guided Block Design
YOLOs often use the same block structure across all stages, which can be inefficient and cause bottlenecks. Besides YOLOv9 PGI and GELAN, YOLOv10  introduces a rank-guided block design to address redundancy. It calculates the intrinsic rank of each stage(the last convolution in the last basic block in each stage), identifies redundant stages, and replaces their basic blocks with a compact inverted block (CIB) structure. 

![image24](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/7c1fae73-deed-49d5-9d7e-3a0a43f0fb22)

**Figure 10: CIB Block**

CIB uses depthwise convolutions for spatial mixing and pointwise convolutions for channel mixing. It can be an efficient basic building block, like an embedded part in the ELAN structure(and in the GELAN of YOLOv9). This adaptive strategy first sorts all stages on their intrinsic ranks in ascending order. Then, it replaces the basic blocks(with lower rank) with the more efficient CIB block. The stages are sorted by rank, and these replacements are progressively made to improve performance.

### Accuracy-Driven Model Design

To boost accuracy, YOLOv10 comes with two innovative approaches: large-kernel convolution in CIB and partial self-attention. Let’s explore both in the next section.

#### Large-Kernel Convolution

![image20-1024x576](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/bc57983b-4bdb-4c82-9339-4fcb882aac13)

**Figure 11: Different Shapes of Kernel Used in Convolution of YOLO Models**

Large-kernel depthwise convolutions are used to enlarge the convolution area size, improving the model’s ability to detect detailed features. YOLOv10 uses these large-kernel convolutions in CIB within deeper stages of the model, specifically increasing the kernel size of the second 3×3 depthwise convolution to 7×7. Additionally, structural reparameterization techniques add another 3×3 depthwise convolution branch to alleviate optimization issues without adding inference overhead. This method improves detection performance, especially for small objects, while keeping the computational load manageable.

![image17](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/90f10f03-6c4b-4004-844e-ce8ef4705011)

**Figure 12: PSA Block**

**Self-attention**, while powerful, is computationally intensive. YOLOv10 introduces an efficient partial self-attention (PSA) module to incorporate global representation learning with reduced computational costs. Features are split into two parts after a 1×1 convolution; only one part is processed into the N_PSA blocks comprised of a multi-head self-attention (MHSA) and feed-forward network (FFN). The parts are then concatenated and fused by another 1×1 convolution. PSA is applied only in later stages with lower resolution, avoiding excessive overhead from the quadratic complexity of self-attention. This approach enhances the model’s overall capability and performance with minimal computational cost.


## Architecture Overview
The researchers of YOLOv10 hasnt provided the complete architecture diagram as of now. But we can have an architecture overview by understanding the codebase. Let’s explore:

```
# Parameters
nc: 80 # number of classes
scales: # model compound scaling constants, i.e. 'model=yolov8n.yaml' will call yolov8.yaml with scale 'n'
  # [depth, width, max_channels]
  b: [0.67, 1.00, 512] 
 
# YOLOv8.0n backbone
backbone:
  # [from, repeats, module, args]
  - [-1, 1, Conv, [64, 3, 2]] # 0-P1/2
  - [-1, 1, Conv, [128, 3, 2]] # 1-P2/4
  - [-1, 3, C2f, [128, True]]
  - [-1, 1, Conv, [256, 3, 2]] # 3-P3/8
  - [-1, 6, C2f, [256, True]]
  - [-1, 1, SCDown, [512, 3, 2]] # 5-P4/16
  - [-1, 6, C2f, [512, True]]
  - [-1, 1, SCDown, [1024, 3, 2]] # 7-P5/32
  - [-1, 3, C2fCIB, [1024, True]]
  - [-1, 1, SPPF, [1024, 5]] # 9
  - [-1, 1, PSA, [1024]] # 10
 
# YOLOv8.0n head
head:
  - [-1, 1, nn.Upsample, [None, 2, "nearest"]]
  - [[-1, 6], 1, Concat, [1]] # cat backbone P4
  - [-1, 3, C2fCIB, [512, True]] # 13
 
  - [-1, 1, nn.Upsample, [None, 2, "nearest"]]
  - [[-1, 4], 1, Concat, [1]] # cat backbone P3
  - [-1, 3, C2f, [256]] # 16 (P3/8-small)
 
  - [-1, 1, Conv, [256, 3, 2]]
  - [[-1, 13], 1, Concat, [1]] # cat head P4
  - [-1, 3, C2fCIB, [512, True]] # 19 (P4/16-medium)
 
  - [-1, 1, SCDown, [512, 3, 2]]
  - [[-1, 10], 1, Concat, [1]] # cat head P5
  - [-1, 3, C2fCIB, [1024, True]] # 22 (P5/32-large)
 
  - [[16, 19, 22], 1, v10Detect, [nc]] # Detect(P3, P4, P5)
```

### Backbone

The backbone extracts features from images. YOLOv10 uses an improved version of CSPNet (Cross Stage Partial Network) to improve the flow of information and use less computing power.

### Neck

The neck combines features from different backbone levels and passes them to the head. It effectively mixes features from multiple scales(layers) using PAN (Path Aggregation Network) layers.

### One-to-Many Head

During training, this head makes several predictions for each object. This gives the model lots of information(features), helping it learn better.

### One-to-One Head

During inference, this head makes a single best prediction for each object. This removes the need for NMS (Non-Maximum Suppression), making the process faster.

YOLOv10 – Range of Models

YOLOv10 comes with six variants of models depending on the size and efficiency –

![image27-768x432](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/61409a12-da1c-469c-88b2-ced0f8b0cb2b)

**Figure 13: YOLOv10 Model Variations**

- **YOLOv10-N:** Nano for small and lightweight tasks.
- **YOLOv10-S:** Small, upgrade of Nano with some extra accuracy.
- **YOLOv10-M:** Medium for general-purpose use.
- **YOLOv10-B:** Balanced with increased width of Medium for improved accuracy.
- **YOLOv10-L:** Large for higher accuracy with higher computation.
- **YOLOv10-X:** Extra-large for maximum accuracy and performance.
