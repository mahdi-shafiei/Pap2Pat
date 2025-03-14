# INTRODUCTION

With the development of multimedia technology, people's demand for Virtual Reality (VR) video grows rapidly. VR videos are usually stored as Ultra-High Definition (UHD) 360degree videos [1] which place high demands on the coding efficiency. The widely used video coding standards, such as Advanced Video Coding (AVC) [2], High Efficiency Video Coding (HEVC) [3] and Audio Video coding Standard 2 (AVS2) [4], are gradually becoming incapable of meeting the market demand. Therefore, the Joint Video Exploration Team (JVET) proposed a new generation of video coding standard named Versatile Video Coding (VVC). VVC introduces a variety of new coding techniques with outstanding compression performance, such as the new Quad Tree with nested Multi-type Tree (QTMT) [5] structure in the block partition, Cross-component Linear Model (CCLM) [6], Position Dependent Prediction Combination (PDPC) [7], Intra Sub-Partitions (ISP) [8] and so on. These new techniques introduced in VVC achieve significant improvement in coding efficiency. For example, QTMT makes the coding units (CUs) have more flexible shapes and sizes. However, the enhancement in compression performance comes at the cost of drastically increased complexity. Related research [9] shows that the VVC encoder is 31 times more complex compared to HEVC in All-Intra (AI) conditions, thus making it unsuitable for practical applications. For example, when VVC encodes 4K or even 8K videos, huge amount of encoding time will lead to the inability to meet real-time requirements, making it unfeasible to broadcast UHD videos. For content creators on multimedia platforms, fast algorithms can help them save a lot of transcoding time, thereby improving work efficiency. Therefore, it is of great significance to reduce the encoding time through fast algorithms.

To reduce encoding time, researchers have developed many fast algorithms for intra CU partition. These algorithms can be divided into two categories: statistic-based methods and learning-based methods. Statistic-based methods utilize CU information and statistical information to build statistical models for CU partition. With these models and finely adjusted thresholds, redundant Rate Distortion Optimization (RDO) processes will be skipped to decrease encoding time. These methods are relevantly easy to implement while relying heavily on handcrafted feature extraction and the rationality of thresholds. Learning-based methods treat the partitioning processes as classification issues, using pre-trained classifiers to determine the partition modes. These methods can achieve better performance by introducing machine learning (ML) and deep learning (DL); however, large-scale datasets and expensive GPUs are not easy to obtain.

In this paper, we propose a learning-based technique called QTMT-LNN to reduce the complexity of intra CU partition in VVC. First, we extract feature information and split modes of CUs to establish a dataset. Next, we propose Lightweight Neural Network (LNN) to skip unnecessary partition modes, and five LNNs are trained for five partition modes in QTMT. Finally, the QTMT-LNN algorithm is developed to drastically reduce the encoding time, while the coding efficiency loss is negligible. The main contributions of this paper are as follows:

1) We extract features that contain the texture distribution and characteristics of 360-degree videos, which can be roughly categorized as CU information, texture information and 360-degree video information. Five LNNs are used for five QTMT partition modes individually to reduce the intra CU partition complexity. 2) Our proposed LNN is lightweight, which consumes negligible computation time compared with the total encoding time while achieving high accuracy for the classification issues.

LNN can be implemented on VVC encoder with pre-trained weight models, without using external libraries.

The remainder of this paper is organized as follows. Section 2 reviews the related works on CU partition and fast algorithms to achieve complexity reduction. Section 3 presents details about the proposed method. Section 4 presents experimental results and comparisons with other fast intra partition algorithms. Finally, Section 5 concludes this paper.

# RELATED WORKS

## Overview of CU partition

During video coding, video sequences will be divided into CU blocks. In HEVC, the CU would be split by Quad tree (QT), which splits one CU block into four sub-CUs, and the partition depth of CU adds one every split. Therefore, the size of CU has a one-to-one mapping relationship with the partition depth. VVC has the same block-based hybrid video coding scheme as HEVC. However, a new method called QTMT is introduced, making the CUs have more flexible shapes and sizes. In VVC, the size of Coding Tree Units (CTUs) expands to 128 × 128. These CTUs will be split by QT into four 64 × 64 CUs, and each CU can be split by QTMT. As shown in Figure 1, multi-type tree split includes Horizontal Binary Tree (BT_H) split, Vertical Binary Tree (BT_V) split, Horizontal Ternary Tree (TT_H) split, and Vertical Ternary Tree (TT_V) split. Binary Tree (BT) will split CU into two sub-CUs of the same size, and Ternary Tree (TT) will split CU into three sub-CUs according to the ratio To determine the best partition mode, CU block needs to recursively traverse all possible partition modes (QTMT and not split) and the best partition mode is selected by Rate Distortion Optimization (RDO). The RDO formula based on the Lagrangian optimization method is as follows

where D stands for distortion, R stands for bit rate and the optimal encoding method is determined by comparing the size of J .

In the intra partition mode selecting process, the formula above changes to

where D SSE represents the sum of squared errors of the original block and the reconstructed block, Bit block represents the bitrate required to encode the current block, and the optimal partitioning structure is the one with the smallest RD cost .

## Fast algorithms for HEVC

During the past decades, numerous approaches have been proposed to decrease encoding time in intra CU prediction. HEVC, developed by the Joint Collaborative Team on Video Coding (JCT-VC), has been widely used in the past ten years. As mentioned above, approaches for intra prediction can be generally divided into two categories: statistic-based methods and learning-based methods.

For statistic-based methods, Liao and Chen [10] proposed a CTU depth range predictor to accelerate the CU partition process using neighbouring CTUs to restrict the current CTU. And Bayesian decision rules were applied to achieve early split and early termination strategies. Li et al. [11] proposed a fast CU partition algorithm using early split and early termination strategies based on spatial depth correlation and RD Cost characteristics. Zhu et al. [12] proposed a fast mode decision algorithm for HEVC based on texture partition and direction. The algorithm used texture partition of CTUs to predict the depth, while using texture direction to reduce the number of candidate modes for the RDO process. Li et al. [13] proposed a fast depth intra coding scheme to reduce computation complexity using texture feature and spatio-temporal correlation in 3D-HEVC. CU block would be classified into smooth, texture or edge block according to its texture feature, and early termination was introduced to reduce compression complexity.

For learning-based methods, Shi et al. [14] proposed a CNN for fast CTU and Prediction Unit (PU) partition prediction, using asymmetric horizontal and vertical convolution kernels to extract the texture features. Zhang et al. [15] proposed a threshold-based texture classification using CNN, with joint consideration of the CU depth, quantization parameter, and texture complexity. Fu et al. [16] proposed a dual support vector machine (SVM) based on texture features of CU and its sub-CUs to select the size of CU. These texture features included direction complexity and content complexity. Feng et al. [17] proposed a CNN to predict the depth map for a CTU, getting rid of the time-consuming RDO process.

Since the split mode of HEVC is QT split, there are two partition results for a CU: be QT split into four sub-CUs, or not be split. Therefore, the majority of methods regard CU split as a binary classification problem, using texture complexity of CU or depth of neighbouring CUs to predict the depth of the current CU. However, QTMT makes it more difficult to predict the partition mode in VVC. Therefore, more useful features and stronger classifiers are introduced in intra partition fast algorithms for VVC.

## Fast algorithms for VVC

In VVC, the newly introduced QTMT technique enhances the flexibility of CU partition, while drastically increasing the complexity. Similar to that of HEVC, statistic-based methods and learning-based methods can also be used to reduce the complexity of VVC.

For statistic-based methods, [18,19] and [20] used features that depict the texture and character of CU to skip unnecessary steps of CU partition. The results of these methods may not be so competitive nowadays, while features mentioned in the articles have been widely used in subsequent researches. Zhang et al. [21] proposed a fast intra block partition pattern pruning algorithm, using gray level co-occurrence matrix (GLCM) as features to calculate texture direction information. Liu et al. [22] used cross-block difference measured by the gradient and content of sub-blocks to skip unnecessary partition modes. Saldanha et al. [23] proposed partitioning decision classifiers using light gradient boosting machines to skip QT, BT and TT, which performs well on intra CU partition. Besides, the time saving and coding efficiency of the method can be adjusted by setting different parameters.

For learning-based methods, Tech et al. [24] used a CNN to estimate two parameters which restrict the width and height of coding blocks in partitioning, skipping inefficient partitioning modes to achieve time saving. Park and Kang [25] used a lightweight neural network to skip TT split. Li et al. [26] proposed a deep CNN model trained on a large-scale database to determine the CU partition, however, establishing large-scale databases and training might be time-consuming and expensive.

In addition, in the field of fast algorithms for video coding, [18] - [24] the commonly used time complexity evaluation index is the proportion of time saved by the fast algorithm compared to the original method.

Compared with HEVC, split modes increase from QT to QTMT, making it difficult to predict partition mode. The statistic-based methods are gradually incapable of dealing with complex QTMT split prediction. And the learning-based methods leverage the powerful classification capability of ML/DL models to perform CU partition prediction.

## Fast algorithms for 360-degree video

As mentioned before, VR videos are usually stored as 360degree videos, which is different from the traditional 2D videos.

To encode 360-degree videos with current video coding standard developed for 2D videos, the projection from 3D sphere to 2D plane is required. 360Lib [27] introduced several projection formats including Equi-rectangular Projection (ERP), Cubemap Projection (CMP) and so on. Besides, Spherical PSNR (S-PSNR) [28] and Weighted to Spherically uniform PSNR (WS-PSNR) [29] were introduced to evaluate the quality of 360-degree videos. Common methods used in HEVC and VVC perform well on traditional 2D videos, but they are not effective enough for 360-degree videos. Therefore, it is vital to propose new methods that consider the difference between 360-degree and 2D plane videos. Nasrabadi et al. [30] pointed out that ERP mapping will severely stretch the frames at the north and south poles of sphere, which reduced coding efficiency and increased bandwidth consumption. Besides, human visual attention was mainly focused on the equatorial region in the center of the videos, which could save resources by reducing the encoding quality in the polar regions. Storch et al. [31] found that the horizontal mode will be chosen more frequently in the intra prediction than other angular modes in the polar regions of the ERP format frames, therefore they proposed a method to reduce the number of intra angular modes according to the position of CU. Filipe et al. [32] introduced features using the latitude of CU, which took stretch information of 360-degree videos into account. Zhang et al. [33] proposed a fast intra algorithm using texture characteristics to early terminate the CU partition. And reduced the candidate modes list of RMD process according to the direction of texture. Mao et al. [34] introduced a fast intra prediction algorithm using the similarity of CU and its sub-CUs. Besides, spherical weights were introduced to optimize mean squared error (MSE) considering the weight characteristics of ERP projection. Since most of these methods are deployed in HEVC, it is necessary to develop a new fast intra algorithm for VVC. Therefore, this paper proposes a learning-based intra prediction fast algorithm for 360-degree video based on VVC.

# PROPOSED ALGORITHM

## Overview of QTMT-LNN

The proposed fast intra CU partition framework is shown in Figure 2. In the left part of the image, those white modules are the original flowchart in VVC, while those blue skip modules are the proposed fast algorithm in this paper. And in the right part of the image, modules in the blue dotted frame are the detailed description of the blue skip module on the left. Before the split modules of a certain partition mode (QT, BT_H, BT_V, TT_H and TT_V), the encoder will use the skip modules to decide whether skip the current partition mode or not. Inside the skip module, 9 features of the current CU are extracted and sent into LNN, and then the output of LNN will decide whether skip the current partition mode or not by comparing it with the pre-set threshold. Since 5 skip modules operate independently, 5 LNNs are trained for 5 partition modes individually as well. Briefly summarized, the proposed fast algorithm introduces skip modules before the partition modules in VVC encoder, and the skip modules can reduce encoding time by skipping unnecessary partition modules.

## Features extraction

Several features extracted in the proposed method are shown in Table 1, which can be generally divided into three categories: texture information, CU block information and 360-degree video information. 

### Texture information

Texture information plays a significant role in CU partition. For example, those CUs with larger texture complexity are more likely to be split into smaller sub-CUs, and CUs with vertical texture tend to be split in the vertical direction. Variance is introduced to describe the texture complexity of a CU as follows: where w and h are the width and height of CU, i and j are the position of pixel P (i, j ), and P mean is the mean luminance of pixels in CU. However, the variance of CU could only depict the global texture complexity, while in some cases (as shown in Figure 3) CU with the same variance may vary a lot in texture distribution, leading to completely different partition modes. Therefore, it is significant to introduce features to depict the local texture complexity.

To solve this issue, we choose the mean of absolute difference between pixels (MADP) to measure the difference between one pixel and its eight surrounding pixels, it is defined as follows:

where x and y are the position of eight pixels surrounding pixel P (i, j ). After that, neighbouring mean squared error (NMSE) is introduced to depict the local texture complexity of a CU block as follows:

Besides texture complexity, the directions of CU blocks also matter. In the field of image processing, the Sobel operator is widely used in edge detection. Compared with other operators, it has the advantages of simple structure, strong anti-noise ability and high processing efficiency. Therefore, Sobel operators are introduced to calculate the texture distribution in different directions as

where P 3×3 (i, j ) is the 3 × 3 matrix of pixel located at (i, j ) and its eight surrounding pixels, * denotes convolution.

To avoid situations where the Sobel operator matrix is out of the boundary of CU block, Sobel operator matrix moves on (w -2) × (h -2) blocks, rather than w × h.

# 3.2.2

360-Degree video information 360-degree videos in ERP format have a characteristic that area in the polar regime will be horizontally distorted compared to the equatorial regime. With numerous statistical analyses over encoding 360-degree videos, it is found that CUs in the polar regime are more likely to be large and flat blocks with horizontal textures. This characteristic can be helpful to skip some less possible partition modes. latitude is introduced to depict the position of CU blocks as

where H is the height of the frame, y is the vertical position of the current CU and h is the height of the current CU.

The biggest advantage of ERP projection is its intuitive projection method, and the completely linear transformation formula makes it easy to operate. However, the extremely low projection complexity reduces the sampling uniformity. The same number of sampling points on each latitude leads to a situation where the pixel sampling density in the polar regime is greater than that in the equator regime. K latitude is introduced to describe this situation as follows:

## CU information

The size of CU(width, height) and the depths of partition tree (depth, QT depth, MT depth) are also significant. Besides, the aspect ratio of the CU block also affects the tendency to split in different directions. Therefore, Block-Shape-Ration (BSR) [25] is introduced to quantitatively measure the shape of rectangular blocks as

where w means the width of CU and h means the height of CU. QP is also a vital feature, when QP is large, the encoder tends 

## Features selection

To ensure the lightweight of the proposed neural network, it is necessary to reduce the number of features. Pearson Correlation Coefficient (PCC) is introduced to evaluate the correlation between features and class (class is a bool, meaning whether to split current CU or not). PCC has been widely used in the fields of deep learning and data analysis to evaluate correlation.

Compared to other tools such as cosine similarity and squared Euclidean distance, PCC introduces data normalization and vector centring methods, which make it have better performance in practical applications. The PCC is calculated from the dataset we established, and the construction of the dataset is detailed in Section 3.4. The principle for selecting features is that the correlation between features and class should be strong while the correlation among features should be weak, and features of each category should be included. Take TT_V as an example, from the heatmap shown in Figure 4, it is clear that among features to depict CU information, Width, Height, QT_depth, MT_depth, and BSR exhibited strong correlations with class, while QP and Depth exhibited weak correlation with class. Besides, BSR is calculated using Width and Height, but the correlations with them are low (-0.014 and -0.052, respectively), which proves that feature BSR is effective.

Among features to depict 360-degree video information, the correlation of feature Latitude with class is a little higher than that of K latitude . However, the correlation of Latitude with other features are higher. Besides, K latitude is calculated using Latitude, containing the information of Latitude to some extent. Therefore, K latitude is selected to depict 360-degree video information.

For texture information features, NMSE is chosen to depict the complexity of texture rather than Var because the correlation between NMSE and class is stronger. Besides, the previous part of the paper points out the downside of Var. Since BT and TT divisions are directional, Texture H and Texture V are necessary to depict the orientation of texture.

Finally, 9 features (Width, Height, QT_depth, MT_depth, BSR, K latitude , NMSE, Texture H and Texture V ) are chosen among all features. The PCC between features and class for five split modes are shown in Table 2, these 9 features perform well on all five split modes.  

## LNN

### LNN architecture

The proposed LNN model has 9 input nodes, 30 hidden nodes and 1 output node. The model is shown in Figure 5. 9 features are extracted during the encoding process and sent to LNN as 9 input nodes, and the output value determines whether skip the current partition mode or not. The calculation between input layer, hidden layer and output layer is defined as:

where x i denotes the value of i th input, w i j denotes the weight form i th current node to j th next layer node, b j denotes bias, 𝜑 is the number of nodes per layer. To strengthen the nonlinear fitting ability of the LNN, Sigmoid function is introduced after y i as:

### Datasets preparation and LNN training

We establish 5 datasets for 5 LNNs, and the data are extracted when encoding different videos with different QPs (22,27,32,37) in AI configuration, using the VVC reference software VTM-14.0 [35] with 360Lib-13.1 [27]. The setting of coding parameters conforms to common test condition (CTC) [36] recommended by JVET experts. To avoid overfitting, these videos contain sequences recommended in CTC and other non-CTC 360-degree sequences [37]. Datasets are built using partition information when encoding AerialCity, Driving-InCity, Natatorium, Highway, Canolafield and THEPLACE. To ensure the diversity, these sequences are chosen carefully, which contain natural scene, artificial scene, distant scene, close Besides, to achieve a balance between the positive samples and the negative samples, these samples need to be randomly added or deleted in some datasets. For example, the proportions of different partition modes in all five partition modes are shown in Figure 6. One can notice that the TT_H occupies a very low proportion, thus making TT_H dataset unbalanced. Therefore, the positive samples are repeatedly written into the TT_H dataset, while the negative samples would be randomly deleted.

Finally, five LNN models were trained individually for five split modes, setting the learning rate as 0.01, using Adam as optimizer. The data set is randomly divided into training set and testing set according to the ratio of 4:1. Since the proposed neural network is lightweight, CPU is sufficient for training. Then, LNNs and pre-trained models are converted to C++ and implemented into the VTM-14.0 software. Therefore, the proposed method can work on VTM, needless of external libraries.

# EXPERIMENTAL RESULTS

In this section, we conduct experiments to evaluate the effectiveness of the proposed method in decreasing the encoding time of VVC. Besides, subjective quality assessment and partition results visualization are used to display the results more intuitively. Finally, the complexity analysis and ablation study are conducted.

## Evaluation protocol and configuration

Our experiment is implemented on the VVC reference software VTM-14.0 with 360Lib-13.1. 360-degree video sequences recommended in CTC were encoded at All-Intra configuration, with four QPs (22,27,32,37). The Bjontegaard delta bitrate (BDBR) and Bjontegaard delta PSNR (BDPSNR) [38] are measured to assess the RD performance. To evaluate the encoding performance of 360-degree sequences, PSNR is replaced with WS-PSNR when calculating BDPSNR. Besides, Time Saving Ration (TSR) denotes the encoding time reduction of the proposed methods compared to the original methods as:

where T Original denotes the encoding time of original method using VTM-14.0 encoder and T Proposed denotes the encoding time of our proposed method. All experiments are conducted on a server with the Ubuntu 16.04.7 LTS operating system, Intel Xeon E5-2678 v3 @2.5GHz, and 128 GB RAM.

## Performance of the proposed algorithm

Since five LNN models can work individually, the performance of modules with different combinations are evaluated in this section. The proportions of different partition modes in all five partition modes when encoding sequence AerialCity are calculated, and the result is shown in Figure 6.

It can be seen from Figure 6 that among all QPs (22,27,32,37), BT occupies the majority, and TT occupies a small part, while QT occupies a very low proportion. Besides, the proportions of TT_H and BT_H are slightly higher than those of TT_V and BT_V, respectively, which verify [30] and [31] that horizontal mode will be chosen more frequently in the intra prediction when encoding 360-degree ERP videos due to the horizontal distortion.

Since the proportion of QT is extremely low, running the QT skip module alone only has a little effect on the encoding time saving as shown in Table 3. Since QTMT is a newly introduced technology in VVC, in order to ensure the integrity of the partition fast algorithm, we have designed five skip modules for all five partition modes. Hence, we still keep the QT skip module and compare the performance of different combinations of modules (QTTT, QTBT, QTMT) as shown in Table 3.

From Table 3, it can be seen that the proposed methods averagely achieve 52.28% encoding time reduction with 0.71% coding loss in BDBR using QTTT, and 50.35% encoding time reduction with 0.98% coding loss in BDBR using QTBT. If all five LNN modules are used, the proposed QTMT method can achieve 72.17% encoding time reduction with 1.78% coding loss in BDBR. Besides, encoding time reduction is a little higher on 8K sequences, and the reason might be as follows: higher-resolution sequences contain more CUs, and content of each CU tends to exhibit pixel-level textures, which makes it easier for LNNs to skip unnecessary partition modes.

The results show that the proposed method achieves huge encoding time savings with only a small increase in BDBR. Besides, our method can attain different encoding performances using different combinations of LNN modules, making it more flexible in practical applications.

## Comparison with other fast algorithms

The comparison between the proposed QTMT-LNN with other intra partition fast algorithms are conducted in this  section. Since there are few intra CU partition methods for 360-degree videos on VVC, and their encoding experiments did not use 360-degree sequences recommended in CTC either, we reproduce the algorithms in [25] and [23], and implement them on VTM-14.0 with 360Lib. Table 4 shows that our proposed method QTTT averagely reduces 52.28% of encoding time on the 360-degree videos, more effective than the time reduction of 40.49% in [25], while their BDBR are similar. Average results may not show large differences, but when observing the performance sequence-by-sequence, it is obvious that the proposed method performs better and more stable on most sequences. In particular, time reduction of QTTT for AerialCity sequence achieve 38.41%, while only 19.50% in [25]. Besides, QTTT achieves 39.33% and 41.01% time reduction for Balboa sequence and Broadway sequence, respectively, almost three times of 13.17% and 14.91% in [25]. The reason may be that features used in [25] do not have good generalizability. Hence, it is for sure that the proposed QTTT outperforms the method in [25].

And our QTMT method achieves 72.17% encoding time saving on average with only 1.78% loss in BDBR, which performs much better than 58.42% time saving and 3.05% BDBR increase in [23]. Although more features are used in [23], our method use features that consider the characteristic of 360-degree videos, and the fitting ability of LNN is stronger than the transitional decision tree. That is why our QTMT-LNN outperforms the method in [23]. Therefore, the effectiveness of the proposed method has been verified.

## Visualized result

In order to visually demonstrate the effectiveness of the proposed method, the partition results are visualized in Figure 7.

One frame from AerialCity is used for illustration since the BDBR of AerialCity is similar to the average result. The regions with red ellipses are examples of areas where the partition modes of the proposed method and original method (VVC) are different, and the regions with yellow ellipses are examples of areas where the partition results of the proposed method and original method (VVC) are similar. It is obvious that the partition results of the proposed method are consistent with the original method in most regions, which means that the proposed method has little coding performance degradation.

To evaluate the subjective quality of the proposed method, two frames are selected, respectively, from AerialCity and DrivingInCity encoded by the proposed method to compare with the results of the VVC encoder. Since the whole frame is too large, only some parts of the frames are shown in Figure 8. Among them, AerialCity is a distant scene of a city containing large-area, flat, irregular-shaped natural objects and regularly shaped man-made objects. And DrivingInCity is a close scene of cars on streets which contains man-made objects, English letters, numbers and detailed car logos. Structural similarity (SSIM) [39] is introduced to quantitatively describe the similarity of images. It is difficult to notice a subjective difference between pictures and the SSIM of them are similar. Therefore, we can conclude that the subjective quality between the original method and the proposed method has little difference, and the proposed method has little impact on subjective quality. It is obvious that the proposed method can skip unnecessary partitions while the subjective performance is similar to VVC, which explains why the proposed method reduces the encoding time significantly with negligible coding loss in terms of BDPSNR and BDBR.

## Complexity analysis

The complexity reduction comes from skipping unnecessary partition modes, therefore it is important to analyse the proportion of skipped partition modes among all partition modes respective. This indicator is measured as follows

where N mode denotes the number of a certain partition mode and Skip mode denotes the number of partition mode that can be skipped among N mode . Table 5 shows that quite a few proportions of split modes judged by LNNs as unnecessary partition modes are skipped, which reduce compression complexity. Besides, as the QP increase, the Ratio Skip of all five split modes increase as well. The reason may be that in the case of large QP, the encoder prefers not to split CU into smaller sub CUs.

It can be seen that the value of Ratio Skip is lower than the ratio of encoding time saving, which is explained as follows. The process of trying to traverse all possible partitions should structurally behave as a multi-forked tree, and the proposed method reduces the size of the tree by pruning, thereby achieving time saving. In a tree structure, different nodes are not of equal status, their importance is related to their position in the tree. In this case, different nodes will also have different weights for encoding time. For example, nodes close to the root node correspond to large-sized CUs, and skipping the unnecessary split modes of these CUs can save more time, since the sub-trees of these nodes are much larger than the sub-trees of nodes close to the leaf nodes. Ratio Skip only counts the number of split modes, which does not include the weights between these splits, while the encoding time reductions are implicitly affected by such weights. Therefore, skipping ratios and encoding time reductions do not correspond strictly numerically.

QTMT-LNN is introduced to skipping unnecessary partition modes which needs extra time. To analyse the time complexity of QTMT-LNN, the ratio of LNN computation time to total encoding time is introduced as

where T LNN denotes the computing time of LNN and T Proposed denotes the encoding time of the proposed method. Table 6 demonstrates that the proposed LNN consumes negligible computation time while achieving great classification performance.

# TABLE 5

The ratio of skipping partition modes  

# Ratio

## Ablation study

In this section, ablation studies are conducted to investigate the effectiveness of the proposed LNN and the features. The detailed results are presented below.

### LNN structure

To compare the fitting ability of LNNs with different hidden layers structure, we change the number of hidden layer nodes to 20, 25, 50 and 70 (use 9×n×1 to name their models, respectively, where n is the number of hidden layer nodes), and construct deeper LNNs by adding hidden layers (9×30 × 30×1 LNN has two hidden layers with 30 nodes, 9×30 × 50 × 30×1 LNN has three hidden layers). Besides, decision tree and random forest are also introduced for comparison. The accuracies of these models are shown in Table 8. It is noticeable that LNN performs much better than decision tree and random If the size of hidden layer reduces to 25, the accuracy will slightly decline, which still can be seen as a good result. However, when the size of hidden layer reduces to 20, there is a relevantly large decline in accuracy. Therefore, the minimum size of hidden layer should between 20 and 25. To ensure the robustness of the LNN, using 30 as size of hidden layer is reasonable.

In order to show the training process more intuitively, the graphs of accuracy and loss shown in Figure 9 are drawn by taking QT as an example. The chart shows that as the epoch increases, the accuracy increases and the loss drops substantially for a short period of time, and stabilize for a long period of time thereafter, meaning that LNNs only take a short time to converge while demonstrating a strong fitting ability for this issue. It is obvious that increasing hidden layers and nodes did not improve the accuracy noticeably, using LNN with 9×30×1 structure can simplify network structure while sustaining high accuracy.

### Features

In the previous section of this article, Pearson Correlation Coefficient is introduced to evaluate the correlation between features and class, selecting nine features which can be roughly divided into three classes: CU information (width, height, QT_depth, MT_depth, BSR), 360-degree video information (latitude) and texture information (NMSE, Texture H , Texture V ). In order to verify the influence of these features on the LNN classification ability, we feed these features into n × 30 × 1 LNN with different arrangements, where n represents the input number of features. The accuracy and loss of LNN trained with different features is shown in Table 7 and Figure 10, where CU means using CU information as five inputs to train LNN, and CU + texture means using CU information and texture information as eight inputs to train LNN, CU + 360 means using CU information and 360-degree video information as six inputs to train LNN, All means using all nine features as input to train LNN. From the upward trend of the accuracy curve and the downward trend of the loss curve, it is clear that LNNs can converge quickly under different training strategies. However, they vary a lot in accuracy. From Table 7, it can be seen that the accuracy of LNNs using CU information features is much higher than that using texture features, since CU is the basic unit of video coding and CU information has a great influence on whether split current CU or not.

When comparing each number one by one, it is apparent that the accuracy of LNN for QT using CU information and 360-degree video information features is 0.9208, which is lower than the accuracy (0.9340) using CU information and texture information features. However, in the other split modes (BT_H, BT_V, TT_H, TT_V), the introduction of 360-degree video information features has a greater improvement in accuracy.

Since the blocks divided by QT are usually flat, it is reasonable to speculate that among flat blocks, the texture complexity has a great influence on whether to split or not. And in the other split modes (BT_H, BT_V, TT_H, TT_V), since CU partition is recursive, texture direction is related to the shape of CU. In this situation, the improvement in accuracy brought by texture information features is not that obvious. Considering that horizontal mode will be chosen more frequently than other angular modes in the polar regions of the 360-degree video, 360-degree video feature (K latitude ) performs well when dealing with BT_H, BT_V, TT_H, TT_V split.

In general, 360-degree video information and texture information introduced in this paper enhance the accuracy of LNN.

# CONCLUSION

In this paper, we propose a fast intra CU partition algorithm using LNN to reduce the coding complexity by skipping unnecessary QTMT partitions. We introduce features that consider the characteristic of 360-degree videos, and ablation studies are conducted to verify the effectiveness of proposed features as well as LNN. The experimental result shows that the proposed method can reduce the encoding time from 52.28% to 72.17% on average, with coding efficiency losses ranging from 0.71% to 1.78% compared to the VVC reference software VTM-14.0, which outperforms other fast intra partition algorithms. Considering that the encoding method for traditional 2D video does not perform well on 360-degree video, we have improved the traditional method to make it suitable for 360-degree video coding. In the future, we will modify the features according to the characteristics of 2D video, and extend the usage scenarios of this fast algorithm to 2D videos.

# ACKNOWLEDGEMENTS

This work was supported in part by the National Natural Science Foundation of China under Grant 61871437; in part by the Shenzhen Research Project under Grants JSGG20210802152811033, 2022ZTE03-05; in part by the Natural Science Foundation of Hubei Province of China under Grant 2019CFA022. Z. Sun, L. Yu and W. Peng are with the College of Electronic Information and Communications, Huazhong University of Science and Technology, Wuhan, China. The authors would also like to thank the editors and anonymous reviewers for their insightful comments, which have greatly improved the quality of this paper.

# Funding information

National Natural Science Foundation of China, Grant/Award Number: 61871437; Shenzhen Research Project, Grant/Award Numbers: 2022ZTE03-05, JSGG20210802152811033; Natural Science Foundation of Hubei Province, Grant/Award Number: 2019CFA022

# AUTHOR CONTRIBUTIONS

Zhewen Sun: Data curation, formal analysis, investigation, methodology, project administration, software, validation, visualization, writing -original draft, writing -review and editing. Li Yu: Conceptualization, funding acquisition, methodology, project administration, resources, supervision. Wei Peng: Conceptualization, methodology, project administration, supervision, writing -review and editing.

# CONFLICT OF INTEREST

The authors have declared no conflict of interest.

