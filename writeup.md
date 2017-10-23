# Follow Me Write-up

## Rubics
#### 1. Provide a write-up / README document
You are reading it!

#### 2. The write-up conveys the an understanding of the network architecture.
In this project, I implemented SegNet. The goal is to identify target, pedestrians and others in pixelwise manner. This is achieved by building an series of encoder blocks. An encoder block is composed separable convolution layer. The advantage of separable convolution layers is less parameters to train. The final layer of encoder is a fully-convolution layer. As for decoder block, each decoder will first upsample the input image and concatenate with a previous encoder block. Concatenation with encoder blocks help preserve spatial information.

#### 3. The write-up conveys the student's understanding of the parameters chosen for the the neural network.

- Batch size: the given training dataset has 4,132 instances. Hence I pick batch size of 20 so that each batch can be trained with 20 images.
- Number of batches per epoch: I chose this parameter to be 200 so that each epoch trains 20 x 200 = 4,000 images (*batch size x batches per epoch*). So each epoch roughly walk through the whole training dataset.
- Epochs and learning rate: as the model is closer to local minima, the slope is smaller and probably needs smaller learning rate. In first 40 epochs I used learning rate of 0.001; and in the last 25 I used learning rate of 0.0005. The last 5 epochs are executed one by one to get the best performance.

#### 4. The student has a clear understanding and is able to identify the use of various techniques and concepts in network layers indicated by the write-up.

- 1 by 1 convolution: this layer is using kernel shape (1x1xNk), where Nk is the number of kernels. This layer is useful for transforming the input layer to desired depth while keeping original depth and height. The addtional number of parameters is Dx1x1xNk.

- fully connected layer: uses three techniques to help us solve semantic segmantation problems.
    1. Replace fully connected layer with one by one convolutional layer. This reduced computational complexity and provides a starting point building more advanced architecture of decoders rather t	han single fully-connected layer.
    2. Upsample through the transposed convolutional layers. This technique help restore orignal image size so that we can do pixel-wise classification.
    3. Skip connection to allow former layers leak into decoder blocks, which help preserve spatial information.

#### 5. The student has a clear understanding of image manipulation in the context of the project indicated by the write-up.
The encoder blocks transform the original images into featrue maps. For example, in human face recognition, a feature map can be a mask of eyes or a mask of noses. Deep learning exploits modern computational power to explore nonintuitive features as opposed to man-crafted image filters.

The decoder blocks decipher the features maps and relate features to each pixel and original images.

One should not use too many skip connections, because it can lead to explosion of the size of the model.

#### 6. The student displays a solid understanding of the limitations to the neural network with the given data chosen for various follow-me scenarios which are conveyed in the write-up.
If we were to follow another object instead of a human, we need to replace the trainig mask images. So the interested object can be encoded in the blue channel, while the original human pixels are moved to the green channel.

If we want to classify more than three classes, RGB is not enough anymore. We may need **N** colors that is correspondent to **N** classes. And the mask images should be hot coded with **N** channels.

#### 7. The model is submitted in the correct format.
My model is stored in `data/weights`.

#### 8. The neural network must achieve a minimum level of accuracy for the network implemented.
My model achieves accuracy of 42.87%.

## References
|Ref. No.|Purpose|URL|
|--|--|--|
|[1]|SegNet|http://mi.eng.cam.ac.uk/projects/segnet/
|[2]|Separable Convolution Layer| https://medium.com/towards-data-science/types-of-convolutions-in-deep-learning-717013397f4d|