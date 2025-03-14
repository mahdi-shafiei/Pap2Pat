# Sensor Field

Ordinal data d(x 1 , y 1 ) > d(x 1 , y 3 ) d(x 1 , y 1 ) < d(x 1 , y 6 ) . . . y 1 y y y y y y y 1 1 1 1 1 1 1

x x x x x x 1 1 1 1 1 1 1 x x x x x x x x x x x x 1 1 1 1 1 1 y 3 y y y y y y y y Sensor field (shaded in light gray) that contains anchors (sensors at known locations, denoted by blue and white pins) and target nodes (sensors at unknown locations, marked as green hexagonal prisms). Only comparative pairwise distances are known, giving rise to ordinal data. Ordinal UNLOC is used to estimate the location of target nodes based on ordinal data. The method contains three main steps: rank aggregation (Section IV-A), function learning (Section IV-B), and unfolding localization using approximate anchor-to-target distances (Section IV-C).

• We show that using only one-bit data (ordinal, pairwise comparisons), we can accurately solve the localization problem. • We show through numerical simulations, that our method outperforms traditional localization approaches in rich scattering environments.

• Through hardware experiments, we show that Ordinal UNLOC outperforms traditional localization.

• We report that Ordinal UNLOC is only slightly worse off than fingerprinting-based methods, but with significant lower overhead.

The rest of this paper is organized as follows. In Section III, the problem is described. The Ordinal UNLOC algorithm introduced in this paper to solve the localization problem is presented in Section IV. Simulation and hardware experiments are discussed in Section V. Concluding remarks and future work are presented in Section VI.

# II. RELATED WORK

Predominant research on indoor localization focuses on addressing the lack of infrastructure, by utilizing wireless sensor networks (WSNs). A given area is covered by placing a set of low-cost wireless sensors (called anchors). The anchors, together with the target node that is to be localized, form a sensor network via wireless communications (See Figure 1). Assuming that the distance between the anchors and the target can be reasonably estimated, such approximate anchor-to-target distances together with the known location of the anchors are used to estimate the location of the target. Under this framework, many methods have been developed for target localization based on noisy distance measures, including multilateration and triangulation [14]- [20], and linear and nonlinear optimization [21]- [24]. However, as noted earlier, these methods are not suitable for practical indoor localization since the distances measured between the sensors, typically inferred from proxies such as time and power signals, are generally unreliable in indoor environments [6], [8]- [10], [12], [13], [25]- [32].

In recent years, fingerprinting methods have become increasingly common for localization with superior accuracy [9], [29], [33]- [35] where signal distributions are mapped to specific regions in space. Though there are performance advantages to using fingerprinting, it requires extensive mapping of the environment, a priori. This presents a challenge in dynamic environments that change frequently. It is also necessary that the training database be sufficiently large to provide robustness to the algorithm. Several methods to mitigate these issues have been developed including neural networks [26], histogram interpolation to decrease the amount of sampling required [34], and sensor fusion [8], [9], [13]. Methods to use the database for localization include Bayesian estimators [9], and deep learning [13], [26], [36].

In contrast to the work described above, we propose an algorithm that does not require detailed distance measurements, or rely on pretrained, precollected, and expensive methods such as with fingerprinting.

# III. SYSTEM MODEL

A standard setup of a sensor field for indoor localization is shown in Figure 1, where N sensors are classified depending on whether their locations are known or unknown: m sensors at known locations are called anchors and the n sensors at unknown locations are targets (note that N = n + m). We use y i = [y 1i , . . . , y qi ] ∈ R q and x j = [x 1j , . . . , x qj ] ∈ R q to represent the coordinates of the i-th anchor (i = 1, . . . , m) and those of the j-th target (j = 1, . . . , n) in a q-dimensional space, respectively. Typically q = 2 or q = 3 but our analysis is valid for general q. We classify the distances between sensor pairs into three types: anchor-to-anchor, anchor-to-target, and target-to-target, which we represent using three matrices:

We also define an aggregated distance matrix

where D XY is the transpose of D Y X . When we refer to the sensors without specifying the type, the first m sensors are anchors and the rest (n) are targets, so that sensor i for 1 ≤ i ≤ m refers to the i-th anchor, whereas sensor m + j refers to the j-th target node (j = 1, . . . , n).

Since the anchor locations are known, the matrix D Y can be directly obtained. However, for many practical settings such as in indoor environments, the rest of the block matrices D X , D Y X , and D XY are generally unavailable [17]- [19], [37]. Here, we take a different approach. Instead of using unreliably measured distances, we consider comparisons of distances (or their proximity measures) which give rise to ordinal comparison data, which we describe as follows. For each sensor triplet (i, j, k), we compare the distance measurements or proxies from sensor i and from sensor j to the reference sensor k, respectively, to determine which sensor (between i and j) is closer to k. We denote the binary outcome of such a pairwise comparison as z (k) ij , which generally depends on some unknown function f of the actual distances d ik and d jk , represented as

where ξ

(k) ij denotes noise in such a comparison. Due to skew symmetry constraints (i being closer to k than j is the same as j being farther away from k than i, and vice versa), we assume that ξ

ji (∀i, j, k). For the majority of the paper, we focus on ordinal comparisons, defined by a "thresholding" function

where sgn(•) is the signum function [38]. Note that this is a special case of the logistic function f (d, d , ξ) = 1/ 1 + e -β(d-d +ξ) in the limit of the parameter β → ∞. Furthermore, in practice one might only have measurements of physical quantities ("signals") that serve as proximity of distance, in which case

where s is a (monotonic) function of distance, such as power loss or time of flight of signals between sensors.

The collection of all pairwise comparisons form N square-shaped matrices: {Z (k) }, where

), which we refer to as pairwise comparison matrices, can be combined into single tensor Z (see Figure 2 for an example).

## IV. ORDINAL UNLOC

Our localization algorithm, which we term Ordinal Unfolding-based Localization (Ordinal UNLOC), consists of three main steps (see Fig. 1, right panel). First, from the ordinal comparison data, we apply rank aggregation to obtain a set of "dissimilarities", which, for a given reference sensor, k, assigns a score to each sensor that serves as a proxy to its distance to the k-th sensor according to the ordinal data and rankings. Such sets of spatial proximities (scores) are not unique, since any shifting and monotonic scaling preserves the ranking of the scores. Consequently, the scores obtained in this step cannot be directly used as approximate anchor-to-target distances for localization.

Next, the estimated distance proximities among the anchors, together with the known anchor-to-anchor distances, are used to fit functions that best transform proximities into distances, and such functions are then applied to obtain an estimation of anchor-to-target distances.

Finally, given the locations of anchors and estimated anchor-to-target distances, we formulate a multidimensional unfolding optimization problem, the solution of which provides an estimate of the location of the target.

In what follows, we describe each of these steps. 

### A. Rank aggregation from ordinal data

The first step in Ordinal UNLOC is to use a rank aggregation method to infer spatial proximities from the given ordinal data. For each reference sensor k, we seek a set of scores, denoted by ψ (k) = [ψ 1k , . . . , ψ N k ] , that serve as proximities and ideally preserves the ordinal data as constraints, such that ψ ik > ψ jk if and only if d ik > d jk (or in other words, z

Note that this is equivalent to requiring the ranking of the entries in each ψ (k) be identical (or as close as possible) to the ranking of the entries of the k-th column of D, which we denote as

The problem of inferring the ranking or scoring of a set of items from their pairwise comparisons is commonly known as "rank aggregation" [39], [40], interpreting each pairwise comparison as assigning a local ranking between two items, with the goal of obtaining an aggregated global ranking that preserves these local rankings as much as possible. Several methods are available for solving a rank aggregation problem, most of which compute a score for each item based on the collection of ordinal data [41].

Here, we adopt a common, easy-to-implement, and effective method, referred to as HodgeRank [42] or simply least squares (LS) ranking. The idea is to formulate a linear least squares problem using the ordinal data as inputs, the solution of which gives the estimated spatial proximities and approximately preserves distance orderings from the comparison data. Consider an arbitrary enumeration of the set of all ordered pairs of sensors, where the -pair is denoted by (i , j ), defining a set

where M = N (N -1)/2. Thus, E can be interpreted as the edge set of a complete graph of N nodes, and {(i , j )} is an ordered list of all the edges. From this, we define the corresponding incidence matrix, B = [b ,q ] M ×N [43], where

For each sensor k, the ordinal data in matrix Z (k) can be effectively represented by a column vector

following the same enumeration of pairs as in the incidence matrix. Then, the LS ranking method solves a linear least squares problem to yield

Such a solution can be computed in several ways, including for example, using normal equations or singular value decomposition [44], to produce

where (•) † denotes the pseudoinverse of a matrix [44], the matrix B B is also known as the graph Laplacian and is generally noninvertible, and the constant c is chosen such that the condition 1 ψ (k) = 0 is satisfied. Finally, we collect the vectors ψ (k) into a matrix of spatial proximities Ψ = [ψ (1) , . . . ,

We partition this matrix in the same way as the aggregated distance matrix D, so that

where Ψ XY is the transpose of Ψ Y X .

### B. Function Learning: estimating distances from spatial proximities

The second step of Ordinal UNLOC is to estimate the unknown anchor-to-target distances from the matrix of spatial proximities, Ψ, obtained by rank aggregation in (11), together with the known anchor-to-anchor distances. In particular, for each sensor k that is an anchor (so 1 ≤ k ≤ m), we learn a function g k that maps the estimated proximities with respect to anchor i into known distances d ik , so that

, distances between pairs of anchors), the function g k needs to be inferred from the k-th column of the anchor-to-anchor distance matrix D Y and that of the dissimilarity matrix Ψ Y , which we denote as d Y k and ψ Y k , respectively. We express g k using a basis expansion. Using the standard polynomial basis, we have the representation

To preserve the ordering among the dissimilarities, we additionally require that g k be a monotonic function. Under this additional constraint, we here consider a truncated series to the first order so that g k becomes just a linear function:

where the coefficients c

are to be determined from the vectors d Y k and ψ Y k . This can be done via solving a linear regression problem, using standard least squares, for example, producing

for k = 1, . . . , m (every anchor).

From these coefficients, we compute an estimate of the distance from the k-th anchor to the targets, using the formula

Repeating the procedure for all the anchors, k = 1, . . . , m, produces a preliminary estimate of the anchor-to-target distance matrix, DXY (or equivalently, DY X , by transposing DXY ). Before moving on to the next step of localization, it is important to recalibrate the estimated anchor-to-target distances, for the following reason. Consider the j-th target node (j ∈ {1, . . . , n}), whose distances to the anchors are preliminarily estimated to form the j-th column of DY X , denoted by dY X j = [ dY X 1j , . . . , dY X mj ]. For each anchor k ∈ {1, . . . , m}, the entry dY X kj is obtained from its corresponding function g k , which generally differs from one anchor to another. The error in fitting g k can cause the ordering of dY X kj 's to differ from the ordering of the true distances d Y X kj , and even different from the estimated proximities ψ Y X kj . Therefore, for each target node j, to best ensure that the estimated anchor-to-target distances preserve the ordering of estimated proximities, we re-fit a linear function g m+j in the same form as (13), but now with the goal of constructing a monotonic function that transforms the anchor-to-target proximities to the preliminary estimate of anchor-to-target distances, in place of the unknown distances, giving: for j = 1, . . . , n, where

. Finally, we use g m+j to recalibrate the estimated anchor-to-target distances, as

for j = 1, . . . , n, thus forming the estimated anchor-to-target distance matrix DY X and its transpose DXY .

### C. Unfolding localization from distance measures

For each target node j (i.e., sensor m + j, where 1 ≤ j ≤ n), the estimated anchor-to-target distances DY X together with the locations of the anchors {y i } can be used to infer the location of the target x j . We achieve this by formulating an unfolding optimization, with cost function [45] 

where the matrix Y = [y 1 , . . . , y m ], δ = [δ 1 , . . . , δ m ] , and the solution is termed UNLOC [45]. Applying UNLOC to each column of the estimated distance matrix DY X leads to the estimated location of all the targets. That is, we compute, for each j = 1, . . . , n, xj = argmin

This can be solved in several ways, including those discussed in [21], a global optimization routine [46], or generalized to include weights [45].

### D. Computational Complexity

In this section, we discuss the computational complexity of Ordinal UNLOC with reference to traditional localization methods. In terms of computational complexity, the final step of Ordinal UNLOC is a traditional multilateral algorithm such as the one discussed in [45]. Ordinal UNLOC is, therefore, more complex than traditional ToA/RSS-based systems. However, while the traditional methods require accurate distance or time estimates, our approach does not. The cost in terms of computational complexity is therefore mitigated by the fact that we can obtain accurate localization estimates even in rich scattering environments.

# V. EXPERIMENTS AND RESULTS

To validate Ordinal UNLOC and verify its effectiveness, we conduct a variety of numerical simulations. In each such simulation, we place m anchors as well as a target uniformly at random in [0, 1] 2 and obtain ordinal comparison data from the threshold model ( 2)-(3) using Gaussian noise N (0, σ 2 ). Specifically, for m = 5, we show an example of the Z tensor in Fig. 2, for one realization of noise. 

## A. Benchmark tests: effects of the number of anchors and noise

As the result of the Monte-Carlo simulations, as shown in Fig. 3, we found that the root-mean-squared-error (RMSE) of localization decreases rapidly as the number of anchors increases. Because ordinal comparisons inevitably introduce information loss about the exact target location, a non-diminishing localization error is expected, and is indeed observed in our simulations. Both the quick improvement with increasing anchors and error saturation is found across a range of noise levels, suggesting robustness of Ordinal UNLOC.

To further understand the non-perfectness of localization with ordinal constraints, we compare the RMSE of localization with the rank correlation between D and D, by plotting both versus increasing noise levels. The results, shown in Fig. 4, validate our hypothesis that increasing the number of anchors improves performance, and increasing noise deteriorates performance. It can also be seen that as the rank correlation between the distance matrices decreases, that is, as the estimates of the cross-distances between the anchors and the targets reduce its quality, the error in localization increases.

## B. Localization under physical transmission models

Although we have shown the effectiveness of Ordinal UNLOC in simulations using ordinal data, such ordinal data come directly from comparing distances under white noise. However, effective noise in practice is expected to deviate from this idealized scenario. For example, a sensor typically transmits and receives signals that are distance-dependent. For a method to be potentially useful, it must be able to deal with more realistic physical transmission models, which is the focus of our next set of numerical experiments. We consider two common models: received signal strength and time of arrival.

1) Received Signal Strength (RSS): Given a signal transmitted with power, P T , the received power, P R , is given by [37]

where α is a constant that depends on the hardware and signal characteristics and G is the path-loss exponent, which equals two in free space and is typically larger in indoor environments [47]. Given a sensor field, we now assume that signals are transmitted with strength P T and the received signal strength (RSS) is given according to (20). To mimic realistic challenges of unknown environments, we conduct a simulation where G is drawn randomly in the interval [a, b] with a = 2 and b = 6 [32]. We perform three numerical experiments. First, we run Ordinal UNLOC on the RSSI values directly. This is compared with two other scenarios, where the distance is estimated from the RSSI values first using (20), and then, UNLOC [45] is applied in two ways. In the first scenario, a single estimate of the path loss exponent, G, is available for the entire environment, as is typically the case [32], [37]. In the second case, we assume Genie-aided calibration using (20), where the path loss exponent for each link available at all times. Performance for these three cases, where we plot localization error (MSE) versus the number of anchors, is shown in Fig. 5. Clearly, having all information in the Genie-aided case provides the best performance. In the more realistic cases, Ordinal UNLOC outperforms traditional localization even though some information about the path loss exponent is known to UNLOC, but no information is available to Ordinal UNLOC except for the raw RSS values.

2) Time of Arrival (TOA): In order to perform ranging using time of arrival, the signal propagation time between the transmitter and the receiver needs to be measured. In indoor environments, the time estimate is noisy, and may be modeled using a random variable as . We compare UNLOC [45] with Ordinal UNLOC. Each transmit receive link sees a different value of G (case (iii)). For Ordinal UNLOC, we directly compare the received power values. We then estimate the distances between anchors and the target using (20). In one case, a fixed G, that is derived using calibration is used. This is compared against the ideal "Genie-aided" case, where instantaneous G values of each link are available. While the Genie-aided localization performs the best, Ordinal UNLOC directly with received power outperforms UNLOC in a realistic setting. where d is the true distance between the transmitter and the receiver, c is the speed of propagation, and σ 2 T is the variance of the TOA estimates on the channel [19], [20], [48]. We perform ordinal UNLOC on the TOA measurements, where the time estimates for each transmit-receive pair are drawn from the TOA model described in (21). We enlarge the sensor field to a square of size 200 × 200 and use the normalized variance in order to better show the effects of variations in the TOA measurements [20], [49]. We plot the localization error vs the normalized variance cσ 2

T for different number of anchors in Figure 6.

## C. Hardware Experiments

Experiments were designed to validate our algorithm. Android devices were used as transceivers and placed in a lab environment. Four Android devices were designated as anchors and placed in fixed positions. One Android device was designated as the target and placed at a location unknown to the system. Bluetooth links were established between all the devices and received signal strength indication (RSSI) data was collected. The Ordinal UNLOC algorithm was applied to the collected data.

For the hardware experiment, anchors were placed approximately in the corners of a rectangle of sides 4m × 5m. To avoid symmetry, that may play role in the results, the anchors were not placed exactly in the corners, but were moved away slightly. Within the convex hull of the four anchors, targets were placed in 20 separate locations for evaluation. Since the data was collected in a lab environment, due to multipath fading [50], some paths are more reflective than others. Therefore, for each target location, from the over 10,000 measurements we collected, we reduced the number of localization attempts to 100, by only selecting the paths with large line-of-sight power using the Ricean K factor estimator described in [51]. Once this was accomplished, the reduced set of RSSI values was used to estimate the location of each target 100 times, by using these RSSI values directly with Ordinal UNLOC.  Two target nodes are shown below as examples. Using these two targets, we consider three localization configurations. In the first two configurations, the targets are localized individually (see Figure 7 and Figure 8). In the third configuration, both targets are localized simultaneously (see Figure 9). In each of the figures, the anchors are represented by green circles, and the targets locations by the red diamonds. Four scenarios were used for estimating the location of the target. In the first idealized scenario, Ordinal UNLOC was used to estimate the location of the target, when the ground-truth cross-distances between the target and the anchors and the actual distances between the anchors is assumed to be known to the algorithm. The second estimation scenario uses the RSSI values obtained from the experiments directly. The procedure followed was as described above.

The final scenario was conducted to provide insight into the effectiveness of the Ordinal UNLOC algorithm in highly reflective environments. In this case, we used (20) to estimate the distances between the target and the anchors, with a fixed G of 4, estimated using the techniques in [37]. With these distance estimates, the UNLOC algorithm, described in Section IV-C and [45], is used to estimate the target location. We compute estimates for all RSSI values, and only show the best estimate.

Note that when multiple targets are simultaneously localized (see Figure 9), we do not compare against the UNLOC algorithm, since UNLOC is designed to localize one target at a time. Furthermore, to keep the figure from being cluttered, we only show the averaged localization estimation when using RSSI values with Ordinal UNLOC, and omit showing the individual estimates. As can be seen from the results, the best performer is the case when the actual distances are known. While this is not possible in a real localization problem, it is used here to establish a performance benchmark. Using RSSI values directly with ordinal UNLOC provides better performance in spite of not needing to calibrate, and outperforms the (standard) UNLOC algorithm using calibrated values for the operating environment. The location estimate obtained by averaging all location estimates obtained by ordinal UNLOC on RSSI values provides the best performance of all the realistic scenarios.

The Ordinal UNLOC algorithm was successful in estimating the location of the target devices in three different configurations and four different scenarios, in a rich scattering environment, outperforming conventional localization methods. Recall that we had over 200,000 samples of data recorded for our experiments, from which we calculated over 2000 location estimates from twenty locations of targets. From this data, we estimate that the normalized error when using Ordinal UNLOC is on the order of 0.02m/m 2 . This is comparable to, but a little worse than the normalized error expected from using carefully calibrated fingerprinting methods (normally between 0.01m/m 2 and 0.001m/m 2 ) [35]. However, we should note here that fingerprinting methods require significant precomputing and training, and are reliable only when the environment does not change. This, in addition to our simulation results in Sections V-A and V-B, validate the effectiveness of our approach.

## D. Latency

To implement Ordinal UNLOC successfully, we require one sample for good localization. Based on our hardware experiments, we see that one in ten samples provides an accurate estimate. That is, the latency of our approach is on the order of 10 measurements per location estimate. In fingerprinting work such as [9], [29], [33]- [36], [52]- [59], on the order of 10-100 samples are needed for each estimate, showing that our work is competitive when it comes to latency. Note that although there are advantages to using fingerprinting methods for localization, these methods require extensive mapping of the environment, a priori, leading to challenges in dynamic environments, as well as maintaining a sufficiently large database to provide robustness to the algorithm.

# VI. CONCLUSIONS AND FUTURE WORK

In this paper, we present a new algorithm for performing localization with limited and noisy measurement data between anchors and targets. Since estimating distances from this limited data is typically unreliable, we instead use the measurements to compute ordinal comparisons for the distances between all the anchors and targets. We used the ordinal data and designed a three-step "Ordinal UNLOC" algorithm for localization: (1) rank aggregation; (2) function learning; and (3) unfolding localization. Through simulation results, we demonstrate the effectiveness of our algorithm. Hardware experiments, where Ordinal UNLOC is used to localize devices using Bluetooth RSSI, were successful, suggesting that the algorithm can be used in real settings, including rich scattering environments. Furthermore, unlike traditional methods which require calibration depending on the operating environment, Ordinal UNLOC can be directly applied to measured distance proxies such as TOA or RSSI.

While we have demonstrated the effectiveness of the approach, we also note a couple of limitations: the algorithm consists of a three stage process which can become computationally expensive, and for the Z tensor to be constructed, we require distance or distance proxy cross-information between all nodes, anchors and targets.

Future work involves the investigation of the role of anchor locations on localization. While ordinal data is one-bit data, another avenue of exploration is the effect of increasing the number of bits available for approximating incomplete and noisy measurements. We will investigate further, the complexity and latency of Ordinal UNLOC and compare it with other methods such as traditional multilateration and fingerprinting.

