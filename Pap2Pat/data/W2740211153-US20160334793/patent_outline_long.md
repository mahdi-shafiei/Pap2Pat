# DESCRIPTION

## BACKGROUND OF THE INVENTION

- introduce UUV applications

## SUMMARY OF THE INVENTION

- demonstrate optical detector array capabilities
- summarize pose estimation performance
- conduct uncertainty analysis
- develop optical detector array for UUV navigation
- evaluate feasibility of underwater distance detection
- develop simulator for optical array dimensions
- develop pose detection and control algorithms
- evaluate image processing algorithms
- experimentally test optical detector array capabilities

## DETAILED DESCRIPTION

- introduce underwater communication links
- motivate UUV operation in formation
- describe static-dynamic system
- describe dynamic-dynamic system
- summarize formation architectures
- describe virtual structure approach
- describe behavior based methods
- describe leader-follower method
- describe artificial potentials
- describe graph theory
- motivate leader-follower strategy
- describe acoustic communication
- describe LBL systems
- describe SBL systems
- describe USBL systems
- motivate optical detection
- describe optical communication in astronautical applications
- describe challenges of underwater optical communication
- describe detector arrays for pose detection
- describe planar and curved array designs
- summarize previous studies on optical communication for UUVs
- outline goals and applications of this study

### UUV Modeling, Control and Stability

- introduce UUV modeling
- define kinematics and dynamics
- introduce two coordinate frames
- describe UUV motion in body-fixed frame
- define position and orientation in Earth-fixed frame
- introduce Euler angles
- derive velocity transformation matrix
- describe angular velocities in body-fixed frame
- introduce UUV dynamics
- derive 6-DOF nonlinear dynamic equations
- introduce Newton-Euler formulation
- derive rigid-body equations of motion
- define inertia tensor
- derive elements of inertia tensor
- introduce assumptions in dynamics analysis
- derive distance from origin to center of gravity
- relate time derivatives in Earth-fixed and body-fixed frames
- describe translational motion
- derive UUV velocity equations
- derive UUV acceleration equations
- derive rotational motion equations
- define absolute momentum
- derive time derivative of absolute momentum
- simplify rotational motion equations
- write rotational motion equations in vectorial form
- derive DOF rigid-body equations of motion
- simplify DOF rigid-body equations of motion
- introduce hydrodynamic forces and moments
- introduce UUV modeling
- analyze hydrodynamic forces
- define added mass
- derive added mass matrix
- simplify added mass matrix
- introduce strip theory
- estimate hydrodynamic derivatives
- compute hydrodynamic coefficients
- discuss hydrodynamic damping
- explain potential damping
- explain skin friction
- explain wave drift damping
- explain vortex shedding
- formulate viscous damping force
- express quadratic drag
- introduce restoring forces and moments
- define gravitational and buoyancy forces
- express restoring force and moment vector
- expand restoring force and moment vector
- introduce UUV controllers and stability
- explain PID control
- discuss PID stability for UUVs
- define UUV modeling
- derive control law
- introduce Lyapunov function
- motivate PID control
- derive PID control law
- introduce sliding mode control
- derive SMC dynamics
- define sliding surface
- analyze sliding condition
- derive equivalent control law
- add discontinuous term
- choose switching function
- derive control input
- prove SMC stability
- define Lyapunov function candidate

### Characterization of Optical Communication in a Leader-Follower UUV Formation

- introduce optical communication in UUV formation
- describe light field characterization
- design photo-detector array
- evaluate communication algorithms
- determine optimal spectral range
- measure range between leader and follower vehicles
- modify array design and algorithms
- develop control design for distance detection
- model light field using Gaussian function
- integrate light field model into UUV equations of motion
- construct and test photo-detector array
- discuss limitations of underwater light
- introduce theoretical background
- describe beam pattern
- model beam pattern using Gaussian function
- introduce inverse square law
- derive inverse square law equation
- introduce Beer-Lambert law
- derive Beer-Lambert law equation
- describe experimental setup
- conduct beam diagnostics
- perform spectral analysis
- measure intensity from light sources
- describe experimental procedure
- discuss results of 1-D experiments
- discuss results of 3-D experiments
- calculate diffuse attenuation coefficient
- determine spectral range for optical communication
- discuss performance of distance detection algorithms
- discuss limitations of experimental setup
- discuss potential applications
- discuss underwater optical communication
- discuss underwater docking
- summarize conclusions
- discuss feasibility of control design
- highlight future work

### Optical Detector Array Design for Navigational Feedback Between UUVs

- introduce optical detector array design
- motivate 5-DOF motion detection
- describe limitations of underwater optical communication methods
- introduce planar and curved array designs
- motivate unique signature and minimum detector components
- describe optical design considerations
- introduce light source and guiding beam
- describe planar array design
- describe curved array design
- introduce environmental considerations
- describe light source radiance
- describe radiance change along beam axis
- describe beam pattern using Gaussian function
- describe beam pattern angular terms
- describe RMS width of intensity distribution
- describe light attenuation in water
- describe environmental background noise
- describe Hanning window for background noise
- introduce hardware considerations
- describe detector element and signal conditioning
- describe noise sources in hardware
- describe net noise current
- describe geometrical design of array
- describe incidence angle and Lambert's cosine law
- introduce simulator
- describe simulator goals
- describe simulator inputs and assumptions
- describe operational distance for underwater communication
- describe reference frame and coordinate system
- describe array geometry and adapted coordinate axes convention
- introduce detector array design
- define planar detector array geometry
- define curved detector array geometry
- describe radiometric calculations
- describe hardware considerations
- describe environmental background noise
- describe simulator output image generation
- introduce results section
- describe simulator results analysis
- introduce SAM algorithm
- describe SAM angle calculation
- describe image parameter extraction
- describe planar array results
- describe curved array results
- compare planar and curved array results
- describe detector array comparison
- evaluate planar array performance
- evaluate curved array performance
- describe array size evaluation
- describe operational distance evaluation
- introduce experimental confirmation
- describe experimental setup
- describe empirical measurements
- compare simulator and experimental results
- introduce discussion section
- discuss simulator capabilities
- discuss array design limitations
- discuss roll rotation sensing
- discuss water column disturbances
- discuss detector noise
- discuss camera array alternative
- introduce conclusions section
- summarize detector array simulator
- summarize conclusions

### Pose Detection and Control Algorithms for Dynamic Positioning of UUVs Via an Optical Sensor Feedback System

- introduce UUV dynamic positioning via optical sensor feedback system
- describe optical system components
- explain pose detection methods
- introduce SAM algorithm
- introduce image moment invariants algorithm
- introduce phase correlation and log-polar transform algorithm
- describe analytical simulations
- explain PID and SMC controllers
- describe static-dynamic and dynamic-dynamic control scenarios
- introduce detector array interface design
- describe curved optical detector array design
- explain array size selection
- introduce two underwater applications
- describe static-dynamic system
- describe dynamic-dynamic system
- compare pose detection algorithms
- explain phase correlation algorithm
- explain log-polar transform
- describe SAM algorithm implementation
- explain image moment invariants calculation
- describe UUV modeling and control
- introduce UUV pose-based feedback control system
- explain PID and SMC implementation
- describe UUV control restrictions
- conclude UUV control system performance
- introduce pose detection and control algorithms for dynamic positioning of UUVs via an optical sensor feedback system
- define control problem for static-dynamic system
- define control problem for dynamic-dynamic system
- motivate image moments approach
- summarize calibration procedure
- describe static-dynamic system algorithm
- describe detection and control strategy
- present results for single-DOF SMC with 21×21 detector array
- present results for single-DOF SMC with 5×5 detector array
- describe x-axis PID control with 5×5 detector array
- present results for x-axis PID control
- describe case study for dynamic positioning with multiple concurrent initial pose errors
- present results for case study
- describe case study for dynamic positioning with multiple concurrent initial pose errors in the presence of added disturbances
- present results for case study with disturbances
- describe dynamic-dynamic system
- describe control strategy for dynamic-dynamic system
- present results for dynamic-dynamic system
- discuss simulation results
- compare performance of pose detection algorithms
- evaluate SMC and PID controllers
- discuss effect of detector array size on dynamic positioning
- discuss limitations of SMC
- discuss robustness of SMC
- discuss effect of external disturbances
- discuss conclusions
- summarize pose detection and control algorithms
- summarize criteria for pose detection and control algorithms
- summarize image moment invariants algorithm
- summarize case studies
- summarize PID control evaluation
- summarize SMC evaluation
- summarize detector array size evaluation
- summarize dynamic-dynamic system case study
- discuss UUV pose detection and control algorithms
- discuss static-dynamic and dynamic-dynamic systems
- discuss pose detection algorithms
- discuss control algorithms
- discuss detector array size
- discuss conclusions
- conclude UUV pose detection and control algorithms

### Experimental Pose Detection for UUV Control System Using an Optical Detector Array

- introduce UUV control system using optical detector array
- describe UUV docking stations
- explain limitations of current docking systems
- introduce optical communication for docking
- describe experimental setup for pose detection
- explain detector array design
- describe hardware selection
- introduce image moment invariants approach
- describe analytical pose detection algorithms
- explain detector module components
- describe photodiode array design
- explain reverse-bias circuit
- describe data sampling and transmission
- introduce methodology
- describe wave and tow tank experiments
- explain scaled model of Portsmouth Harbor
- describe factors affecting system performance
- introduce calibration procedures
- describe photodiode calibration
- explain temperature calibration
- describe noise and cross-talk calibration
- introduce pose calibration procedure
- describe x-axis calibration
- describe y-axis calibration
- describe z-axis calibration
- explain data analysis for pose detection
- introduce performance evaluation
- describe positioning accuracy criterion
- describe velocity estimation criterion
- explain dynamic experiments
- describe stochastic assessment of pose uncertainty
- introduce Monte Carlo analysis
- describe environmental parameters
- describe hardware parameters
- explain simulator design
- describe uncertainty modeling
- introduce results
- describe calibration results
- explain photodiode response consistency
- describe temperature dependence of SM05PD1A
- explain temperature sensitivity
- describe experimental results
- explain pose estimation accuracy
- describe velocity estimation accuracy
- summarize system performance
- introduce experimental pose detection system
- describe system components and calibration
- discuss noise and cross-talk in system
- outline underwater calibration procedure
- summarize performance evaluation results
- describe pose estimation algorithm
- discuss x-axis pose estimation
- discuss y and z-axis pose estimation
- discuss velocity estimation
- present case 1: aligned light source and UUV platform
- present case 2: maximum offset between light source and UUV platform
- discuss stochastic model results
- describe Monte Carlo simulation approach
- present simulation results for case 2
- discuss uncertainty in pose estimation
- present second Monte Carlo simulation scenario
- discuss results for second scenario
- discuss yaw angle detection
- discuss discussion of results
- discuss limitations of system
- discuss potential contributors to errors
- discuss importance of filtering velocity signal
- discuss potential hardware improvements
- discuss design features of detector array
- discuss limitations of calibration range
- discuss importance of water clarity
- discuss Monte Carlo simulation results
- discuss image moments invariants method
- discuss noise tolerance of algorithm
- discuss distinguishing between y-axis and yaw motion
- discuss cross-talk between yaw and y-axis motion
- discuss importance of validating yaw motion
- discuss potential applications beyond UUV navigation
- discuss use of detector array for short-range optical communication
- discuss use of detector array for in situ beam diagnostics
- discuss potential use of COTS cameras
- discuss conclusions of study
- summarize performance of optical detector array unit
- discuss Monte Carlo analysis results
- discuss uncertainty in pose estimation
- discuss importance of diffuse attenuation coefficient
- discuss importance of hardware noise
- discuss algorithm's discrimination power
- discuss faulty detections of yaw rotation
- discuss expected increase in pose detection uncertainty in field implementation
- discuss importance of considering environmental disturbances
- discuss importance of mechanical design and construction of docking platform

### Discussion

- characterize underwater environment
- experiments in wave and tow tank
- effective range between light source and detector
- beam pattern emitted from light source
- limitations of single light source with Gaussian profile
- potential of multiple light sources or different intensity patterns
- detector array detects translational and rotational changes
- limitations of larger array size
- light source characteristics affect detection capabilities
- pose feedback obtained through image processing algorithms
- phase correlation and log-polar transform algorithm
- Spectral Angle Mapper (SAM) algorithm
- image moments invariants algorithm
- evaluation of two types of controllers (PID and SMC)
- response characteristics of PID and SMC
- overshoot in PID controller
- satisfactory performance of SMC
- accuracy of pose detections tested for two types of docking applications
- factors contributing to accuracy of pose detections
- uncertainty analysis through Monte Carlo simulations
- distinguishing motion in the same plane

### Conclusions

- proof of concept of optical detection system
- pose detection capabilities and limitations
- limitations of system in real-world conditions
- characterization of underwater environment
- selection criteria for optical array design
- evaluation of performance criteria
- development of pose detection algorithms
- evaluation of control algorithms
- analysis of effect of detector number on array
- experimental results of pose detection accuracy

### Photodiode Data Collection Procedure Using Beagleboard-XM and Two Arduinos

- data collection procedure using BB-XM and two Arduinos
- photodiodes intersect light and convert to current
- voltage reading across terminals using Arduino A/D input pins
- Arduinos connected serially to BB-XM via USB ports
- BB-XM connected to PC through RS-232 to USB cable
- Python serial libraries used for communication
- code to collect data written in Python programming language
- Arduino sketch uploaded to Arduino Boards
- disconnect Arduino from PC
- powering BB-XM using RS-232 to USB cable
- connect to BB-XM from PC through minicom
- access data collection program in BB-XM
- data collection and observation on PC side

### Programs for Experimental Data Collection and Analysis

- programs written for photodiode data collection and analysis

