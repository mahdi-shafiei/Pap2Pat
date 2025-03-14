# DESCRIPTION

## FIELD

- relate to computerized systems and methods

## BACKGROUND

- introduce large communication networks
- motivate managing networks efficiently
- describe computerized control planes
- summarize advantages of control planes
- limitations of control plane technology
- prior art solutions for control plane scalability
- motivate flexible deployment of distributed control planes
- describe next generation of networked systems
- motivate virtualized infrastructures with performance guarantees
- describe importance of flow delay
- motivate automated deployment of virtualized infrastructures

## SUMMARY OF THE INVENTION

- introduce electronic data transmission scheduling system
- describe electronic reliability checker
- calculate network mapping reliability score
- introduce electronic bandwidth checker
- calculate bandwidth allocations
- introduce electronic resource planner
- develop computer-implementable deployment strategy
- introduce electronic flow delay checker
- determine end-to-end delay
- use end-to-end delay in deployment strategy
- introduce method for electronic data transmission scheduling
- examine possible data transmission routes
- calculate bandwidth allocations
- prepare computer-implementable deployment strategy
- determine end-to-end delay
- use end-to-end delay in deployment strategy
- describe automatic monitoring and decision-making method

## DETAILED DESCRIPTION OF AN EMBODIMENT OF THE INVENTION

- introduce computerized network management system
- analyze network deployment impact
- provide computer-operable deployment strategy
- define network control operations
- direct physical adjustments to data transmission paths
- limit data end-to-end transmission delay
- provide black-box optimization framework
- quantify control traffic effect
- formalize distributed control plane optimization problem
- tune reliability and bandwidth requirements
- introduce generic black-box optimization process
- specify computerized system components
- identify computerized processing entities and electronic aggregators
- enable automated deployment of computerized processing entities

### Relevant Terminology and Definitions

- define computerized processing entity
- define virtual computerized processing entity
- define electronic aggregator
- define data transmission node
- define data transmission link
- define network link propagation delay
- define network topology
- define bandwidth
- define electronic bandwidth checker
- define network flow
- define flow delay
- define electronic flow delay checker
- define reliability
- define electronic reliability checker
- define computer-implementable deployment strategy
- define electronic resource planner
- describe limitations of prior art
- describe distributed control plane
- describe control plane scalability
- describe reliability models
- describe cloud computing
- describe VM placement
- describe elastic network services
- describe VNF placement
- describe importance of traffic consideration
- describe shortcomings of prior art
- describe formal problem definition
- describe assumptions and requirements
- describe electronic data transmission scheduling system
- describe electronic automated rescheduling system
- describe electronic power utilization manager
- describe computerized network
- describe electronic data transmission scheduling system operation
- define network components
- describe control plane architecture
- explain data transmission nodes
- introduce electronic reliability checker
- describe electronic bandwidth checker
- introduce electronic resource planner
- describe electronic flow delay checker
- explain electronic network monitor
- describe electronic change detector
- introduce electronic power utilization manager
- represent network topology as graph
- define node and link operational probabilities
- calculate delay bound using network calculus
- apply network calculus to calculate worst-case delay
- describe guaranteed performance service disciplines
- formulate delay bound calculation
- describe electronic reliability checker functionality
- formulate network mapping reliability score calculation
- describe electronic bandwidth checker functionality
- formulate bandwidth allocation calculation
- describe electronic flow delay checker functionality
- formulate delay bound calculation for sub-flows
- describe formal problem solved by embodiments
- formulate delay bounded deployment problem
- describe constraints for deployment problem
- formulate objective function for deployment problem
- describe constraint for node association
- describe constraint for processing entity deployment
- describe constraint for reliability threshold
- describe constraint for bandwidth allocation
- describe constraint for delay bound
- describe constraint for backlog bound
- describe constraint for sub-flow routing
- describe constraint for binary decision variables
- define electronic bandwidth checker
- motivate processing components and workflow
- introduce computerized workflow
- describe electronic resource planner
- perform initial configuration and input
- map computerized processing entities
- associate connectivity among entities
- estimate traffic
- perform routability check
- perform delay and backlog verification
- determine routability
- output deployment strategy
- specify end-conditions
- discuss practical considerations
- express workflow mathematically
- discuss computing equipment and methods
- provide exemplary implementation
- highlight differences to prior art
- define prior art limitations
- motivate invention
- summarize prior art differences
- introduce advantages
- describe applications
- explain guaranteed performance
- highlight delay guarantees
- describe queuing delay consideration
- introduce iterative optimization process
- explain flexible implementation
- describe offline and online usage
- introduce application example
- describe control plane deployment problem
- explain computerized workflow
- detail mapping step
- describe simulated annealing algorithm
- define cost function
- explain bandwidth verification
- describe flow delay verification
- introduce reliability computation
- describe approximation method
- detail association step
- describe simulated annealing algorithm for association
- explain cost function for association
- describe new solution generation
- explain redoAssociation signal
- conclude association step
- define traffic estimation
- estimate bandwidth demands
- introduce simple traffic estimation model
- estimate flow demand
- estimate burstiness of flow
- introduce bandwidth verification
- check routability of flows
- formulate maximum concurrent flow problem
- solve maximum concurrent flow problem
- introduce dual of maximum concurrent flow problem
- solve dual problem using FPTAS
- implement FAS algorithm
- accelerate bandwidth verification
- illustrate optimization time
- introduce delay and backlog bound verification
- calculate routability indicator χ
- engage genetic algorithm or column generation heuristic
- illustrate time reduction ratio
- execute entire process
- illustrate total running time
- introduce exemplary use cases
- describe computer-implementable deployment strategy output
- illustrate deployment plan
- introduce analytic tool
- quantify impact of deployment strategy
- illustrate failure probability versus link bandwidth
- determine minimum data transmission link bandwidths
- illustrate influence on reliability
- quantify influence on reliability
- illustrate trade-off between flow delay and bandwidth
- employ embodiments as practical tool
- develop flexible deployment policies
- provide computerized analytic tool
- assess impact of deployment strategy
- quantify trade-off between bandwidth and reliability
- make decisions on network infrastructure upgrades

### Example #2: Deployment of VMs for a Cloud Computing Task

- deploy computational task in distributed big data framework
- control one or many computerized worker entities
- map deployment of distributed computational infrastructure
- associate worker entities with distributed agents

### Application Example #3: Deployment of a VNF Service Chain

- deploy service chains comprising connected VNF instances
- deploy computerized processing entities without aggregator nodes
- simplify placement of VNFs on network topology
- set reliability R(G, j, C)=1 and assume N∈Ø
- execute steps from Workflow 100
- exclude “Association” step
- introduce network calculus
- define arrival curve α(t)
- define service curve γ(t)
- calculate flow delay bound
- calculate backlog bound
- relate D* and B*
- compute output bound α*
- apply concatenation property
- derive ESC of entire path
- split and route flow on selected paths
- assume b_(f(K))=b_(f)
- calculate delay bound (D*) and backlog bound (B*)
- satisfy delay and backlog constraints
- formulate delay and backlog verification problem
- solve MILP problem
- perform computerized column generation
- split problem into master problem and subproblem
- solve master problem with subset of variables
- obtain dual values
- find new variable in subproblem
- repeat process until no new variables
- ignore delay and backlog constraints
- reduce problem to maximum concurrent flow problem
- formulate dual problem
- identify new potential paths
- obey tsf(K)≤Dmax−Σe∈K te and bf(K)≤Bmax
- relax problem to LP
- initiate with subset of paths κ′
- solve relaxed master problem
- check dual constraint
- add new paths to subset κ′
- restrict δ(K) to binary values and solve problem

