# YOLO-R

## Paper Summary
- In mid-2021 a few authors from the YOLOv4 team published YOLO-R. With this paper, the authors started exploring along the lines of multi-task learning.

- The authors found that the feature extracted for a specific task is not generalized enough to be applied to other tasks. This means one can’t directly use the features from a detection model in a segmentation model. To solve this, everything boils down to producing a generalized representation. And this is where **Multi-task Learning(MTL)** comes into the picture. In MLT the model is trained to perform multiple different tasks simultaneously, rather than training task-specific models. By parallel training a model for multiple tasks, one can improve the model’s generalization capabilities. This is because the model leverages the commonalities and differences across the tasks. MLT is also helpful because it reduces the number of model parameters, resulting in fewer FLOPs and a decreased model latency.

- The concept of **YOLOR** is not like usual MLT models, where you see shared representation components, task-specific components, rather, the concept of YOLOR comes from implicit knowledge and explicit knowledge. Knowledge is two types: **explicit knowledge and implicit knowledge**. Humans learn by looking, hearing, tactile, etc., which are a type of explicit knowledge, and they also learn from past experiences, which is categorized as implicit knowledge. Implicit knowledge refers to the knowledge learned in a **subconscious state**.

- In terms of neural networks, there will be a network for learning explicit knowledge, and there will be another model jointly trained with the explicit model for learning implicit knowledge. In this diagram, the authors presented the same concept, 

![image-20231220131024284](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/f1451ea5-f2dd-4c32-902a-60e4c42c21d9)

**Figure 1: YOLOR workflow diagram [(Source)](https://arxiv.org/abs/2105.04206)**

For the Explicit knowledge network, the authors used YOLOv4-Scaled with a slight modification of adding a **reOrg Layer** followed by two convolution layers. In the Implicit network, they used a single convolutional layer with two **learnable priors** for kernel space alignment and prediction refinement. They included object detection, multi-label image classification, and feature embedding for multi-task learning.


# YOLOR - Paper Explanation & Inference - An In-Depth Analysis

In recent years, we have seen tremendous progress in the YOLO series, now hosting both anchor-free and anchor-based object detection models. Instead of focusing solely on architectural changes, YoloR takes a new route. It takes inspiration from how humans combine implicit knowledge with explicit knowledge to tackle unseen tasks. The proposed techniques significantly improve the performance of the **YoloR object detection** models, resulting in them **being ~88%  🚀 faster and better (🎯 57.3% on the COCO test set)** with minimal additional cost.

## Who are the Authors of YoloR?
The authors of YoloR are *Chien-Yao Wang, I-Hau Yeh, and Hong-Yuan Mark Liao*. Along with *Alexey Boschovskiy*, these four authors have been involved in the development of CSPNet, YoloV4(2020), Scaled-YoloV4 (2020), and YoloV7 (2022). YoloV4 was a critically acclaimed paper to which Scaled-YoloV4 made further improvements. Scaled-YoloV4 was an “architecture improvement paper.” **YoloR (2021)** is based on Scaled-YoloV4 models and has its own take on improving the results further.

![YoloR-yolo-series-timeline_v2-768x521](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/68967e02-216c-40d8-844c-27d8feb15769)

**YoloR is the 9th object detection model in the Yolo series and is based on Scaled-YoloV4.**

The authors of the YoloR paper take a “roads untraveled” approach for further improvement of the Yolo series. The empirical and comparison results published by the authors show that the new approach has some strong validity. **Besides, YoloR-D6 (2021 – 57.3%) performs better than the (best) YoloV7-E6E model (2022 – 56.8%) on the COCO test-dev set.**

This is not a diss on the YoloV7. For example, in this validation mAP comparison of YoloV7 with YoloR and other models, you can see the superiority of YoloV7.

They are both excellent papers that use different techniques for improvements and are creditworthy.


## What is YOLO-R?

As we are aware, the Yolo series primarily tackles the problem of object detection. This paper is no different in that aspect. YoloR stands for “You Only Learn One Representation: Unified Network for Multiple Tasks.”  the second part of the title states something new and unexplored in other Yolo versions.

The authors of YoloR aimed to create:

  ***“A unified network that can generate a unified representation to serve various tasks simultaneously.”***

Their goal is to create a single model capable of learning general representations. Then each sub-network can utilize these representations to create suitable sub-representations for solving a task. A task can be classification, detection, pose estimation, object tracking, etc. 

**The origins of YoloR lie in the problem faced during the Multi-Task Joint Learning network training.** When training a single model that can solve multiple tasks, i.e., joint optimization, each sub-network tends to pull the weights in the direction suitable to itself. This often leads to the poor generation of general features, causing the final overall performance to be worse off than training multiple models individually. 
We’ll go deeper to understand the meaning of “unified network” in the following sections.

![YoloR-architecture-concept-768x422](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/cbacb5b6-5662-4411-9401-081b41a75fe9)

Referring to the above image:

- In Figure 2(a): We have multiple separate networks for each task.
- In Figure 2(b): We have the typical architecture of a multi-task network. 
- In Figure 2(c): The proposed unified network (combining explicit and implicit knowledge) architecture that generates one unified representation for serving multiple tasks.


## What's Different in YOLO-R?
![YoloR-knowledge-combination-768x165](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/245c3eb9-b115-4de9-bafa-f431f8e571dc)

***Combining explicit and implicit knowledge in YoloR for general feature generation***

The authors of YoloR (successfully) take on a new direction unexplored by other papers in the Yolo series for model improvements.

Humans rely on their physical senses and past experiences to understand their surrounding environment. Over the years, we have built a massive database of knowledge and experience gained through regular learning (explicit knowledge) or subconsciously (implicit knowledge) stored in our brains. These two types of knowledge, explicit and implicit, in combination, effectively help us function with ease regularly.

The inspiration for YoloR comes from how humans effectively combine the information perceived from the surrounding environment with their past knowledge and experiences to process data previously unseen.

The main focus of this `README` is:

  ***“…to create a unified network that can encode and combine implicit and explicit knowledge, just like the brain can learn from normal and subconscious learning.”***
  
In YoloR object detection models, implicit knowledge encoded by the network is used for kernel-space alignment and prediction refinement. When enhanced with implicit knowledge, representations generated by networks can be used to solve the challenges faced with multi-task networks. 
For object detection, YoloR relies on predefined anchor boxes similar to Scaled-YoloV4.

## YoloR - A Unified Network
In the previous sections, new terms were used, such as “unified network” and “explicit and implicit knowledge.”  In this section, I’ll explain these points and what they mean in the context of neural networks.


### What Is Meant By Explicit & Implicit Knowledge?
From a psychological perspective, the theory behind the “different types of knowledge” is vast. However, from the paper’s point of view, the following is all you need to know:

- **Explicit Knowledge:** The knowledge that is easy to articulate, write down, and share with others is known as explicit knowledge. It can be codified and digitized in books, documents, reports, memos, etc.

- **Implicit and Tacit knowledge:** The knowledge gained from experience are more difficult to express and deeply embedded in the brain. Personal wisdom, insights, intuition, and expertise are context-specific and more difficult to extract and codify. For example, skills learned on a job that is transferable to different positions/jobs, i.e., knowledge acquired from experience. It refers to knowledge learned in a subconscious state.

  In reality, implicit knowledge differs from tacit knowledge, but in the YoloR paper, they are considered one.


## How Do Humans Understand & Learn New Things?
The YoloR paper begins by asking a loaded question, which is also the source of inspiration for the paper.

Human beings have grown/evolved through centuries by perceiving their surrounding environment and using proactive and reactive measures.  

Experience and intuition are integral parts of human knowledge. Without them, it might be difficult to “make sense of” or “react to” a new environment. We function daily at a high level with ease using information about the surrounding environment from our sensory organs and the knowledge/experience/intuition stored in our brains. 

When we encounter a new situation or problem, we rely heavily on our past experience and information perceived from the surroundings to understand, interpret and react in the best way possible. Ultimately, we can say we’ve gained more knowledge (new experience) which is now part of our database.

![YoloR-human-perception_v2-768x333](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/506e6ecd-88af-4e64-ac5c-14aa2b57fa55)

***Take the above image as an example. Humans can analyze it from different perspectives with ease. We can easily classify and answer various questions for the same data.***

This is not entirely true for neural networks. A CNN trained for classifying “what is this?” will work perfectly well (with some margin of error) on classifying the above image. Still, if we change the task to “where is she?” the model is either not helpful or needs to be retrained or trained in a multi-task setting. 

**The reason is that the features learned (on one task) and extracted from a trained CNN model are poorly adaptable to other problems. And we only use the network for feature extraction. Still, the implicit knowledge, abundant in a network, is ignored.**

The implicit knowledge of the trained model can be thought of as its ability to understand and decide where to look, what, how, and which features to extract and combine to create meaningful feature representations.

Just like humans apply our past experiences to understand and approach a new problem, in this paper, the authors try to encode the implicit knowledge of neural networks so that it can be used for multiple new tasks. 


## What’s It All To Do With Neural Networks? How Do We Relate Them?
From CNN’s point of view, features extracted from the shallow layers, which constitute edges, corners, curves, etc., are heavily related to the input. The extracted features are often called explicit knowledge, as they are derived directly from the input. In contrast, features obtained from the deeper layers are known as implicit knowledge.

The features extracted by the model become more and more complicated and harder to interpret. The information has been deeply encoded in higher dimensional space, which only the network can understand and use.

  ***“Here, we call the knowledge that directly correspond to observation as explicit knowledge. As for the knowledge that is implicit in the model and has nothing to do with observation, we call it as implicit knowledge.”***

To understand this, let’s take another example. Suppose you are given a network figure and have to find the number of parameters. 

When you first look at the network, you realize you haven’t seen that type of structure before. Yet, you recognize the essential components/layers of the network. First, you’ll extract vital information from the diagram, such as the number of nodes per layer, number of layers, connections, etc. Next, you’ll start calculating the number of parameters per node/layer because you understand the steps to perform and the methodology to use to solve the problem. 

Two things:  

  - We observed and extracted the information from the problem — this is explicit knowledge.
  - We used our past knowledge — implicit knowledge; combined it with the current observation to solve the problem.

![YoloR-Knowledge-modelling-overview-768x670](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/88e9d7df-aa4f-40fe-8558-69a0759e488c)

***As mentioned in the above example, explicit knowledge relies on the input to generate an output, while implicit knowledge doesn’t.***

Hopefully, by this point, you understand the concept of explicit and implicit knowledge, the intuition of the paper, and how it relates to neural networks. 

A question arises, “But how does it actually look?”

  - The explicit knowledge is being modeled (learned) by a neural network that takes in inputs and produces output. It can be any architecture (YoloV3, EfficientDet, etc.). 
  - Implicit knowledge – In this paper, the authors have provided three methods we can use to model implicit representations. We will look at this in the next sections, where we’ll go over all three modeling techniques (don’t worry, it’s easier than it feels like) and how to combine them with the explicit knowledge network.


## YoloR Architecture
As previously mentioned, the YoloR models are built upon the Scaled Yolov4 architecture. The authors take the Scaled YoloV4-P6 as the base model.
![YoloR-creating-YoloV4-P6-light-2-768x626](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/6664f565-acbd-4e70-a796-2831761caa9f)

***Creating YoloV4-P6-light from Scaled YoloV4-P6***

First, a light version: YoloV4-P6-light, is constructed by changing the following:
- The Stem of the network to reduce the input size and the number of parameters.
- A significant reduction to each backbone stage’s Depth and Out Channels. Accordingly, the channels in the neck of the model are also updated.

![YoloR-creating-variants-from-p6-light-768x134](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/abfc6c14-2bbf-4952-944c-aec2d72692d1)

***YoloR models are based on the YoloV4-P6-light base architecture, where updates are made sequentially to generate each variant.***

##### The YoloR models use the Yolov4-P6-light model as the base architecture. The authors have provided 4 model variants in the paper: P6, W6. E6 and D6.

- YoloR-P6 is created by simply changing the activation function from Mish to SiLU.
- YoloR-W6 uses YoloR-P6 architecture and changes the number of channels used in each backbone stage and, consequently, the neck.
- YoloR-E6 is created by scaling the channels of YoloR-W6 by 1.25 times and changing all downsampling modules from strided Convolution to CSP convolution.
- Finally, the YoloR-D6 is a collective model of all the above changes with increased depth of the backbone stages.

![YoloR-downsampling-convolution-csp-convolution-768x226](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/758a1f45-37c3-4db5-80df-8585ef091390)






