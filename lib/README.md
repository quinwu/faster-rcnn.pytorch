- Anchor Generation Layer

  This layer generates a fixed number of "anchors"(bounding boxes) by first generating 9 anchors of different scales and aspect radios and then replicating these anchors by translating them across uniformly spaced grid points spanning the input image.

- Proposal Layer

  Transform the anchors according to the bounding box regression coefficients to generate transformed anchors. Then prune the number of anchors by applying non-maximum suppression using the probability of an anchor being a foreground region.

- Anchor Target Layer

  The goal of the anchor target layer is to produce a set of "good" anchors and the corresponding foreground/background labels and target  regression coefficients to train the Region Proposal Network. The output of this layer is only used to train the RPN network and is not used by the classification layer.

- RPN Loss

  The RPN Loss function is the metric that is minimized during  optimization to train the RPN network. The loss function is a combination of:

  - The proportion of bounding boxes produced by RPN that are correctly classified as foreground/background. 
  - Some distance measure between the predicted and target regression coefficients.

- Proposal Target layer

  The goal of the proposal target layer is to prune the list of anchors produced by the proposal layer and produce _class specific_ bounding box regression targets that can be used to train the classification layer to produce good class labels and regression targets.

  The goal of the proposal target layer is to select promising ROIs from the list of ROIs output by the proposal layer.

​	



- ROI Pooling Layer

  Implement a spatial transformation network that samples the input feature map given the bounding box coordinates of the region proposals produced by the proposal target layer. These coordinates will generally not lie on integer boundaries, thus interpolation based sampling is required.

- Classification Layer

  The classification layer takes the output feature maps produced by the ROI Pooling Layer and passes them through a series of convolutional layers. The output is fed through two fully connected layers. The first layer produces the class probability distribution for each region proposal and the second layer produces a set of specific bounding box regressors.

- Classification Loss

  Similar to RPN loss, classification loss is the metric that is minimized during optimization to train the classification network. During back propagation, the error gradients flow to the RPN network as well, so training the classification layer modifies the weights of RPN network as well. The classification loss is a combination of:

  - The proportion of bounding boxed produced by PRN that are correctly classified ( as the correct object class )
  - Some distance measure between the predicted and target regression coefficients.