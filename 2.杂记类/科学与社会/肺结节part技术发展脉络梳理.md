# 选题：基于计算机视觉技术的临床影像诊断发展综述——以肺结节与眼科疾病为例
## 研究目的：
本研究旨在系统性梳理计算机视觉技术在医疗影像诊断领域的实际应用，重点探讨 AI 在肺结节诊断和眼科影像分析中的应用现状、技术原理和临床价值。
## 技术发展历程：
### 第一阶段：传统CAD时代
> **Doi, K. (2007). Computer-aided diagnosis in medical imaging: Historical review, current status and future potential. _Computerized Medical Imaging and Graphics, 31_(4–5), 198–211. [https://doi.org/10.1016/j.compmedimag.2007.02.002](https://doi.org/10.1016/j.compmedimag.2007.02.002)**
> 
> Although early attempts at computerized analysis of medical images [26–32] werre made in the 1960s, serious and systematic investigation on CAD began in the 1980s with a fundamental change in the concept for utilization of the computer output, from automated computer diagnosis to computer-aided diagnosis.
> 
> For CAD research on lung cancer, we attempted to develop a computerized scheme for detection of lung nodules on chest radiographs. At that time and still now, it is well known that the visual detection of lung nodules is a difficult task for radiologists, who may miss up to 30% of the nodules. Radiologists have missed these lesions due to the overlap of normal anatomic structures with nodules, i.e., the normal background in chest images tends to camouflage nodules [59–63]. Therefore, it was predicted that the normal background structures in chest images would become a large obstacle in the detection of nodules, even by computer. Thus, the first step in the computerized scheme for detection of lung nodules in chest images would be removal or suppression of background structures in chest radio- graphs. A method for suppressing the background structures is the difference-image technique [64–68], in which the difference between a nodule-enhanced image and a nodule-suppressed image is obtained.
> 
> Early studies on quantitative analysis of medical images by computer [26–31] were reported in the 1960s. At that time, it was generally assumed that computers could replace radiologists in detecting abnormalities, because computers and machines are better at performing certain tasks than are human beings.
> 
> ![[Pasted image 20251203194732.png]]
> ![[Pasted image 20251203194755.png]]

> **Katsuragawa, S., & Doi, K. (2007). Computer-aided diagnosis in chest radiography. _Computerized Medical Imaging and Graphics, 31_(4–5), 212–223. [https://doi.org/10.1016/j.compmedimag.2007.02.003](https://doi.org/10.1016/j.compmedimag.2007.02.003)**
> 
> The purpose of CAD is to improve the quality and pro- ductivity in radiologists’ image interpretation by improving the diagnostic accuracy and reducing the image reading time, respectively.

> **Yoshida, H., & Näppi, J. (2007). CAD in CT colonography without and with oral contrast agents: Progress and challenges. _Computerized Medical Imaging and Graphics, 31_(4–5), 267–284. [https://doi.org/10.1016/j.compmedimag.2007.02.011](https://doi.org/10.1016/j.compmedimag.2007.02.011)**
> 
> CAD for CTC is attractive because it has the potential to overcome the above problems, that is, CAD that detects polyps and masses has the potential to improve radiologists’ detection performance, in particular the detection sensitivity, and to reduce the variability of detection accuracy among readers. An improvement in the detection performance can be achieved because CAD can reduce radiologists’ perceptual errors during the detection of polyps. These perceptual errors may be caused by the presence of normal structures that mimic polyps or by variable conspicuity of polyps, depending on the display method [23–27]. The absence of visual cues that nor- mally exist with colonoscopy, such as mucosal color changes, and a large number of images for each patient, also makes image interpretation tedious and susceptible to perceptual error. Endoluminal 3D interpretation is also subject to observer error because 15% of the mucosa is not seen with a uni-directional fly-through [28]. A reduction of the variability in the detection performance can be achieved with CAD because it can provide objective and consistent results, whereas the performance of a radiologist may be influenced by his or her skill and experience. Moreover, a variety of circumstances, including distraction and fatigue, as well as time constraints in a busy clinical practice, influence the diagnostic performance of radiologists. Although radiologists may detect a certain type of polyp in a majority of cases, the same observers may miss the same type of polyp under different circumstances. Use of CAD can potentially overcome this lack of consistency by radiologists, and thus it can be useful for reducing variability among readers in identifying polyps in CTC. CAD also has the potential to reduce the interpretation time if radiologists are led to focus on a small number of regions indicated by a CAD scheme. A reduction in interpretation time is most likely to occur when CAD is used as a first reader. The usefulness of such a paradigm in CAD is still controversial and is subject to further investigation (see Section 7.4).

CAD （Computer-aided diagnosis，计算机辅助诊断）的根本目的是作为“第二意见”，减少漏诊和解读时间，**提高放射科医生的诊断准确性和一致性**，从而提升工作效率。CAD 被设计为**医生的补充**，而非替代。

医学影像的计算机量化分析始于20世纪60年代，但系统的CAD研究始于80年代。其发展源于一个明确的临床需求：解决医生肉眼阅片的固有局限。例如，在胸片中，高达**30%的肺结节可能被漏诊**，主要原因是正常的解剖结构（如肋骨、血管）与结节重叠，形成了复杂的背景干扰，将病灶伪装起来。

为了解决背景噪声问题，CAD研究的第一个关键突破是开发了**背景抑制技术（removal or suppression of background structures）**，特别是**差异成像技术（difference-image technique）**。该技术通过生成一张“结节增强”图像和一张“结节抑制”图像，并将两者相减，从而有效突出可疑结节，抑制正常结构，为计算机的自动检测铺平了道路。

- **提高敏感性**：减少因正常结构模仿息肉、图像数量庞大、显示方法不同导致的**感知错误**。
- **提高一致性**：提供客观、稳定的结果，减少因医生经验、技能、疲劳度、工作环境等主观因素导致的**诊断表现波动**。
- **提升效率**：通过引导医生关注CAD标记的少数可疑区域，有望**缩短解读时间**。

然而，即使使用计算机检测，**背景结构的干扰**仍是巨大障碍：胸片中的正常解剖背景结构（如肋骨、血管）仍然会伪装和掩盖结节，这是阻碍检测准确性提升的一个根本性技术难题。差异成像技术只是解决这一问题的初步尝试。

### 第二阶段：深度学习（CNN）时代

> **Krizhevsky, A., Sutskever, I., & Hinton, G. E. (2012). ImageNet classification with deep convolutional neural networks. _Advances in Neural Information Processing Systems, 25_. [https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)**
> 
> We trained a large, deep convolutional neural network to classify the 1.2 million high-resolution images in the ImageNet LSVRC-2010 contest into the 1000 dif- ferent classes. On the test data, we achieved top-1 and top-5 error rates of 37.5% and 17.0% which is considerably better than the previous state-of-the-art. The neural network, which has 60 million parameters and 650,000 neurons, consists of five convolutional layers, some of which are followed by max-pooling layers, and three fully-connected layers with a final 1000-way softmax. To make train- ing faster, we used non-saturating neurons and a very efficient GPU implemen- tation of the convolution operation. To reduce overfitting in the fully-connected layers we employed a recently-developed regularization method called “dropout” that proved to be very effective. We also entered a variant of this model in the ILSVRC-2012 competition and achieved a winning top-5 test error rate of 15.3%, compared to 26.2% achieved by the second-best entry.

> **ILSVRC（ImageNet Large Scale Visual Recognition Challenge）2012[https://image-net.org/challenges/LSVRC/2012/results.html#t1](https://image-net.org/challenges/LSVRC/2012/results.html#t1)**
> 
> ![[Pasted image 20251203203706.png]]

> **Setio, A. A. A., Ciompi, F., Litjens, G., Gerke, P., Jacobs, C., van Riel, S. J., ... & van Ginneken, B. (2016). Pulmonary nodule detection in CT images: False positive reduction using multi-view convolutional networks. _IEEE Transactions on Medical Imaging_, *35*(5), 1160-1169. [https://doi.org/10.1109/TMI.2016.2536809](https://doi.org/10.1109/TMI.2016.2536809)**
> 
> ![[Pasted image 20251203204734.png]]
> 
> The architecture of the proposed CAD system is schematized in Fig. 2. Two main stages are incorporated: 1) candidates de- tection and 2) false positive reduction. We applied three can- didates detectors specifically designed for solid, subsolid, and large solid nodules. The combination of these detectors is ap- plied to increase the detection sensitivity of nodules. Note that nodules have a large variations in both size and morpholog- ical characteristics. For each candidate, we extract multiple 2-D views in fixed planes. Each 2-D view is then processed by one ConvNets stream. The ConvNets features are then fused to com- pute a final score. In the next sections we describe the CAD system in details.

> **Dou, Q., Chen, H., Yu, L., Qin, J., & Heng, P.-A. (2017). Multilevel contextual 3-D CNNs for false positive reduction in pulmonary nodule detection. _IEEE Transactions on Biomedical Engineering, 64_(7), 1558–1567. [https://doi.org/10.1109/TBME.2016.2613502](https://doi.org/10.1109/TBME.2016.2613502)**
> 
> We propose a novel method employing three-dimensional (3-D) convolutional neural networks (CNNs) for false pos- itive reduction in automated pulmonary nodule detection from volumetric computed tomography (CT) scans. Com- pared with its 2-D counterparts, the 3-D CNNs can encode richer spatial information and extract more representative features via their hierarchical architecture trained with 3- D samples. More importantly, we further propose a simple yet effective strategy to encode multilevel contextual infor- mation to meet the challenges coming with the large varia- tions and hard mimics of pulmonary nodules.
> 
> ![[Pasted image 20251203205311.png]]
> 
> The proposed framework has been extensively validated in the LUNA16 challenge held in conjunction with ISBI 2016, where we achieved the highest competition performance metric (CPM) score in the false positive reduction track.

> **LUNA（Lung Nodule Analysis）2016 [https://luna16.grand-challenge.org/Results/](https://luna16.grand-challenge.org/Results/)**
> 
> ![[Pasted image 20251203205744.png]]
> 
> 3DCNN for Lung Nodule Detection And False Positive Reduction
> Deep Convolution Neural Networks for Pulmonary Nodule Detection in CT imaging
> 3D Deep Convolution Neural Network Application in Lung Nodule Detection on CT Images

> **Ardila, D., Kiraly, A. P., Bharadwaj, S., Choi, B., Reicher, J. J., Peng, L., Tse, D., Etemadi, M., Ye, W., Corrado, G., Naidich, D. P., & Shetty, S. (2019). End-to-end lung cancer screening with three-dimensional deep learning on low-dose chest computed tomography. _Nature Medicine, 25_(8), 1247–1248. [https://doi.org/10.1038/s41591-019-0447-x](https://doi.org/10.1038/s41591-019-0447-x)**
> 
> We propose a deep learning algorithm that uses a patient’s current and prior computed tomography volumes to predict the risk of lung cancer. Our model achieves a state-of-the-art performance (94.4% area under the curve) on 6,716 National Lung Cancer Screening Trial cases, and performs similarly on an independent clinical validation set of 1,139 cases. We conducted two reader studies. When prior computed tomography imaging was not available, our model outperformed all six radiologists with absolute reductions of 11% in false positives and 5% in false negatives. Where prior computed tomography imaging was available, the model performance was on-par with the same radiologists. This creates an opportunity to optimize the screening process via computer assistance and automation. While the vast majority of patients remain unscreened, we show the potential for deep learning models to increase the accuracy, consistency and adoption of lung cancer screening worldwide.

CNN（Convolutional Neural Network，卷积神经网络）的根本目的是通过学习大规模数据中的层次化特征，实现图像的自动高效分类与识别，**直接提升计算机视觉任务的准确性和泛化能力**。CNN 被设计为**通用图像特征提取器**，而非针对特定问题的定制工具。

早在1979年，福岛邦彦就提出了神经网络模型Neocognitron，该模型包含卷积层和池化层结构，被认为是卷积神经网络雏形，这被认为是第一个真正意义上的卷积神经网络结构。2012年的ILSVRC比赛（ImageNet大规模视觉识别挑战赛）上，SuperVision采用基于CNN的深度学习技术夺冠则将这一技术推向高潮。系统的CNN研究应用于肺结节检测始于2016年左右，其发展源于一个明确的技术需求：解决传统CAD系统在假阳性减少阶段**特征设计（由研究人员手动设计、选择和构建的、用于描述医学图像的关键量化指标）复杂且性能有限**的问题。例如，在CT肺结节检测中，传统方法依赖手工设计的特征来区分结节与非结节结构，其性能**极易达到瓶颈**。

为了解决特征表达不足的问题，CNN时代的关键突破是开发了**端到端的深度特征学习技术（end-to-end deep feature learning）**，特别是**多视图/三维卷积神经网络技术（multi-view/3D convolutional neural networks）**。该技术通过让网络直接从原始图像数据（2D切片或3D区块）中自动学习最具判别力的特征，从而有效区分真实结节与相似假阳性结构，为实现高敏感度、低假阳性率的自动检测系统铺平了道路。

- **提升检测性能**：利用网络强大的表征能力，显著降低传统方法中因特征局限性导致的**识别错误**。
- **实现端到端优化**：将候选检测与假阳性减少等阶段整合或优化，利用统一的学习目标，减少因多阶段流程中**信息损失与误差累积**。
- **利用三维上下文信息**：通过3D CNN架构直接处理CT体数据，更充分地利用肺结节固有的**三维空间形态与上下文信息**，克服2D方法信息不完整的局限。

然而，即使使用深度学习，**复杂背景结构与形态学模仿物**仍是巨大障碍：CT图像中存在的血管交叉点、炎性病灶、疤痕组织等结构与结节在局部形态上高度相似，这是阻碍检测特异性（即假阳性率）进一步降低的一个根本性技术难题。多视图融合与3D上下文建模只是解决这一问题的初步尝试。
### 第三阶段：GAN辅助深度学习时代
  
> **Han, C., Murao, K., Noguchi, T., & Satoh, S. (2019). Synthesizing diverse lung nodules wherever massively: 3D multi-conditional GAN-based CT image augmentation for object detection. In _2019 International Conference on 3D Vision (3DV)_ (pp. 729–737). IEEE. [https://doi.org/10.1109/3DV.2019.00085](https://doi.org/10.1109/3DV.2019.00085)**
> 
> Accurate Computer-Assisted Diagnosis, relying on large-scale annotated pathological images, can allevi- ate the risk of overlooking the diagnosis. Unfortu- nately, in medical imaging, most available datasets are small/fragmented. To tackle this, as a Data Augmentation (DA) method, 3D conditional Generative Adversarial Net- works (GANs) can synthesize desired realistic/diverse 3D images as additional training data. However, no 3D conditional GAN-based DA approach exists for general bounding box-based 3D object detection, while it can locate disease areas with physicians’ minimum annotation cost, unlike rigorous 3D segmentation. 
> 
> ![[Pasted image 20251203214857.png]]
> 
> ![[Pasted image 20251203214928.png]]
> Therefore, we propose 3D Multi-Conditional GAN (MCGAN) to generate realistic/diverse 32 × 32 × 32 nodules placed naturally on lung Computed Tomography images to boost sen- sitivity in 3D object detection. Our MCGAN adopts two discriminators for conditioning: the context discriminator learns to classify real vs synthetic nodule/surrounding pairs with noise box-centered surroundings; the nodule dis- criminator attempts to classify real vs synthetic nodules with size/attenuation conditions. The results show that 3D Convolutional Neural Network-based detection can achieve higher sensitivity under any nodule size/attenuation at fixed False Positive rates and overcome the medical data paucity with the MCGAN-generated realistic nodules—even expert physicians fail to distinguish them from the real ones in Visual Turing Test.

>**陈佛计,朱枫,吴清潇,郝颖明,王恩德 & 崔芸阁.(2021).生成对抗网络及其在图像生成中的应用研究综述.计算机学报,44(02),347-369.** 
> 
> 受博弈论中两人零和博弈思想的启发，GAN主要由生成器和鉴别器两个部分组成.生成器的目的是生成真实的样本去骗过鉴别器,而鉴别器是去区分真实的样本和生成的样本.通过对抗训练来不断的提高各自的能力，最终达到一个纳什均衡的状态因为生成对抗网络在生成图像方面的能力超过了其他的方法，所以其成为了一个热门的研究方向。


GAN（Generative Adversarial Networks，生成对抗网络）的根本目的是生成真实的样本去骗过鉴别器，最终达到一个纳什均衡的状态。GAN被设计为由生成器和鉴别器两个部分组成的对抗训练模型。

医学影像的GAN应用源于其**生成图像方面的能力超过了其他方法**。其发展源于一个明确的学术动机：受**博弈论中两人零和博弈思想的启发**。生成器的目的是生成真实的样本，而鉴别器是去区分真实的样本和生成的样本。通过该对抗训练模型不断的提高各自的能力，其成为了一个热门的研究方向。

为了解决医学影像数据不足的问题，GAN研究的关键突破是将**对抗学习思想与深度学习中的其他研究方向相互渗透**，从而诞生了很多新的研究方向和应用。在医学影像中，该技术通过生成器与判别器的对抗训练，从而有效生成**逼真的合成医学图像**，为解决医学数据稀缺问题铺平了道路。

- **提高生成能力**：利用其超过其他方法的图像生成能力，能**显著提高数据增强的效果**。
- **拓展研究方向**：对抗学习思想与深度学习的其他方向相互渗透，**诞生了很多新的研究方向和应用**。

然而，生成对抗网络在医学影像中的应用存在一个关键缺陷：**其数据增强能力与高层临床任务需求脱节**。早期研究主要关注提升图像本身的逼真度以辅助分类，但缺乏专门针对**3D目标检测**这一实用场景的定制化方案。这导致GAN无法有效生成用于训练检测模型所必需的、在位置、大小、衰减值等多条件控制下且能自然嵌入解剖背景的多样化病灶数据，从而限制了其在提升检测灵敏度方面的直接效用。


三维多条件生成对抗网络的根本目的是**合成所需的逼真且多样的3D图像作为额外的训练数据**，以**缓解因忽视诊断带来的风险**。MCGAN被设计为解决医学影像中**大多数可用数据集规模小且零散**这一核心问题的**数据增强方法**。

医学影像的MCGAN应用始于2019年左右，旨在解决明确的临床数据需求：突破高质量标注医学图像稀缺的瓶颈。例如，在肺部CT图像中，使用MCGAN生成自然嵌入的真实结节来提升3D对象检测的灵敏度。

为了解决3D对象检测中数据增强方法缺失的问题，MCGAN研究的关键突破是开发了**三维多条件生成对抗网络**。该技术通过**上下文判别器**学习区分真实与合成的"结节/周围组织对"，以及通过**结节判别器**在特定大小/衰减条件下区分真实与合成结节，从而能够生成**逼真且多样的32×32×32结节**，并将其自然地嵌入肺部CT图像中，为提升3D对象检测的灵敏度铺平了道路。

- **提升检测性能**：使用MCGAN生成的结节数据，能使基于3D卷积神经网络的检测器**在任何结节大小和衰减条件下，在固定的假阳性率下实现更高的灵敏度**。
- **克服数据稀缺**：利用生成的逼真结节有效克服医学数据匮乏问题，其逼真程度在视觉图灵测试中**甚至让专家医师也无法将其与真实结节区分**。

然而，即使使用MCGAN，**医学数据的稀缺性和多样性**仍是巨大障碍：大多数可用的医学影像数据集仍然**规模小且零散**，这是阻碍检测性能进一步提升的一个根本性数据难题。三维多条件生成对抗网络技术只是解决这一问题的初步尝试。
### 政策落地阶段
2018年4月，美国FDA批准了IDX-DR作为首个基于AI的医疗设备，用于糖尿病视网膜病变筛查，这是全球首例无需临床医生复核即可提供筛查决策的自主式AI诊断设备。
2020年7月，我国推想科技的“InferRead CT Lung”产品获FDA批准使用，这是首批专门用于肺结节分析的AI辅助软件，其聚焦CT影像中的肺结节检测、测量和分类。

### 第四阶段：自注意力机制（Transformer）时代
> **Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, Ł., & Polosukhin, I. (2017). Attention is all you need. In _Advances in neural information processing systems 30 (NIPS 2017)_ (pp. 5998–6008). Curran Associates, Inc.  [https://proceedings.neurips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf](https://proceedings.neurips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf)**
> 
> The dominant sequence transduction models are based on complex recurrent or convolutional neural networks that include an encoder and a decoder. The best performing models also connect the encoder and decoder through an attention mechanism. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely. Experiments on two machine translation tasks show these models to be superior in quality while being more parallelizable and requiring significantly less time to train. Our model achieves 28.4 BLEU on the WMT 2014 English- to-German translation task, improving over the existing best results, including ensembles, by over 2 BLEU. On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.8 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature. We show that the Transformer generalizes well to other tasks by applying it successfully to English constituency parsing both with large and limited training data.

> **Qiao, L., Zhang, S., & Wang, W. (2022). FcTC-UNet: Fine-grained combination of transformer and CNN for thoracic organs segmentation. In _2022 44th Annual International Conference of the IEEE Engineering in Medicine and Biology Society (EMBC)_ (pp. 4749–4753). IEEE. [https://doi.org/10.1109/EMBC48229.2022.9870880](https://doi.org/10.1109/EMBC48229.2022.9870880)**
> 
> ![[Pasted image 20251203222915.png]]
> 
> ![[Pasted image 20251203222944.png]]
> 
> ![[Pasted image 20251203223005.png]]
> As an alternative structure, Transformers have emerged due to the outstanding capability of capturing the global contextual information provided by Self- Attention(SA) mechanism. However, Transformers need more computational cost than CNNs for introducing the SA module.
> 
> In this paper, we propose a novel module named fine-grained combination of Transformer and CNN(FcTC). FcTC module is composed of dual-path extractor and fusing unit to effectively extract local information and model long-distance dependency.
> 
> Recently, convolutional neural networks(CNNs), represented by U- NET [2] and its variants [3-5][7][19-22] have achieved state-of-the-art(SOTA) performance. These methods adopt an encode-decoder architecture, where the encoder captures contextual information, and the decoder fuses the features to restore resolution. Although CNNs perform well, there is a problem that the features extracted by CNN focus on local pixels without acquiring the features of long-distance interactive information. Nowadays, many researchers try to solve this problem by using Transformer [8], which is designed to solve sequence- to-sequence prediction problem in Natural Language Pro- cessing (NLP). Transformer uses the self attention mechanism to capture the long-distance dependencies effectively between different sequences. Inspired by NLP, Vision Trans- former (ViT) [9] successfully applied the Transformer to image recognition, firstly demonstrating the great potential of this novel architecture in the vision domain. After that, many researchers have proposed various Transformer-based models[10-11][13-17] to improve the accuracy of the segmentation algorithm. TransUNet [10] is the first model that successfully applies Transformer to the medical image segmentation. Later, SwinUnet [11] further studied the feasibility of pure Transformer structure by using Swin Transformer [12]. In addition, there are many variants based on CNN- Transformer achieving satisfying results[13-17]. However, Transformer-based method has its own disadvantages. First, due to the self attention mechanism, these methods have high computational complexity. Second, among these meth- ods, combination of Transformer and CNN is coarse-grained without great information interaction. To tackle these problems, inspired by Swin Transformer [12] and ResNet [18], we design a module named fine- grained combination of Transfomer and CNN(FcTC), which is composed of a dual-path extractor and a feature fusing unit. In the dual-path extractor, we mainly design Various Window Global-local Self Attention(VW G/L-SA), which models the long-distance dependencies of different scales with lower computational cost. The reason is that we believe that the visual dependencies between regions nearby are equally important as those far away on the basis of our experiment.

ViT（Vision Transformer）在医学影像中的根本目的是**捕获不同序列之间的长距离依赖关系**，并**通过自注意力机制有效建模长距离交互信息**。

Transformer架构本身于**2017年**在《Attention is all you need》一文中被提出，其基于纯注意力机制的设计在自然语言处理领域**取得了优于现有最佳结果的性能，并展现出更强的可并行性与更短的训练时间**。对肺结节诊断而言，一个结节的恶性特征，可能不仅取决于它自身的形状，还取决于远处胸膜的牵拉程度。传统CNN的卷积核只能看到图像上一个**很小的局部区域**（比如3x3或7x7像素）。它要理解远处两个区域的关系，需要经过很多层卷积慢慢传递信息，这个过程很容易丢失或淡化这种远程关联。ViT将图像切成一个个小方块（图块），然后通过**自注意力机制**，让**任何一个图块都能直接与图像上所有其他图块进行“沟通”和“比较”**。这样，结节图块可以直接“注意到”胸膜图块的变化，无需中间层层传递。

然而，普通的Transformer架构的需要计算图像中**每一个图块与所有其他图块**的关联度。如果图像有N个图块，其计算量会以 **N²** 的规模增长。这会导致模型**训练非常慢**（需要更多时间）、**推理延迟高**（分析一张图要等更久），并且需要**非常昂贵的大显存GPU**才能运行，极大地增加了研究和部署的成本与难度。

2022年，FcTc-UNet（Fine-grained Combination of Transformer and CNN for Thoracic Organs Segmentation - UNet，用于胸腔器官分割的Transformer与CNN细粒度融合UNet模型，UNet指模型所基于的骨干分割网络架构）的架构被提出，目的是解决普通Transformer架构的算力问题。

![[Pasted image 20251204071253.png]]
![[Pasted image 20251204071920.png]]

FcTc-UNet架构通过设计**双路径提取器**与**特征融合单元**，并采用**可变窗口全局-局部自注意力机制**来建模不同尺度的长距离依赖，从而在**保持较低计算复杂度**的同时实现更精细的信息交互，为提升医学图像分割精度铺平了道路。

- **提升分割精度**：采用**可变窗口全局-局部自注意力**的细粒度组合架构，能够**以更低计算成本建模不同尺度的长距离依赖**，并实现**更高效的信息交互**。
- **验证架构潜力**：研究表明**纯Transformer结构**在医学图像分割中具有可行性，而**CNN-Transformer混合架构**能够取得满意的结果。

然而，即使使用Transformer混合架构，**计算复杂度与特征融合粒度**仍是巨大障碍：基于自注意力机制的方法仍具有**较高的计算复杂性**，这是阻碍模型效率与精度进一步提升的一个根本性技术难题。双路径提取器与细粒度特征融合只是解决这一问题的初步尝试。
### 第五阶段：深度融合与多元化发展时代
- **循环神经网络（RNN，Recurrent Neural Network）**
	RNN专门用于处理序列数据。其变体，如长短期记忆网络（LSTM）和门控循环单元（GRU），能有效捕捉序列中的长期依赖关系。
	**处理三维CT序列**：将CT扫描的连续切片视为一个序列，LSTM可以学习切片之间的空间上下文信息，帮助理解结节在三维空间中的完整形态。
	**多期相CT分析**：如果患者有不同时间点的多次CT扫描（例如年度筛查），LSTM可以分析结节在时间维度上的变化（生长速度、密度变化），这对于良恶性判断至关重要。CNN处理单一时点的图像，而LSTM则捕捉动态演变过程。

- **自监督学习（Self-supervised Learning）**
	这是一种“无师自通”的学习范式。模型利用数据本身生成标签进行预训练，而不需要昂贵的人工标注。
	**解决标注数据稀缺问题**：可以先利用海量未标注的胸部CT图像对模型进行预训练。例如，训练模型完成“图像补全”（随机遮盖一部分图像让模型预测）、或“旋转预测”（判断图像被旋转了多少度）等任务。
	**流程**：通过自监督学习，模型学会了理解医学图像的基本结构和特征，然后只需要相对较少的标注数据进行微调，就能在肺结节检测等下游任务上取得更好效果。这大大降低了对专家标注的依赖。

- **图神经网络（GNN，Graph Neural Network）**
	GNN用于处理图结构数据（由节点和边构成）。
	**关系建模**：可以将一个肺结节内部的多个特征点（或一个肺叶内的多个结节）建模为一个图。节点是特征，边表示它们之间的关系。GNN可以学习这种复杂的内在结构，用于更精细的分类。
    **多中心数据融合**：将不同医院或设备视为图中的节点，GNN可以帮助进行域适应，提升模型的泛化能力。

- **联邦学习（Federated Machine Learning）**
	一种分布式机器学习技术，其核心思想是“数据不动，模型动”。
	**保护患者隐私**：医院A、B、C各自持有本地患者数据，且由于隐私法规无法共享。通过联邦学习，可以在不交换任何原始数据的情况下，共同训练一个强大的AI模型。中心服务器将初始模型分发给各医院，各医院用本地数据训练后，只将模型参数的更新（而非数据本身）上传到服务器进行聚合。
    **价值**：这使得利用全球多中心数据训练更通用、更鲁棒的肺结节AI模型成为可能，同时严格遵守数据隐私法规。

- **可解释性AI技术（Explainable Artificial Intelligence）**
	这类技术旨在打开AI模型的“黑箱”，让人类理解其决策依据。
	**注意力机制**：模型可以生成“注意力图”，高亮显示它在做出判断时最关注的图像区域。医生可以直观地看到模型是否关注了正确的、具有临床意义的特征（如毛刺征、分叶征）。
    **梯度加权类激活映射**：一种可视化技术，可以生成热力图，显示输入图像中哪些区域对模型预测“肺结节为恶性”的贡献最大。