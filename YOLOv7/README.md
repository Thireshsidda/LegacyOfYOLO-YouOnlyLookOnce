# YOLOv7

## Paper Summary
The YOLOv7 is the continuation after Scaled-YOLOv4. YOLOv4 and YOLOv7 are published by the same authors. The YOLOv7 model introduces novel modifications such as E-ELAN, Model Scaling, Planned re-parameterized convolution, Coarse for auxiliary, and penalty for lead loss. 

## Extended-ELAN (E-ELAN):
E-ELAN is an Extended efficient layer aggregation network, a **variant of ELAN**. ELAN is inspired by VoVNet and CSPNet. We already know about CSPNet, and VoVNet is nothing but an Object Detection Network composed of cascaded OSA modules. **OSA means One-shot Aggregation**. It is more efficient than Denses Block in DenseNet and optimized for GPU computation. The below image clears the concept of OSA,

![1_ywKhAQXscslGicP7ZgdrdA](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/846736d0-01f5-4c72-a928-fd004526841a)

**Fig 14: E-ELAN Aggregation Methods** [(Source](https://arxiv.org/abs/1904.09730)


