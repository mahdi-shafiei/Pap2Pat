# DESCRIPTION

## BACKGROUND

- motivate data privacy
- limitations of differential privacy

## SUMMARY

- introduce user-entity differential privacy
- application of natural language modeling
- embodiment of balanced model

## DETAILED DESCRIPTION

- introduce user-entity differential privacy system
- generate natural language model with user-entity differential privacy
- inject noise into natural language model parameters
- determine sensitivity bound for user-entity estimator
- generate Gaussian noise using noise scale
- update natural language model parameters using average gradient and noise scale
- perform natural language task with noisy parameters
- protect users and sensitive entities from discovery
- discuss limitations of conventional differential privacy systems
- highlight inflexibility of conventional systems
- describe failure to balance model utility and data security
- introduce advantages of user-entity differential privacy system
- provide flexible protection for users and sensitive entities
- facilitate configuration of protection level and sensitive entities
- improve balance of data security and model utility
- generate accurate natural language models with strong protection
- illustrate system 100 with user-entity differential privacy system 106
- describe server(s) 102 and natural language database 114
- discuss machine learning system 104 and user-entity differential privacy system 106
- illustrate client devices 110a-110n and client application 112
- describe user-entity differential privacy system 106 on server(s) 102
- generate natural language model with learned parameters
- provide natural language model to client device 110n
- utilize natural language model on client device 110n
- discuss alternative implementation with web hosting application
- access content and services hosted on server(s) 102
- perform natural language task on server(s) 102
- provide output to client device 110n
- illustrate user-entity differential privacy system 106 generating natural language model
- access natural language dataset 202
- describe natural language dataset
- discuss users and sensitive entities in natural language dataset
- access natural language model 204
- describe natural language model
- discuss machine learning model and neural network
- determine set of sensitive data points from natural language dataset
- generate natural language model using sensitive data points
- determine average gradient using user-entity estimator
- determine noise scale for user-entity estimator
- generate parameters for natural language model
- inject noise into natural language model parameters
- update natural language model parameters using average gradient and noise scale
- perform natural language task with noisy parameters
- protect users and sensitive entities from discovery
- discuss iterative process for generating natural language model
- refine natural language model using noisy parameters
- utilize natural language model for various natural language tasks
- provide flexible and strong protection for users and sensitive entities
- improve balance of data security and model utility
- introduce user-entity differential privacy system
- utilize natural language dataset to generate parameters for natural language model
- define parameter
- generate noise using sensitive data points
- generate parameters using noise
- protect natural language data
- prevent discovery of user participation
- prevent discovery of sensitive entities
- define differential privacy
- introduce equation 1 for differential privacy
- define user-entity differential privacy
- introduce equation 2 for user-entity differential privacy
- determine set of sensitive data points
- illustrate block diagram for determining sensitive data points
- associate natural language texts with users and sensitive entities
- sample users and sensitive entities
- determine set of sensitive data points using sampling
- generate natural language model using sensitive data points
- illustrate diagram for generating natural language model
- determine sensitive data points
- identify sensitive entities within sensitive data points
- determine gradients for each user
- introduce equation 3 for determining gradients
- determine gradients using sensitive data points and sensitive entities
- train natural language model using gradients
- generate model predictions
- determine errors of natural language model
- determine bounded gradients
- clip gradients using clipping model
- utilize federated learning to determine gradients
- determine average gradient
- introduce user-entity estimator
- determine average gradient using user-entity estimator
- introduce equation 4 for determining average gradient
- utilize user sampling rate and sensitive entity sampling rate
- utilize weights for users and sensitive entities
- determine sensitivity of user-entity estimator
- bound sensitivity of user-entity estimator
- replace gradients with bounded gradients
- generate natural language model using average gradient
- provide user-entity differential privacy
- offer improved flexibility and security
- protect multiple types of data simultaneously
- provide robust security of data
- determine set of sensitive data points for generating natural language model
- generate natural language model that provides user-entity differential privacy
- define user-entity differential privacy system
- determine sensitivity bound for user-entity estimator
- determine noise scale for user-entity estimator
- utilize noise scale to determine Gaussian noise
- generate parameters for natural language model
- iterate process to generate natural language model
- summarize algorithm for generating natural language model
- initialize parameters of natural language model
- sample users and sensitive entities
- determine gradients of model parameters
- clip per-user gradients
- compute average gradient using clipped gradients
- add random Gaussian noise to model update
- utilize moments accountant to determine privacy budget consumption
- generate natural language model with user-entity differential privacy
- offer configurability options for user-entity differential privacy system
- generate natural language model with improved balance between security and model utility
- conduct studies to determine interplay between model utility and data security
- describe CONLL-2003 news dataset
- illustrate data included in CONLL-2003 dataset
- illustrate distribution of data in CONLL-2003 dataset
- describe experimental results regarding effectiveness of user-entity differential privacy system
- compare performance of user-entity differential privacy system with other models
- illustrate privacy budget consumed by user-entity differential privacy system
- illustrate performance of user-entity differential privacy system on next word prediction task
- describe components and capabilities of user-entity differential privacy system
- describe sensitive data point sampling manager
- describe natural language model training engine
- describe natural language model application manager
- describe data storage
- describe natural language dataset
- describe natural language model
- describe model parameters
- describe implementation of user-entity differential privacy system
- describe flowchart of series of acts for generating natural language model
- determine sensitive data points associated with users and sensitive entities
- determine users to represent using user sampling rate
- determine sensitive entities to represent using sensitive entity sampling rate
- generate average gradient for sensitive data points using user-entity estimator
- determine average gradient using user-entity estimator and gradients for each user
- generate natural language model that provides user-entity differential privacy
- utilize natural language model to perform natural language task
- implement natural language model application manager
- store natural language dataset
- store natural language model
- store model parameters
- describe alternative embodiments of user-entity differential privacy system
- describe computer-implemented method for implementing differential privacy
- describe non-transitory computer-readable medium storing instructions for implementing differential privacy
- describe system for implementing differential privacy
- describe at least one memory device comprising natural language dataset
- describe at least one processor configured to cause system to perform acts of generating natural language model
- describe user-entity differential privacy system
- generate natural language model
- determine sensitive data points
- generate gradients for users
- determine average gradient
- generate noise scale
- modify natural language model parameters
- generate Gaussian noise
- receive sensitive entities from client device
- determine natural language texts referencing sensitive entities
- generate noise scale using sensitivity bound
- modify natural language model to focus protection
- describe computer-readable media
- describe non-transitory computer-readable storage media
- describe transmission media
- describe network
- describe computer-executable instructions
- describe computer system components
- describe cloud computing environments
- describe service models
- describe deployment models
- illustrate computing device
- describe processor
- describe memory
- describe storage device
- describe input/output interfaces
- describe communication interface
- describe bus
- describe computing device components
- describe processor functions
- describe memory functions
- describe storage device functions
- describe input/output interface functions
- describe communication interface functions
- describe bus functions
- describe computing device variations
- describe non-limiting embodiments
- describe invention scope
- describe appended claims
- describe method variations
- describe step variations
- describe parallel step execution
- describe repeated step execution
- describe equivalent claims
- describe invention scope

