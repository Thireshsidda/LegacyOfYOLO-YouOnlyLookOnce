# YOLOv7

## Paper Summary
The YOLOv7 is the continuation after Scaled-YOLOv4. YOLOv4 and YOLOv7 are published by the same authors. The YOLOv7 model introduces novel modifications such as E-ELAN, Model Scaling, Planned re-parameterized convolution, Coarse for auxiliary, and penalty for lead loss. 

## Extended-ELAN (E-ELAN):
E-ELAN is an Extended efficient layer aggregation network, a **variant of ELAN**. ELAN is inspired by VoVNet and CSPNet. We already know about CSPNet, and VoVNet is nothing but an Object Detection Network composed of cascaded OSA modules. **OSA means One-shot Aggregation**. It is more efficient than Denses Block in DenseNet and optimized for GPU computation. The below image clears the concept of OSA,

![1_ywKhAQXscslGicP7ZgdrdA](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/846736d0-01f5-4c72-a928-fd004526841a)

**Fig 14: E-ELAN Aggregation Methods** [(Source)](https://arxiv.org/abs/1904.09730)

It is similar to DensesNet, but we concatenate the features after a few conv blocks here. 

## Compound Model Scaling:
The authors used model scaling to adjust parameters to generate models of different scales. By different scales, it means a change in model parameters. It helps to meet the need for different inference speeds. In EfficientDet, scaling is done in width, depth, and resolution, and Scaled-YOLOv4 scales the model in stages (No. of feature pyramid). In this model, they used **NAS (Network Architecture Search)**, which is a commonly used model scaling method, authors also showed that this method can be further improved using the compound model scaling approach.

## Model Re-parameterization:
Model re-parameterization means model weight averaging. There are two ways of doing model averaging, 
  -1.Averaging the weights of the same model trained on different folds of data. 
  -2.Averaging the model weights of different epochs.
This is a very popular ensemble technique. Pytorch has an implementation of the same named SWA(Stochastic Weight Averaging).


## Coarse for Auxiliary and Fine for Lead Loss:
As you can see, the model architecture has multiple heads predicting the same thing. The head responsible for the final output is the lead head, and the other heads assist in the model training. With the help of an assistant loss, the weights of the auxiliary heads are updated.

For a deeper comprehension of the YOLOv7 paper and its inference outcomes, YOLOv7 Paper Explanation is an invaluable resource. It offers a thorough analysis and provides critical insights, enhancing understanding of the subject matter.


## Training YOLOv7
The process of YOLOv7 fine-tuning was demonstrated using the pothole dataset with the official YOLOv7 repository. This included both fixed-resolution and multi-resolution training, accompanied by an in-depth analysis of model inference.
