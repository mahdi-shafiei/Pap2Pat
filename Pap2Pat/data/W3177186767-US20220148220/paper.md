# Introduction

Estimating the relative pose between two cameras is a fundamental problem in computer vision, which forms the backbone of structure from motion (SFM) methods. Geometric approaches based on the 5-point method [37] and bundle adjustment (BA) [59] are well-studied, while recent methods based on deep neural networks (DNNs) also achieve promising results [61,7,56,65]. But the question of how the two families of methods may be combined to trade-off their relative benefits has as yet remained under-explored, which is the subject of our study in this paper.

The behavior of geometric methods [18] is theoretically  characterizable under a wide range of camera motions. But such understanding does not always guarantee good performance. While high accuracy is obtained in situations with strong perspective effects, performance may degrade due to lack of correspondences, planar degeneracy and basrelief ambiguity [9], to name a few. On the other hand, learning-based methods may avoid the above issues by learning sophisticated priors that relate images to camera motion, but can suffer from poor generalization outside the training domain and not be amenable to interpretation.

This paper proposes an uncertainty based probabilistic framework to fuse geometric and DNN predictions, as illustrated in Fig. 1, with the aim of overcoming the limitations of either approach. The underlying intuition is that the geometric solution may be trusted more due to its well-understood rationale if it is highly confident, but the network should play a role in driving the solution closer to the true one in geometrically ill-conditioned scenarios. We obtain geometric uncertainty using the Jacobian of the error functions, serving as an indicator of the quality of the solution, while we design a network to additionally predict the uncertainty associated with camera pose estimation. The uncertainty so obtained may be interpreted as (co)variance of a Gaussian distribu-tion, which allows us to fuse the two predictions using Bayes' rule. We highlight that the geometric solution and the fusion step are both tightly integrated into our end-to-end trainable pipeline, hence enforcing strong interaction between DNNs and the geometric method during training.

Our relative pose learning framework is the first of its kind in terms of forcing the network to give an account of the classical geometric solution along with its uncertainty in a principled way, during training. The network can also be thought of as a means to learn to improve the geometric solution such that the final fused one is closer to the ground truth. Further, the geometric guidance also distinguishes our uncertainty learning from previous works (e.g. [27]) that learn standard aleatoric uncertainty [26]. More importantly, our learned uncertainty may be considered as geometrically calibrated, in the sense that its numerical range can readily match to that of the geometric uncertainty and permits a direct fusion of the two during training.

In terms of network architecture, inspired by SuperGlue [46], we find a self-attention [62] graph neural network (GNN) to be effective at learning from keypoint correspondences. This is probably since self-attention permits strong interactions between correspondences, which is an essential procedure to determine the relative pose. We also illustrate that even both translation direction and rotation lie on a manifold, fusion is still feasible by careful choice of parameterization. We term our uncertainty-aware fusion framework as UA-Fusion. UA-Fusion is extensively validated by achieving state-of-the-art performance, especially in challenging indoor datasets with unconstrained motions.

In summary, our contributions include: • A principled fusion framework to leverage the best of both classical geometric solvers and DNNs for relative pose estimation. 

# Related Works

Geometric methods Due to its essential role in SFM [48,75,77], a wide variety of algorithms have been proposed for the relative pose estimation problem. These could be classified into algebra-based minimal/nonminimal solvers [34,37,19,32,29] and optimization-based nonlinear methods [28,67,5,12]. In addition, some methods are specially tailored to specific types of camera setup or motion prior [71,21,76,16,53,15,14]. Our framework does not require the geometric methods to be differentiable, and in principle could work with any approaches.

Learning methods Deep learning has recently been applied to different problems in SFM, e.g. [45,47,58,55,78].

In particular, both supervised [61, 7, 56, 65] and unsupervised [73,64,35,3,72] methods for relative pose estimation are proposed. Despite the promising performance, such methods are largely developed in isolation of geometric methods. Although many works do borrow ideas from geometric methods to guide the network designing, (e.g. the spirit of bundle adjustment in BA-Net [56], LS-Net [7] and DeepSfM [65]), the geometric solution itself is not explicitly leveraged in network training as we do. Note that while we validate our concepts by learning the camera pose alone, our idea is amenable to learning with depth as well.

Geometric uncertainty In addition to the well-behaved camera pose estimation, geometric methods also offer a natural way to measure the uncertainty [11] of the predictions without requiring access to the ground truth. This is achieved by relating the Jacobian of the error landscape to the covariance of Gaussian distributions. The uncertainty so obtained has been extensively studied in photometric computer vision [11]. It has also found applicability in a wide variety of tasks, such as RGB-D SLAM [10], radial distortion model selection [42] in SFM, skeletal images selection for efficient SFM [51], height map fusion [79], 3D reconstruction [17], and camera calibration [39]. Polic et al. recently make efforts [41,40] towards efficient uncertainty computation in large-scale 3D reconstruction. Despite the prevalence of the geometric uncertainty, it is however not yet fully exploited when it comes to the context of deep learning. Our work makes contributions towards bridging this gap.

Learning uncertainty Uncertainty learning, as a means towards more interpretable deep learning models, has emerged as an important topic recently [26]. Quantifying uncertainty of network predictions is highly desirable in many applications, such as camera relocalization [25], depth uncertainty [4] and photometric uncertainty [68] in SLAM, and optical flow estimation [22]. In the context of SFM, Klodt and Vedaldi [27] utilize uncertainty learning to handle the varying reliability of geometric SFM when applied as reference ground truth to supervise the SFM learning. Our paper is close to but distinct from this work since our goal lies in the fusion between the geometric and learned SFM with both the geometric and learned uncertainty. Laidlow et al. [30,31] put forth to fuse the depth maps obtained from learning and geometric methods, sharing similar spirit to our pose fusion framework. Yet, their uncertainty is learned independent from the geometric solution, and requires a non-trivial postprocessing fusion step. In contrast, our UA-Fusion learns uncertainty by tightly integrating geometric guidance and fusion into network training.

# Method

Tradeoffs between geometric and learned pose We start by noting intuition from geometric pose estimation, which may guide regimes where interesting trade-offs may be ob-served for our uncertainty-based fusion.

• Critical keypoint configurations: Geometric solvers suffer when correspondences are scarce, for example, in textureless regions. Some common keypoint configurations may lead to ambiguous solutions or ill-posed objectives, for example, when keypoints lie on a plane [18]. We expect that our geometric uncertainty will lend greater importance to DNN priors in such situations. • Critical motions: Bas-relief ambiguity [9,54] may arise when the camera undergoes sideways motion due to the resemblance between translational and rotational flow under limited field of view, leading to a less accurate pose estimation. Forward motion also poses challenges to geometric SFM, partially due to small feature movement near the focus of expansion and partially due to severe local minima in the least squares error landscape [63,38]. • Rotation and translation: Translation estimates are known to be more sensitive than rotation, leading to issues such as forward motion bias in linear methods if proper normalization is not carried out [37,57]. Thus, one may expect rotation to be more reliable in the geometric solution, while the DNN may play a more significant role for translation. We will show that our uncertainty-based framework handles this in a principled way.

## Background: Geometric Uncertainty

Geometric solution Formally, we are interested in solving the relative camera pose between two cameras C 1 and C 2 with known intrinsics. We assume C 1 as the reference with pose denoted as P 1 = [I 0] and wish to estimate the relative camera pose of C 2 , denoted as

where R ∈ SO(3) and t ∈ S 2 denote the relative rotation and translation direction, respectively. Suppose both cameras are viewing a set of common 3D points X i , i = 1, 2, ..., n, each yielding a pair of 2D correspondences x 1 i and x 2 i in the image plane. It is well-known that a minimal set of 5-point correspondences suffices to determine the solution, with Nister's 5-point algorithm [37] being the standard minimal solver. A RANSAC procedure is usually applied to obtain an initial solution, followed by triangulation [18] to obtain 3D points X i . Finally, one could refine the solution by nonlinearly minimizing the re-projection error using bundle adjustment [59],

where π() denotes the standard perspective projection and θ = {θ R , θ t , X i , i = 1, 2, ...n}. θ R and θ t represent the parameterizations of rotation and translation; we will come back to their specific choice in the next section.

# Geometric uncertainty

In order to describe the uncertainty associated with the optimum θ in a probabilistic manner, the distribution of θ could be approximated locally by a Gaussian distribution N (θ| θ, Σ). As a first-order approximation, the information matrix Λ, i.e. Σ -1 , is computed as:

where J( θ) denotes the Jacobian of the nonlinear least squares (Eq. 1) at θ. We remark that J( θ) is of full rank in this paper, implying the absence of gauge ambiguity [11,24]. This is attributed to the fixed camera pose of C 1 as well as our minimal parameterizations of (R, t) to be discussed shortly. Also, we shall conduct fusion on each individual parameter in {θ R , θ t } separately due to the discontinuity [74] in representation, and we will directly work on inverse variance for convenience. The inverse variance 1/σ 2 i of a parameter θ i in {θ R , θ t } may be obtained by Schur complement:

where J includes the index to the remaining parameters in θ. This step is also called S-transformation [2] that specifies the gauge of covariance matrix [11,42]. From the probabilistic point of view, it is in essence the inverse variance of a conditional Gaussian on θ i given all the other parameters [23].

As the expert reader may have noticed, we omit the keypoint localization uncertainty [11,39] of x 1,2 i , for simplicity.

## Geometric-DNN Relative Pose Fusion

Notations Whenever ambiguity arises, we use subscript g, d, and f to distinguish the prediction from the geometric prediction (g), the DNN prediction prior to fusion (d), and the fused solution (f).

# Bayes fusion

We conceptually treat the geometric and DNN predictions as measurements from two different sensors and fuse them using Bayes' rule, akin to a Kalman filter. Specifically, given ( θi,g , 1/σ 2 i,g ) and ( θi,d , 1/σ 2 i,d ) as the geometric and DNN prediction, respectively, the posterior distribution of the motion parameter θ i is [36] 

(5) The maximum-a-posterior (MAP) estimation θi,f is returned as the final fused solution, which receives supervision.

## Neural Architecture for Probabilistic Fusion

Architecture Overview As illustrated in Fig. 2, our UA-Fusion framework takes as input an image pair along with the correspondences extracted by a feature matcher, for which we use SuperGlue [46] due to its excellent performance. The two images are stacked and passed to a ResNet [20] architecture to extract the appearance feature. The corresponding keypoint locations are first embeded into a higherdimensional space by a Multilayer Perceptron (MLP) and 

# Pose Branch

Uncertainty Branch then feed into an attentional graph neural network to extract the geometric feature. Afterwards, the appearance and geometric features are concatenated before being passed to the pose and the uncertainty branches, each using an MLP to predict respectively the mean and inverse variance of the underlying Gaussian distribution of the motion parameters. These are then fused with the geometric solution (5pt&BA) based on uncertainty. Note that the loss is imposed on the final fused output, which induces gradient flows back-propagated through the fusion, hence coupling the geometric and learning module in training.

Intuition for the Architecture While the ResNet offers global appearance context, the graph neural network and geometric feature encode strong geometric cues from any available correspondences to reason about camera motion. Further, correspondences as the sole input to the 5-point solver and bundle adjustment have a more explicit correlation with the uncertainty of geometric solution, allowing the network to decide the extent to which the geometric solution should be trusted in relation to the DNN prediction.

Self-Attention Graph Neural Network As the network input, we stack all the correspondences (x 1 i , x 2 i ) between the two views as x 12 ∈ R n×4 , which is subsequently passed to an MLP for embedding, yielding f (0) ∈ R n×d , with d = 128 being the feature dimension. Next, f (0) is passed to four sequential message passing layers to propagate information between all pairs of correspondences, with structure similar to SuperGlue [46]. The message passing is best represented as a graph, with each node containing a pair of correspondence and edges connecting different pairs of correspondences. Specifically, in the l-th layer, the feature vector f l i associated with the correspondence pair i is updated as:

where [ . , . ] indicates concatenation and m l i denotes the message aggregated from all the correspondences based on the self-attention mechanism [62]. As per the standard procedure, we define the query (Q l ), key (K l ) and value (V l ) as linear projections of f l , each with their own learnable parameters shared across all the correspondences. The message m l is then computed as

The softmax is performed row-wise and m l i is row i of m l . The output of the last layer f 4 i is passed to an MLP and average pooling to be concatenated with the ResNet feature.

# Why self-attention?

First of all, the self-attention encourages interactions between all correspondences, which mimics the knowledge from classical geometry that all pairs of correspondences (n >= 5) together contribute to the relative pose determination, necessitating the interactions between points. More importantly, the strong representation capability of self-attention facilitates the learning of complex relationships among different pairs of correspondences. A straightforward example is the spatial relation. It is known in classical SFM [18] that two pairs of correspondences far from each other and widely spread in the image plane provide stronger signals for motion estimation; it effectively makes full use of the perspective effect in the field of view and prevents the degradation of perspective to affine camera model [18,50]. Conversely, two pairs of correspondences near to each other typically contribute weaker extra cues compared to either pair alone. Hence, different pairs of correspondences are not on equal footing and should be treated differently. Indeed, we empirically observe a strong correlation between the attention and the spatial distance. In a nutshell, the self-attention enforces extensive interactions and permits learning more complex and abstract relationships among different pairs of correspondences, which we empirically observe to play a significant role in terms of performance. More analyses will follow in the experiments.

## Motion parameterization

It is crucial to choose the proper motion parameterizaton for the above network, which we discuss now.

Translation We consider the properties of two distinct parameterizations for training and fusion:

t(α, β) = (cos α, sin α cos β, sin α sin β),

where α ∈ [0, π] and β ∈ [-π, π] are constraints for uniqueness. Since the unit-norm vector t itself is not a convenient quantity for fusion as it lies on the S 2 manifold, we seek to fuse the parameters (t x , t y , t z ) or (α, β). As the scale of (t x , t y , t z ) is indeterminate, causing gauge ambiguity and a rank-deficient Jacobian, we opt for (α, β) as the fusion entity, that is, θ t = {α, β}. However, due to its circular nature, the wrap-around of β at ±π leads to discontinuity in the representation, which leads to training difficulties if the network predicts β directly. To address this issue, we design the network to output (t x , t y , t z ) followed by unit-normalization, then we extract (α, β) and proceed to fusion. Circular Fusion While the fusion of α remains straightforward, the circular nature slightly complicates the fusion of β. Ideally, a meaningful fusion is obtained only when |β d -β g | < π, which could be achieved by letting βg = β g + 2kπ with k ∈ {-1, 0, 1}. This is illustrated by the toy example in Fig. 3, where depending upon the specific values of β g and β d , a direct fusion of the two might yield a solution far from both when |β d -β g | > π. This is, however, addressed by fusing β d and βg instead. We term this procedure as circular fusion and βg as β g 's circular nearest neighbor to β d . Rotation We consider two minimal 3-parameter representation of rotation-angle-axis representation and Euler angles. The network is designed to regress the angle directly. Although this also faces discontinuity at ±π [74], it barely poses a problem since rotations between two views are often far from ±π (such strong rotation will quickly diminish the overlapping field of view). We observe similar performance from the two representations, but opt for Euler angels since its fusion of yaw-pitch-roll angles, denoted as θ R = {φ y , φ p , φ r }, has a clearer geometric meaning. Loss One could impose the loss directly on the fused angles {θ R,f , θ t,f } with a circular loss [33], or convert them back to (R(θ R,f ), t(θ t,f )) and impose loss on it. All the combinations perform similarly in our experiments; we choose the following loss for marginally better performance,

where * indicates the ground truth and θ * R is θ * R 's circular nearest neighbour to θ R,f . w = 1.0 is a weight. Implementation details We make use of OpenCV for the 5-point solver with RANSAC, and Ceres [1] for BA and Jacobian computation. More discussions on the framework and network training are in the supplementary.

# Experiments

## Dataset

DeMoN Dataset [61]. This dataset contains a large amount of data including indoor, outdoor, and synthetic data, extracted from SUN3D [66], RGB-D [52], MVS [60,13,49,48], and ShapeNet [6]. The diversity of both scenes and motions makes the dataset challenging for SFM. ScanNet [8]. To evaluate the generalization ability, we also test our network on the ScanNet data. We use the testing data extracted by [56], that consists of 2000 pairs of images with accurate ground truth pose. The sequences are captured indoors by hand-held cameras, making it particularly hard.

## Results on DeMoN

We first compare UA-Fusion with the state-of-the-art relative pose learning methods including DeMoN [61], LS-Net [7], BA-Net [56] and DeepSfM [65], in Tab. 1. The error of translation (resp. rotation) is measured as the angle between the prediction and the ground truth. First, although SuperGlue followed by 5pt&BA produces competitive results compared to methods that learn pose directly, it can be seen that our fusion of the geometric and DNN prediction further boosts the performance, achieving the overall best results. We also test UA-Fusion with SIFT as the feature matcher, under which case ours again improves over the pure geometric method, further validating its merits. In addition, we test two more methods that focus on learning correspondences, LGC-Net [69] and NM-Net [70], by passing their output to 5pt&BA. We observe that the obtained accuracies lag behind our fusion method.   

## Analysis: Geometric vs. DNN

Since the DeMoN test set contains only 354 pairs of images, we train a model with a few training sequences (∼10k pairs) left for testing, in order for statistically meaningful analysis. We denote the translation and rotation error of the geometric solution ("5pt&BA"), the plain DNN predition prior to fusion ("DNN"), and the final fused solution ("DNN-Fusion") as (E t,g , E R,g ), (E t,d , E R,d ) and(E t,f , E R,f ).

# Geometric error vs. DNN error

We first study the accuracy against the uncertainty of the geometrical solution. We sort the test data by the total inverse variance in translation 1/σ 2 t = 1/σ 2 α + 1/σ 2 β , and plot the sorted E t,g , E t,d and E t,f in the top row of Fig. 4. The curves are smoothed by averaging over every 300 consecutive image pairs. First observe the overall trend that E t,g decreases while 1/σ 2 t increases, revealing the correlation between geoemtrical error and uncertainty. In addition, the accuracy gap between E t,g and E t,f is increasingly large with decreasing 1/σ 2 t . This indicates a more significant role of DNNs in geometrically uncertain scenarios; conversely, the geometrical solution is mostly retained if it is highly confident. Further observe the superiority of "DNN-Fusion" over "DNN", which is expected as only "DNN-Fusion" receives direct supervision. Another potential reason is that "DNN" likely has to contain bias to compensate the error/bias in "5pt&BA" during fusion. The same curve is plotted for the rotation error in the bottom row of Fig. 4. We observe that the geometrical solution transfers to the final output in most cases, probably due to the higher stability of the rotation estimation compared to the translation; this viewpoint has been conveyed by Nistér [37]. Geometric uncertainty vs. DNN uncertainty Next, we visualize the uncertainty by plotting (1/σ 2 α , 1/σ 2 β ) sorted by E t,g . As shown in Fig. 5

) for cases with larger E t,g , and vice verse. This implies the desired capability of "DNN" to dominate the final solution when "5pt&BA" tends to fail. Similarly, we present the same plot for rotation in Fig. 5(c)-(e). Comparing the range of 1/σ 2 R,g and 1/σ 2 t,g , one observes the higher confidence/stabability in the geometric rotation  estimate than the translation. This also makes it dominate in most cases when fused with the DNN prediction, as evidenced by the curves. We also note that the smoothed curves reveal the overall trend but may overly obscure the DNN's impact on the rotation, thus we provide in the supplementary more analyses in this regard.

Homography degeneracy Motion estimation may be unstable with correspondences that could be related by a homography. Characterizations of the algorithm's stability in this aspect is essential. To this end, we fit a homography to correspondences in each image pair; the ratio of inliers, H ratio , is used to indicate closeness to degeneracy. We then sort image pairs by H ratio and plot the error curves in Fig. 6(a). As can be seen, "DNN-Fusion" mostly keeps the geometric solution in well-conditioned cases with lower H ratio , while alleviates the degradation under higher H ratio . Forward vs. Sideway motion We first select those image pairs with nearly pure translational motion, and compute the angle between translation direction and z-axis, denoted as φ f orward . φ f orward = 0 • and 90 • indicate pure forward and sideway motion, respectively. We plot the errors sorted by φ f orward in Fig. 6(b). While forward motion does not cause serious issues in our tested data, one observes increasing errors while approaching pure sideway motion, which is liable to the bas-relif ambiguity. Yet, the performance degradation is alleviated in our fused solution.

Correspondence Finally, we plot in Fig. 6(c) the errors against the number of inlier correspondences found by "5pt&BA". Clearly, DNNs contribute more when the geometric accuracy drops due to lack of correspondences. Qualitative Results We present here three qualitative examples. Fig. 8(a) represents a geometrically challenging case since all the correspondences nearly lie in a frontal plane with small depth variations, leading to weak perspective effect and potential degeneracy. Similarly, Fig. 8(b) shows an ill-conditioned instance with sparse correspondences concentrated on a small region. Fig. 8(c) demonstrates a case with nearly lateral motion along the vertical direction, which is liable to bas-relief ambiguity. As can be seen, the geometric method may deteriorate significantly when confronted with such challenges, especially the translation, whereas our fusion network returns more reasonable solutions.

## Self-attention

A comprehensive understanding on what is learned by the attention module is challenging. However, as discussed in Sec. 3.2.1, we could study its potential relation to the spatial distance between any two pairs of correspondences. To this end, for a pair of correspondences, we collect its attentions to all the other pairs of correspondences in the last self-attention layer, and then compute its spatial distance 1 to other correspondences at different percentiles of the attention set. This way we can compare spatial distance against varying attention. Averaging over all the points in the entire testing set, we obtained the curve shown in Fig. 7. First, in line with the expectation, the overall trend indicates strong correlation between attention and spatial distance. In addition, it also reveals the overall smaller spatial distance with increasingly higher attentions. Referring to Sec. 3.2.1, we reckon that this is due to the increasing difficulty of extracting additional pose-related information from two points closer to each other; such difficulty enforces stronger interactions between those points in order to make contributions to the final camera pose estimation. More discussions are in the supplementary due to lack of space.

## Baselines & Ablation Study

We provide ablation studies to elucidate the impact of important factors by comparison with several baselines. Aleatoric Uncertainty: We replace our geometry-guided uncertainty with the plain aleatoric uncertainty [26,27,30], 1 Denoting a pair of correspondences as (x 1 i , x 2 i ) and (x 1 j , x 2 j ), the spatial distance is defined as( (  Table 3. Ablation study of our methods under different configurations. We also compute the averaged error over all the four scenes, as shown in the "All" column. The lowest error in each column is bolded. PointNet: We replace the self-attention GNN with Point-Net [43], which has a similar number of parameters.

As shown in Tab. 3, all the baseline methods lead to a drop in the accuracy. This substantiates the effectiveness of our geometry-guided uncertainty, self-attention mechanism, motion prameterization, and network design. In particular, removing uncertainty and fusion causes the most significant degradation. We present more analysis on the aleatoric uncertainty in the supplementary due to lack of space.

## Results on ScanNet

To demonstrate the generalization capability of our network trained on the DeMoN datasets, we also conduct crossvalidation experiments on the ScanNet dataset. Since the indoor model of SuperGlue is trained on ScanNet as well and its training set may have intersection with the testing set here, we instead apply its outdoor model to obtain correspondences. As a reference, we also report the results with the indoor model. As can been in Tab. 2, UA-Fusion achieves the best accuracy compared to prior arts. More importantly, we observe similar behavior in accuracy and uncertainty as analyzed in Sec. 4.3; this is detailed in the supplementary.

Computational performance: Our network inference takes about 0.01s per image pair, on an RTX2080 GPU. Computing geometric uncertainty takes around 0.025s, where the block sparsity of the Jacobian is leveraged for efficiency.

# Conclusion

In this paper, we propose a new relative pose estimation framework capable of reaping benefits from both the classical geometric methods and deep learning approaches. At the crux of our method is learning the uncertainty explicitly governed by the geometric solution, which permits fusion in a probabilistic manner during training. Despite being specific to the relative pose problem, we envision that our idea of learning with geometric uncertainty guidance followed by fusion in a deep network is broadly applicable.

