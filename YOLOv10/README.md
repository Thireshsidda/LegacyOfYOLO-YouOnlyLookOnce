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

where 𝑝 is the classification score, <?xml version='1.0' encoding='UTF-8'?>
<!-- Generated by CodeCogs with dvisvgm 3.0.3 -->
<svg version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' width='7.646286pt' height='13.750996pt' viewBox='-.235271 -.227571 7.646286 13.750996'>
<defs>
<path id='g1-98' d='M2.761644-7.998007C2.773599-8.045828 2.797509-8.117559 2.797509-8.177335C2.797509-8.296887 2.677958-8.296887 2.654047-8.296887C2.642092-8.296887 2.211706-8.261021 1.996513-8.237111C1.793275-8.225156 1.613948-8.201245 1.398755-8.18929C1.111831-8.16538 1.028144-8.153425 1.028144-7.938232C1.028144-7.81868 1.147696-7.81868 1.267248-7.81868C1.876961-7.81868 1.876961-7.711083 1.876961-7.591532C1.876961-7.507846 1.78132-7.161146 1.733499-6.945953L1.446575-5.798257C1.327024-5.32005 .645579-2.606227 .597758-2.391034C.537983-2.092154 .537983-1.888917 .537983-1.733499C.537983-.514072 1.219427 .119552 1.996513 .119552C3.383313 .119552 4.817933-1.661768 4.817933-3.395268C4.817933-4.495143 4.196264-5.272229 3.299626-5.272229C2.677958-5.272229 2.116065-4.758157 1.888917-4.519054L2.761644-7.998007ZM2.008468-.119552C1.625903-.119552 1.207472-.406476 1.207472-1.338979C1.207472-1.733499 1.243337-1.960648 1.458531-2.797509C1.494396-2.952927 1.685679-3.718057 1.733499-3.873474C1.75741-3.969116 2.462765-5.033126 3.275716-5.033126C3.801743-5.033126 4.040847-4.507098 4.040847-3.88543C4.040847-3.311582 3.706102-1.960648 3.407223-1.338979C3.108344-.6934 2.558406-.119552 2.008468-.119552Z'/>
<path id='g0-98' d='M3.311582-8.18929L6.563387-6.718804L6.706849-6.981818L3.323537-8.894645L-.059776-6.981818L.071731-6.718804L3.311582-8.18929Z'/>
</defs>
<g id='page1' transform='matrix(1.13 0 0 1.13 -62.974168 -60.913038)'>
<use x='55.580924' y='62.598599' xlink:href='#g0-98'/>
<use x='56.413267' y='65.753425' xlink:href='#g1-98'/>
</g>
</svg>![CodeCogsEqn](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/f5c379ad-179b-4a09-9b8d-6288f1f434c2)
 and 𝑏 are the prediction and instance bounding boxes, and 𝑠 shows if the prediction’s anchor point is within the instance. The parameters 𝛼 and 𝛽 balance the importance of classification and localization tasks. The one-to-many and one-to-one metrics are denoted as  and  respectively. By using the same metric for both heads, the model ensures the best samples chosen by the one-to-many head are also the best for the one-to-one head.

![image10](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/dbd9ff43-11c7-49b1-817b-c6fa0e3415b6)

**Figure 6: Matching Metric Workflow**



