# DESCRIPTION

## STATEMENT OF GOVERNMENT FUNDING

- acknowledge government funding

## BACKGROUND

- motivate graph search

## SUMMARY

- introduce method for analyzing data
- application of deformation field
- embodiment of system for analyzing data
- alternative embodiment of system
- foreshadow advantages

## DETAILED DESCRIPTION

- define terminology
- explain singular forms
- define ranges
- explain "optional" and "optionally"
- define "comprise" and variations
- explain "exemplary"
- explain "such as"
- disclose components
- explain combinations of components
- discuss methods and systems
- explain hardware and software embodiments
- discuss computer program products
- explain block diagrams and flowcharts
- discuss computer program instructions
- explain computer-readable memory
- discuss article of manufacture
- explain computer-implemented process
- discuss graph search for quantitative analysis
- explain limitations of graph search
- introduce non-Euclidean graph representation
- discuss advantages of non-Euclidean graph search
- explain object segmentation
- discuss medical imaging applications
- explain graph techniques for surface segmentation
- discuss unfolding techniques
- explain partial volume effects
- introduce non-Euclidean deformation field
- discuss adaptive node density
- explain subvoxel accuracy
- discuss validation of non-Euclidean graph approach
- explain non-Euclidean graph search
- discuss Euclidean space graph generation
- explain feasibility constraints
- discuss surface smoothness and separation constraints
- explain cost assignment
- discuss non-Euclidean graph search steps
- define deformation field F(x, y, z)
- derive deformation field F(x, y, z) from negative diffusion of gradient vectors
- illustrate 2D deformation field of an example B-scan
- explain partial volume effects
- illustrate impact of regularization on deformation field
- describe content-based graph construction
- apply deformation field F(x, y, z) to deform each node in graph
- describe surface smoothness constraints
- define bottom-most and top-most neighbors of a node
- illustrate structure of conventional graph and non-Euclidean deformed graph
- describe proper ordering of neighbor ranges
- enforce monotonicity of target surface
- impose smoothness constraints
- extend method to simultaneously detect interrelated surfaces
- illustrate comparison of conventional graph construction and non-Euclidean deformed graph model
- show node distribution between conventional graph construction and non-Euclidean deformed graph model
- assign costs to each node
- derive on-surface cost associated with each node
- deform original cost w(x, y, z) to w*(x, y, z)
- illustrate node density after non-Euclidean deformation
- describe experimental results of surface segmentation
- perform quantitative analysis of retinal layers
- compare graph search segmentation to human experts
- evaluate performance of Non-Euclidean Graph Search
- obtain OCT volumes from normal subjects
- down-sample OCT volumes
- evaluate results of standard graph search and subvoxel graph search
- illustrate B-scan with segmentation
- illustrate down-sampled OCT volume
- describe parameter settings
- compute thickness of region bounded by surfaces
- illustrate example mapping of tissue thickness
- perform statistical analysis
- calculate surface positioning errors and layer thickness errors
- compare errors for standard graph search and non-Euclidean graph search
- illustrate segmentation of OCT volume with conventional graph search
- illustrate segmentation of OCT volume with non-Euclidean graph search
- compare unsigned thickness errors of both approaches
- perform vascular wall segmentation using 3-D MR Dataset
- illustrate vascular wall segmentation results
- describe three-dimensional graph search in non-Euclidean space
- discuss advantages of non-Euclidean approach
- describe application to higher-dimensional image segmentation
- illustrate system for analyzing data
- describe first device configured to receive data
- describe sensing unit configured to receive sensor data
- describe network for providing communication
- describe second device configured to request and receive sensor data
- describe graphing unit configured to generate graph
- describe deformation unit configured to apply deformation to graph
- describe segmentation unit configured to identify surfaces
- describe formatting unit configured to format data and information
- illustrate block diagram of system
- discuss functional description of system
- describe system 1700
- introduce third device 1718
- describe interface unit 1720
- introduce method 1800 of analyzing data
- receive volume data
- generate first graph
- determine deformation field
- apply deformation field
- segment second graph
- introduce method 1900 of analyzing data
- receive intensity data
- generate graph
- determine deformation field
- deform graph
- identify surface
- provide representation
- describe computer implementation
- introduce operating environment
- describe computing system environments
- describe program modules
- describe distributed computing environments
- introduce computer 2001
- describe system bus 2013
- describe processor 2003
- describe system memory 2012
- describe mass storage device 2004
- describe operating system 2005
- describe segmentation software 2006
- describe segmentation data 2007
- describe network adapter 2008
- describe input/output interface 2010
- describe display adapter 2009
- describe display device 2011
- describe human machine interface 2002
- describe remote computing devices 2014a,b,c
- describe network 2015
- describe application programs
- describe computer readable media
- describe artificial intelligence techniques
- describe expert systems
- describe machine learning
- describe scope of invention

