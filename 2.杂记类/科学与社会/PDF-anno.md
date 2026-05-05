> [!PDF|red] [[ALexnet.pdf#page=1&selection=38,33,41,9&color=red|ALexnet, p.1]]
> > reduce overfitting in the fully connected layers we employed a recently developed regularization method called “dropout” that proved to be very effective
> 
> 应对过拟合化的经典方法
>

> [!PDF|note] [[ALexnet.pdf#page=1&selection=51,53,54,39&color=note|ALexnet, p.1]]
> > At the time, most computer vision researchers believed that a vision system needed to be carefully hand-designed using a detailed understanding of the nature of the task
> 
> 之前的传统方法：手工设计

> [!PDF|purple] [[ALexnet.pdf#page=1&selection=71,0,71,8&color=purple|ALexnet, p.1]]
> > Figure 4
> 
>

> [!PDF|red] [[ALexnet.pdf#page=1&selection=132,33,135,5&color=red|ALexnet, p.1]]
> > To improve their performance, we can collect larger datasets, learn more powerful models, and use better techniques for preventing overfitting.
> 
> 三个改进的方法：更大的数据，更好的模型，应对过拟合化更好的技术

> [!PDF|purple] [[ALexnet.pdf#page=1&selection=158,0,160,60&color=purple|ALexnet, p.1]]
> > But objects in realistic settings exhibit considerable variability, so to learn to recognize them it is necessary to use much larger training sets.
> 
> 短数据集的局限性

> [!PDF|purple] [[ALexnet.pdf#page=2&selection=16,39,16,48&color=purple|ALexnet, p.2]]
> >  LabelMe,
> 
>

> [!PDF|purple] [[ALexnet.pdf#page=2&selection=22,11,22,20&color=purple|ALexnet, p.2]]
> >  ImageNet
> 
>

> [!PDF|purple] [[ALexnet.pdf#page=2&selection=29,16,29,54&color=purple|ALexnet, p.2]]
> > a model with a large learning capacity
> 
>

> [!PDF|red] [[ALexnet.pdf#page=2&selection=52,27,55,11&color=red|ALexnet, p.2]]
> >  current GPUs, paired with a highly optimized implementation of 2D convolution, are powerful enough to facilitate the training of interestinglylarge CNNs,
> 
>

> [!PDF|purple] [[ALexnet.pdf#page=2&selection=96,13,96,22&color=purple|ALexnet, p.2]]
> > Section 5
> 
>

> [!PDF|purple] [[ALexnet.pdf#page=2&selection=92,41,92,51&color=purple|ALexnet, p.2]]
> > Section 4.
> 
>

> [!PDF|red] [[ALexnet.pdf#page=2&selection=98,23,100,53&color=red|ALexnet, p.2]]
> > we found that removing any convolutional layer (each of which contains no more than 1% of the model’s parameters) resulted in inferior performance.
> 
>

> [!PDF|purple] [[ALexnet.pdf#page=2&selection=211,50,211,58&color=purple|ALexnet, p.2]]
> > igure 2.
> 
>

> [!PDF|red] [[ALexnet.pdf#page=2&selection=265,21,267,25&color=red|ALexnet, p.2]]
> >  Deep CNNs with ReLUs train several times faster than their equivalents with tanh units. This is demonstrated in Figure 1,
> 
>

> [!PDF|red] [[ALexnet.pdf#page=3&selection=25,4,25,30&color=red|ALexnet, p.3]]
> >  Training on multiple GPUs
> 
>

> [!PDF|red] [[ALexnet.pdf#page=3&selection=159,5,159,24&color=red|ALexnet, p.3]]
> > Overlapping pooling
> 
>

> [!PDF|red] [[ALexnet.pdf#page=3&selection=59,4,59,33&color=red|ALexnet, p.3]]
> >  Local response normalization
> 
>

> [!PDF|red] [[Resnet.pdf#page=2&selection=195,4,195,43&color=red|Resnet, p.2]]
> > addressing vanishing/exploding gradient
> 
>

> [!PDF|red] [[Resnet.pdf#page=2&selection=160,0,160,24&color=red|Resnet, p.2]]
> > Residual Representations
> 
>

> [!PDF|red] [[Resnet.pdf#page=2&selection=185,0,186,1&color=red|Resnet, p.2]]
> > Shortcut Connections.
> 
>

> [!PDF|red] [[Resnet.pdf#page=1&selection=19,10,19,37&color=red|Resnet, p.1]]
> > residual learning framework
> 
>

> [!PDF|red] [[Resnet.pdf#page=1&selection=23,0,25,40&color=red|Resnet, p.1]]
> > We explicitly reformulate the layers as learning residual functions with reference to the layer inputs, instead of learning unreferenced functions
> 
>

> [!PDF|red] [[Resnet.pdf#page=1&selection=133,0,136,1&color=red|Resnet, p.1]]
> > An obstacle to answering this question was the notorious problem of vanishing/exploding gradients [1, 9], which hamper convergence from the beginning. 
> 
>

> [!PDF|purple] [[Resnet.pdf#page=1&selection=151,0,155,1&color=purple|Resnet, p.1]]
> > Unexpectedly, such degradation is not caused by overfitting,
> 
>

> [!PDF|purple] [[Resnet.pdf#page=1&selection=161,16,161,47&color=purple|Resnet, p.1]]
> >  Fig. 1 shows a typical example
> 
>

> [!PDF|purple] [[U-net.pdf#page=1&selection=14,0,15,1&color=purple|U-net, p.1]]
> > Abstract.
> 人们普遍认为，深度网络的成功训练需要数千个带注释的训练样本。在本文中，我们提出了一种网络和训练策略，该策略依赖于大量使用数据增强来更有效地使用可用的带注释样本。该架构由捕获上下文的收缩路径和实现精确定位的对称扩展路径组成。我们证明，这种网络可以从很少的图像进行端到端的训练，并且在电子显微镜堆栈中神经元结构分割的ISBI挑战中优于先前的最佳方法（滑动窗口卷积网络）。使用在透射光显微镜图像（相差和DIC）上训练的相同网络，我们在这些类别中以很大优势赢得了2015年ISBI细胞跟踪挑战赛。此外，网络速度很快。在最近的GPU上，分割512x512图像所需的时间不到一秒钟。全面实施（基于Caffe）和经过培训的网络


>

> [!PDF|red] [[U-net.pdf#page=1&selection=46,48,49,81&color=red|U-net, p.1]]
> > However, in many visual tasks, especially in biomedical image processing, the desired output should include localization, i.e., a class label is supposed to be assigned to each pixel. Moreover, thousands of training images are usually beyond reach in biomedical tasks.
> 
> 在生物领域的局限性

> [!PDF|red] [[U-net.pdf#page=3&selection=23,64,25,6&color=red|U-net, p.3]]
> > e use excessive data augmentation by applying elastic deformations to the available training images. 
> 
>

> [!PDF|red] [[U-net.pdf#page=3&selection=11,0,15,24&color=red|U-net, p.3]]
> > One important modification in our architecture is that in the upsampling part we have also a large number of feature channels, which allow the network to propagate context information to higher resolution layers. As a consequence, the expansive path is more or less symmetric to the contracting path, and yields a u-shaped architecture.
> 
>

> [!PDF|red] [[U-net.pdf#page=3&selection=32,45,34,42&color=red|U-net, p.3]]
> > To this end, we propose the use of a weighted loss, where the separating background labels between touching cells obtain a large weight in the loss function
> 
>

> [!PDF|red] [[GAN.pdf#page=2&selection=45,43,58,2&color=red|GAN, p.2]]
> > a generative model G that captures the data distribution, and a discriminative model D that estimates the probability that a sample came from the training data rather than G. 
> 
>

> [!PDF|red] [[GAN.pdf#page=1&selection=27,19,32,56&color=red|GAN, p.1]]
> > In this work we introduce the conditional version of generative adversarial nets, which can be constructed by simply feeding the data, y, we wish to condition on to both the generator and discriminator.
> 
>

> [!PDF|purple] [[GAN.pdf#page=1&selection=45,16,46,14&color=purple|GAN, p.1]]
> > n order to sidestep the difficulty of approximating many intractable probabilistic computations.
> 
>

> [!PDF|red] [[GAN.pdf#page=1&selection=47,0,51,19&color=red|GAN, p.1]]
> > Adversarial nets have the advantages that Markov chains are never needed, only backpropagation is used to obtain gradients, no inference is required during learning, and a wide variety of factors and interactions can easily be incorporated into the model. Furthermore, as demonstrated in [8], it can produce state of the art log-likelihood estimates and realistic samples.
> 
>

> [!PDF|red] [[GAN1.pdf#page=1&selection=25,31,47,18&color=red|GAN1, p.1]]
> > estimating generative models via an adversarial process, in which we simultaneously train two models: a generative model G that captures the data distribution, and a discriminative model D that estimates the probability that a sample came from the training data rather than G. The training procedure for G is to maximize the probability of D making a mistake. 
> 
>

> [!PDF|red] [[ViT.pdf#page=1&selection=62,26,62,77&color=red|ViT, p.1]]
> >  its applications to computer vision remain limited
> 
>

> [!PDF|red] [[ViT.pdf#page=1&selection=63,8,65,25&color=red|ViT, p.1]]
> > attention is either applied in conjunction with convolutional networks, or used to replace certain components of convolutional networks while keeping their overall structure in plac
> 
>

> [!PDF|purple] [[ViT.pdf#page=1&selection=65,41,67,38&color=purple|ViT, p.1]]
> > this reliance on CNNs is not necessary and a pure transformer applied directly to sequences of image patches can perform very well on image classification tasks
> 
>

> [!PDF|purple] [[ViT.pdf#page=1&selection=71,36,73,49&color=purple|ViT, p.1]]
> > ision Transformer (ViT) attains excellent results compared to state-of-the-art convolutional networks while requiring substantially fewer computational resources to train
> 
>

> [!PDF|red] [[ViT(Attention is all you need).pdf#page=1&selection=65,57,67,8&color=red|ViT(Attention is all you need), p.1]]
> > the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely
> 
>

> [!PDF|red] [[ViT(Attention is all you need).pdf#page=1&selection=69,51,71,17&color=red|ViT(Attention is all you need), p.1]]
> > hese models to be superior in quality while being more parallelizable and requiring significantly less time to trai
> 
> 