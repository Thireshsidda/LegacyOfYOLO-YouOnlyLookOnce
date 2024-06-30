# YOLOv10: The Dual-Head OG of YOLO Series


The classy YOLO series has a new iteration, **YOLOv10**, a new object detection model. The YOLO series is one of the most used models in the computer vision industry. So, what is YOLOv10? We will explore the answer throughout this `README`. Whether you are a beginner or an expert, you will have an overview of the entire YOLOv10 architecture, workflow, and real-time inference with some cool results.


**Table of Contents**

- 1. What is YOLOv10?
- 2. YOLO Master Post – Every Model Explained
- 3. Components of YOLOv10
- 4. YOLOv10 – Range of Models
- 5. Inference using YOLOv10
- 6. YOLOv8 vs YOLOv9 vs YOLOv10
- 7. YOLOv10 - Benchmarks
- 8. Key Takeaways
- 10. Conclusion
- 11. Reference
 
### i) What is YOLOv10?
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

Using the consistent matching metric helps both heads train better together, improving the one-to-one head’s predictions during inference. This metric reduces the supervision gap between the two heads by aligning their training targets. By setting $alpha_{o2o} = alpha_{o2m}$ and , both heads pick the same best samples. This alignment has improved performance, as seen by the number of matching pairs between the one-to-one and one-to-many heads after training. This approach leads to better model performance without needing extensive tuning.



