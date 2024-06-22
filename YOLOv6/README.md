# YOLOv6
## Paper Summary

Two months after the YOLOv7 release, researchers from Meituan Inc, China, released YOLOv6. Yes, you heard it right, `YOLOv7 was published before YOLOv6`. And apparently the authors took permission from the original YOLO authors. YOLOv6 was the state-of-the-art(SOTA) when it was published.

In the published YOLOv6 paper, the authors experimented with many techniques, and the resulting successful methods were integrated with the model and published as YOLOv6. YOLOv6 is an anchor-free, decoupled head architecture with a unique backbone named EfficientRep.

![Screenshot-from-2023-12-26-18-15-46-768x190](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/b3d2c897-cbdd-4919-9a5a-d0296eb0601e)

**Figure 1: YOLOv6 Model Architecture**


EfficientRep consists of a `RepVGGBlock stem`, followed by 4 blocks of ERBlock. ERBlock is made of a RepVGGBlock and a RepBlock. The last layer of ERBlock has a RepVGGBlock and RepBlock, with the exception of `simSPPF`, which is nothing by the `SPPF layer with ReLU` activation. For Neck, they used Rep-PAN, which is nothing but the good old PANet from YOLOv4 and YOLOv5, it is just that we replace the CSPBlock with RepBlock (for small models) or `CSPStackRep Block` (for large models). For Head, they used a customized version of the decoupled head from YOLOX, which they named the `hybrid-channel strategy`. In this method, they reduced one 3×3 conv layer and jointly scaled the width of the head by the width multiplier for the backbone and the neck.

![Screenshot-from-2023-12-26-18-16-11](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/03cc1550-9e66-4022-abf9-2ee226cba4d5)

**Figure 2: RepBlock  and RepVGG model architecture**

Except for these architectural changes, they used advanced Lable Assignment techniques such as simOTA, and TAL(Task alignment learning), a lot of new loss functions were introduced, and to make the model faster and more accurate, they used a lot of post-training quantization methods, such as self-distillation, Reparameterizing Optimizer, etc. 

YOLOv6 is a very important addition to the YOLO series of models, and it’s very important to understand what improvements were made to make the model more efficient. YOLOv6 – Paper Explanation is a great resource to understand YOLOv6 thoroughly; it meticulously clarifies complex concepts such as RepConv, RepVGGBlock, CSPStackRep, VFL, DFL, and Self-Distillation very carefully.

## Training YOLOv6
Although it’s named YOLOv6, it was the best-performing model when it was published. And it is very crucial to understand how to train a YOLOv6 on a custom dataset. YOLOv6 custom dataset training for Underwater Trash Detection provides an excellent guide for such custom training applications.
