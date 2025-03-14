# I. INTRODUCTION

Programming robotic arm manipulators for tasks that require complex, dynamic movements is often difficult and time-consuming. Learning from Demonstration (LfD) offers a more efficient approach to teach motor skills to robots [1], [2], since expert demonstrations can be directly used to learn a suitable representation of the movement to solve the given task. However, it is desirable that these approaches can provide some guarantees regarding operational constraints, which are more difficult to specify from expert demonstration.

One of the most popular LfD approaches is the dynamic movement primitive (DMP), a nonlinear dynamical system that can fit expert demonstration trajectories by decoupling a nonlinear forcing function from the nominal attraction behavior. Since their introduction by Schaal et al, [3], DMPs have been applied to learning a wide range of skills [4], [5], [6], [7], [8]. While they can be reparameterized by their start and goal positions and preserve the qualitative shape of the trajectory, incorporating arbitrary operational constraints on the trajectory is very difficult. Without care, a new start or goal pose may cause the robot to collide with itself or the environment. Consequently, working with DMPs in arbitrary environments require careful engineering to ensure that the computed trajectories stay collision-free and within workspace bounds.

For dynamical systems like DMPs, safety can be defined by whether the trajectories of the system remain within a † Department of Computer Science, Brown University, Providence, Rhode Island. {sshaw4,gdk}@cs.brown.edu. ‡ Mitsubishi Electric Research Labs (MERL), Cambridge, MA USA. lastname@merl.com specified 'safety set.' Such a set can be defined by removing obstacles from the set that defines the robot's workspace or by joint inequality constraints derived joint limits on a manipulator arm. Ames et al. [9] introduce Zeroing-Barrier Functions (ZBFs), a formalism that can certify whether a dynamical system is forward-invariant in a safety set, i.e. whether the state of the system is always in the safety set. A key challenge is to find a suitable barrier function that can be used to certify satisfaction of all constraints desired by the user.

We present Constrained DMPs (CDMPs) which allow constraint satisfaction for the motor skills learned using DMPs. Given a user-defined ZBF that specifies the safety set in the robot workspace, we optimize a set of parameters to the DMP's nonlinear forcing function to guarantee that the DMP trajectory lies within the safety set. We cast the optimization as a nonlinear program which can be solved using offthe-shelf non-linear program solvers such as IPOPT [10], [11]. Figure 1 shows a motivating example where an initial demonstration was provided in an obstacle-free environment. The DMP computed using the demonstration collides with a novel obstacle during execution. The algorithm proposed in this paper is able to compute a collision-free trajectory.

Contributions. This paper presents the following contributions:

1) A novel formulation for Constrained DMPs which can satisfy operational constraints to ensure safe reproduction of motor skills using demonstrations. 2) Demonstrations on several examples where we can successfully compute CDMPs which satisfy operational constraints in the robot workspace.

Compared to the past work on incorporating collision avoidance constraints in DMPs [12], [13], [14], [15], we are able to provide guarantees for safety set invariance for the new trajectories, which encompass obstacle-avoidance, workspace constraints, etc.

## II. RELATED WORK

Learning from demonstration, or imitation learning, is an active area of research in the robot learning community [1]. In general, there are two common behavior representations that are trained by LfD: a stochastic policy or a dynamical or geometric representation of a concrete trajectory. A case of the latter, DMPs have been widely successful in learning motor skills for robots because they present a simple dynamical system that can encode a wide range of motor skills. DMPs enjoy wide application in a variety of robot tasks, but are unable to incorporate operational constraints with strong guarantees. Collision avoidance is usually incorporated by combining DMPs with artificial potential fields (or using other similar heuristics) [12], [13]. In general, the technique lacks a formulation that can naturally incorporate any form of positional constraint without the time-consuming experimental tuning processes or risks of local minima that artificial potential field-like approaches possess ( [13], Sec. V-B). More recently, there has been some work on using DMPs with sampling based methods to allow collision avoidance [16]. These methods use sampling to find a state from which the DMP can avoid obstacles. In contrast, in the proposed method, we perturb the learned forcing function to satisfy constraints on the robot's workspace.

We also draw inspiration from approaches in safety-critical control that uses optimization-based approaches to constrain the control inputs of a dynamical system to maintain strong guarantees of the system's safety. At the center of these approaches are control Lyapunov functions (CLFs) [17] and control barrier functions (CBFs) [9], [18], [19], which certify convergence and safety set forward-invariance, respectively. In a similar vein, Tedrake et al. [20] compute Lyapunovstable regions for locally-defined LQR controllers via sumof-squares optimization. Since a DMP is just a nonlinear dynamical system, we leverage Ames et al.'s zeroing barrier function (ZBF) formalism (which Ames et al. extends to control-affine systems to define CBFs) to certify the forwardinvariance of a DMP in a predetermined safety set [9], [18].

A key insight of our work is to perturb the weights of an already-learned DMP to admit an already-existing ZBF so that the DMP is guaranteed to be forward-invariant in a user-defined safety set. There is a large body of work that constrains more general parameterizable dynamical systems via Lyapunov functions [21], [22], [23] and contraction mappings [24]. While these techniques guarantee convergence of the dynamical system, such methods cannot maintain the DMP's ability to generalize motions over changes in the attraction point, a property that our approach preserves. Nomotista et al. [25] leverage a diffeomorphism to map non-convex obstacles into a complex space, where they constrain dynamics of a control system via CBF. Khansari-Zadeh et al. [26] propose a real-time dynamical-reshaping approach for avoidance of convex obstacles. While these approaches work for obstacles that follow their assumptions, it is unclear if such an approach can be modified for avoidance of noncompact sets as well. Ohnishi et al. [27] consider CBFs for learning agents to avoid more abstract task-specific constraints for more agents whose dynamics are learned via reinforcement learning. We solve a task-specific learning-bydemonstration problem instead.

## III. BACKGROUND

In this section, we review the mathematical details of DMPs and ZBFs relevant to our construction of CDMP.

# A. Dynamic Movement Primitives

DMPs were first introduced by Schaal et al. [3]. To remove explicit time dependency, they use a canonical system to keep track of the progress through the learned behavior:

where s = 1 at the start of DMP execution (and α s > 0) and τ > 0 specifies the rate of progress through the DMP.

To capture attraction behavior, DMPs use a spring-damper system (the transformation system) with an added nonlinear forcing term. Writing the DMP equations as a system of coupled first-order ODEs yields:

where g denotes the goal pose. The forcing term is defined as a radial-basis function:

where h i and c i denote the width and center of the Gaussian basis functions, respectively. The forcing term is learned from the demonstration by solving a locally weighted regression to fit the demonstration provided by an expert.

# B. Zeroing Barrier Functions

Since DMPs are formulated as an autonomous nonlinear dynamical system, we can certify them for safety set forward-invariance using Ames et al.'s zeroing barrier function formalism [18], [9].

First, let h(x) : R n → R be a continuous differentiable function. Define set C to be the following super-level set:

Assume we have a nonlinear dynamical system of the form:

We call h(x) ZBF if the following inequality holds:

where α : R → R is a class-K function [9]. Like Ames et al., we consider the specific case

with constant γ > 0. Given a dynamical system (9), if we can find a valid ZBF h(x)(or alternatively, change our dynamical system to admit an existing ZBF), then our dynamical system ẋ = f (x) is forward-invariant in the set C , i.e. if x 0 ∈ C and ẋ = f (x) then:

## IV. CONSTRAINED DYNAMIC MOVEMENT PRIMITIVES

We now present our formulation of the constrained DMP, which we cast as a nonlinear optimization problem.

The primary insight behind our proposed formulation of CDMPs is to take an existing DMP with a forcing function learned from an expert trajectory, and then optimize perturbations to this forcing term (Sec. IV-A) so that the DMP dynamical system admits a ZBF that certifies trajectory generation within a pre-defined safety set. This safety set (and ZBF) is constructed by composing signed-distance fields from primitive convex polytopes (Sec. IV-C).

# A. CDMPs as a Nonlinear Optimization Problem

As was explained earlier in Section III-A, the system of equations and the forcing function for DMP could be written and expressed by the following system of equations:

where

is the forcing function expressed as sum of radial basis functions and which are learned from the provided expert demonstration.

We introduce additional parameters to the DMP dynamical systems that can be optimized to allow constraint satisfaction. In the proposed formulation, CDMPs are computed by optimizing a set of perturbations {ζ i } of the original (regressed) weights {w i } of the learning forcing function to obey the ZBF inequality for forward-invariance in the user-defined safety set. More formally, we define the perturbed forcing function as follows:

where ζ i are the decision variables optimized for the formulated optimization problem. We note that this is only one possible way to perturb the DMP forcing function, which we based on its most common representation as a radial-basis function in the DMP literature.

We now introduce the formulation of the nonlinear optimization problem to compute the parameter set p = {ζ i }:

subject to the dynamic constraints

where x = [s, z, y] T and p is the set of parameters (including the ζ i ). Note that the function c(•, •, •) represents the cost functional for the optimization problem. The simplest cost function can penalize the L2-norm of the decision variables

The problem formulation represented by Equations ( 16)-( 19) is converted to a finite dimensional discretized problem using PyRoboCOP [11], which is then transcribed into a nonlinear program. The resulting NLP is then solved using IPOPT in the PyRoboCOP environment.

# B. Perturbation from Original Trajectory

Another important consideration during computation of constrained DMP is to limit the amount of the deviation of the CDMP from the original DMP trajectory. The design of CDMP could be seen as a trade-off between constraint satisfaction and the original forcing function. This trade-off could be controlled using a hyperparameter which constrains the maximum allowable deviation between the original DMP and CDMP, which can be added as another constraint to the trajectory optimization problem. More formally, let us denote the original DMP trajectory as { ỹ(t)}, t ∈ [t 0 ,t f ] and the hyperparameter for the deviation from the original trajectory as ε. Then, the additional constraint could be represented as follows:

With this additional deviation constraint, the optimization problem can then be written as follows:

s.t., ( 17)-( 19)

Depending on the value of ε, we can obtain a family of CDMPs that will allow different amount of deviation of the new trajectory from the original DMP trajectory.

# C. ZBFs from Signed-Distance Functions

Given an obstacle set Ω with boundary ∂ Ω in R 3 , we use the usual definition of a signed distance function (SDFs) σ : R 3 → R to the boundary of the obstacle [28]:

and where the distance (or metric) d(x, ∂ Ω) is defined by

We know that |∇σ | = 1 wherever this gradient is well-defined (and for convex polytopes, this is true whenever σ (x) > 0). We also know that the implicit surface embedded in R 3 defined by the set of all points x ∈ 3 where σ (x) = 0 is also the boundary of the obstacle, and σ (x) < 0 whenever x is inside the obstacle. Thus, SDFs fulfill the requirements for a function to designate a safety set (what we denote as h(x) above) and certify that a DMP trajectory is also invariant in the safety set Ω c .

Given two SDFs σ 1 and σ 2 representing obstacles Ω 1 and Ω 2 , we would typically find the SDF of Ω 1 ∪ Ω 2 by taking their minimum σ (p) = min(σ 1 (p), σ 2 (p)). However, this poses problems of differentiability, which is necessary for IPOPT to solve our optimization problem. We resolve this issue by computing a twice-differentiable lower-bound approximation of the minimum function as shown in [29], which is necessary because the ZBF constraint contains a first-order derivative of ZBF h(x).

where k can be interpreted as a blending radius. With repeated application of the smin operation, we can have a twice-differentiable SDF union of all the obstacles known in the workspace.

We then state the following lemma, given the informal arguments above about our current ZBF constructions: Lemma IV.1. Let ẋ = g(x) be a DMP, γ > 0, h : R 3 → R be a ZBF, and C = {x ∈ R 3 : h(x) > 0}. If there exists ζ 1 , ..., ζ n such that the constrained DMP ẋ = g(x) admits the ZBF inequality ḣ(x)+γh(x) ≥ 0 along trajectory x(t), then CDMP g's trajectory x(t) is forward-invariant in C .

Proof. Application of Ames et al.'s Proposition B.1 [9].

As an implication of the above lemma, we can claim that if our solver is able to find feasible solution to ( 16)-( 19) to satisfy the ZBF condition, the optimized CDMP trajectory is guaranteed to stay within the safety set.

## V. EXPERIMENTS AND RESULTS

We tested our algorithm in numerical and physical experiments to understand its efficacy. The numerical examples are used to verify that our method could be used with a wide variety of ZBFs (and thus safety sets), test our method's performance against existing methods for set-invariance of DMPs, and understand its computational efficiency. Our physical experiments showed feasibility of our algorithm in a real-world setting. All the experiments presented in the next Section are performed using a Python-based interface to IPOPT presented in [11].

# A. Numerical Experiments

We first present numerical experiments to verify that CDMPs are forward-invariant in the safety set as defined by the ZBF. We recorded an initial demonstration end-effector trajectory kinesthetically taught by a human expert in R 3 (see Figure 2). Then, we impose a runtime constraint that the robot trajectory needs to follow state constraints of the type x(t) ∈ (X (t)) c , where X (t) could be non-compact and could be defined by a user depending on a task of interest. This kind of constraint satisfaction arises in robotic tasks where the task-specific constraint requires that the robot should always stay in a certain set during the task. An example of such a task could be an assembly task where the end-effector needs to always stay in a pre-defined set with respect to the part positions. As could be seen in Figure 2, the CDMP trajectory is able to satisfy the specified constraint. Furthermore, we can also easily specify and satisfy timedependent state bounds for the trajectory which might be useful for a lot of tasks. However, we skip these results for brevity.

# B. Comparison to Dynamic Artificial Potential Fields

We compare our method to the dynamic (velocitydependant) artificial potential field method introduced in [13] using a generated straight-line trajectory. As with many potential field methods, it is not difficult to construct an adversarial obstacle configuration that causes the DMP to be trapped in a local minimum (see Fig. 3). Even with these obstacles, our CDMP method is able to successfully reach the goal. This demonstrates that our method is fundamentally different from DMPs perturbed with potential field methods, Fig. 2: CDMP trajectory constrained to lie within a cone. The ZBF was constructed using the SDFs of a circles arranged along an axis, their radii paramaterized by z. (Best seen in color.) Fig. 3: CDMP and DMP with a dynamic potential field learned from generated straight-line demonstration. The former is able to reach the goal, while the latter is caught in a local minimum. (Best seen in color.) and can allow for workspace constraint satisfaction in a more principled manner.

# C. Computational Efficiency

Apart from the complexity of the constraints, the computational time for CDMP depends on the size of the NLP created using the original demonstration trajectory. The size of the problem depends on the number of collocation points for the trajectory. We note that constraints are only enforced at the collocation points when the CDMP optimization problem is transformed into a finite dimensional NLP, so the constraints may be violated between the collocation points if the discretization is too coarse. We evaluate the computational efficiency of our algorithm by varying the number of collocation points during optimization (Fig. 4). All experiments were run using unoptimized code on a Lenovo Thinkpad equipped with an Intel i7-8565U processor and 16Gb of RAM. While fewer collocation points result in faster Fig. 4: Experimental runtime of wall-jump problem. Fig. 5: While n = 10 collocation points was much faster to compute (Fig. 4), the trajectory was still in collision with the obstacle since the ZBF was too coarsely enforced relative to n = 50. compute times, the trajectory in between collocation points is more likely to intersect with obstacles (Fig. 5). As a result of this trade-off, the correct number of collocation points should be chosen based on the problem. Figure 4 shows a nonlinear trend between the number of collocation points and the computational time. This is not surprising since we usie an interior-point method (IPM) for a non-convex optimization, where the computational time can increase sharply with problem size.

# D. Physical Robot Experiments

We tested our CDMP on two different physical robots in three different domains: a wall-hopping domain (see Figure 1), a chess-piece moving domain (see Figure 6), and a whiteboard drawing domain (see Figure 7). We test the overall efficacy of our approach in the wall-hopping and chessmoving domains, and the drawing domain tests the CDMP's ability to preserve the qualitative geometry of the original DMP trajectory. A video showing the implementation of the proposed algorithm in these environments could be found here https://youtu.be/hJegJJkJfys.

In the wall and chess domains, we use a Kinova Gen3 robot. In these experiments, a user kinesthetically demonstrates the robot a movement in a workspace free of obstacles. To test the CDMP's ability to generate collision-free trajectories, we introduce new obstacles in the environment. The environment for the wall-hopping domain can be seen in Figure 1, where the CDMP finds a collision-free trajectory. Similarly, for the chessboard environment in Figure 6 For the drawing experiments, we use a Mitsubishi Electric Assista industrial manipulator arm equipped with a force/torque (F/T) sensor at the wrist. The F/T sensor is used to design an admittance controller [30] to move the robot in a kinesthetic teaching mode for contact-rich tasks. This controller is used to provide demonstrations on a white-board to draw several shapes such as a rectangle and a triangle (see Figure 7). Then, we add some constraint sets in the original shapes, which the robot should avoid while trying to re-create the shape. The initial demonstration and the final CDMP trajectories for the experiments could be seen in Figure 7. In all cases, the CDMP is able to find feasible solutions while preserving the geometric shape of the drawings in both the experiments.

## VI. CONCLUSION AND FUTURE WORK

In this paper, we propose a method for incorporating operational constraints while generating trajectories for generalizing to novel initial and goal locations. Our algorithm, constrained dynamic movement primitives, can incorporate task and environmental constraints when generalizing to novel conditions. The proposed CDMP was demonstrated on several examples for collision avoidance in the presence of obstacles of different shapes and sizes, and also preserves the qualitative geometry of the trajectory.

Even though the formulation can, in theory, include complex operational constraints like self-collision, joint limits, etc., we need computationally efficient and smooth barrier function representation of these constraints to be able to compute safe trajectories. In the future, we would like to use the proposed formulation to consider more constraints past end-effector safety sets that can be more easily expressed in different parameterizations of the robot configuration space, such as joint limits, self-collision avoidance, etc. These constraints result in a complex, constrained optimization  problem which needs additional, non-trivial work to find a suitable zeroing barrier function. Another limitation is the computational efficiency with respect to the size of the problem. In this paper, we use IPOPT to solve the resulting optimization which can not make efficient use of warm starting. In our future work, we plan to integrate our problem with an active-set solver to improve the computational efficiency.

