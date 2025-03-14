# Introduction

Parsing text into structured semantics representation is one of the most long-standing and active research problems in NLP. Numerous parsing methods have been developed for many different semantic structure representations (Chen and Manning, 2014;Mrini et al., 2019;Zhou and Zhao, 2019;Clark et al., 2018;Wang et al., 2018). However, most of these previous works focus on parsing a single sentence, while a typical human-computer interaction session or conversation is not singleturn. A prominent example is image search. Users usually start with short phrases describing the main objects or topics they are looking for. Depending on the result, the users may then modify their query to add more constraints or give additional information. In this case, without the modification capability, a static representation is not suitable to track the changing intent of the user. We argue that the back-and-forth and multi-turn nature of human-computer interactions necessitate the need for updating the structured representation. Another advantage of modifying a structured representation in the interactive setting is that it makes it easier to check the consistency. For instance, it is much easier to check whether the user requests two contradicting attributes for the same object in a scene graph during the interactive search, which can be done automatically.

In this paper, we propose the problem of scene graph modification for search. A scene graph (Johnson et al., 2015) is a semantic formalism which represents the desired image as a graph of objects with relations and attributes. This semantic representation has been shown to be very successful in retrieval systems (Johnson et al., 2015;Schuster et al., 2015;Vendrov et al., 2015). Inspired by the dialog state tracking setting (Perez and Liu, 2017;Ren et al., 2018), we consider the scene graph modification problem as follows. Given an initial scene graph and a new query issued by the user, the goal is to generate a new scene graph taking into account the original graph and the new query.

We formulate the problem as conditional graph modification, and create three datasets for this problem. We propose novel encoder-decoder architectures for conditional graph modification. More specifically, our graph encoder is built upon the self-attention architecture popular in state-of-theart machine translation models (Vaswani et al., 2017;Edunov et al., 2018), which is superior to, according to our study, Graph Convolutional Networks (GCN) (Kipf and Welling, 2016). Unique to our problem, however, is the fact that we have an open set of relation types in the graphs. Thus, we propose a novel graph-conditioned sparse transformer, in which the relation information is embed-ded directly into the self-attention grid. For the decoder, we treat the graph modification task as a sequence generation problem (Li et al., 2018;Simonovsky and Komodakis, 2018;You et al., 2018). Furthermore, to encourage the information sharing between the input graph and modification query, we introduce two techniques, i.e. late feature fusion through gating and early feature fusion through cross-attention. We further create three datasets to evaluate our models. The first two datasets are derived from public sources: MSCOCO (Lin et al., 2014) and Google Conceptual Captioning (GCC) (Sharma et al., 2018) while the last is collected using Amazon Mechanical Turk (MTurk). Experiments show that our best model achieves up to 8.5% improvement over the strong baselines on both the synthetic and user-generated data in terms of F1 score.

Our contributions are three-fold. Firstly, we introduce the problem of scene graph modificationan important component in multi-modal search and dialogue. Secondly, we propose a novel encoderdecoder architecture relying on graph-conditioned transformer and cross-attention to tackle the problem, outperforming strong baselines which we setup for the task. Thirdly, we introduce three datasets which can serve as evaluation benchmarks for future research. 1

# Data Creation

In this section, we detail our data creation process. We start with information on scene graphs and a parser to generate them for the captions in two existing datasets, i.e. MSCOCO (Lin et al., 2014) and GCC (Sharma et al., 2018). We then describe how to generate modified scene graphs and modification queries based on these scene graphs, and leverage human annotators to increase and analyze data quality. et al. (2015) introduce scene graphs as semantic representations of images.

# Schuster

As shown in Figure 1, a parser will parse a sentence into a list of objects, e.g. "boy" and "shirt". These objects and their associated attributes and relations form a group of triplets, e.g. boy, in, shirt , boy, attribute, young and shirt, attribute, black . Although there are several scene graphs annotated datasets for images (Krishna et al., 2017), the alignments between graphs and text are unavailable. Moreover, image grounded scene graphs, e.g. the Visual Genome dataset (Krishna et al., 2017), also contain lots of non-salient objects and relations, while search queries focus more on the main objects and their connections.

The lack of a large-scale and high quality public dataset prompts us to create our own benchmark datasets. To do this, we start with the popular captioning datasets: MSCOCO (Lin et al., 2014) and GCC (Sharma et al., 2018). To construct scene graphs, we use an in-house scene graph parser to parse a random subset of MSCOCO description data and GCC captions. The parser is built upon a dependency parser (Dozat and Manning, 2016), similar to the SPICE system (Anderson et al., 2016).

## Modified MSCOCO and GCC for Graph Modification

Our first two datasets add annotations on top of the captions for MSCOCO and GCC. The parser described in §2.1 is used to create 200k scene graphs from MSCOCO and 420k scene graphs from GCC data. Comparing the two datasets, the graphs from MSCOCO are simpler, while the GCC graphs are much more complicated. According to our inhouse search log, image search queries are usually short, thus the MSCOCO graphs represents a closer match to actual search queries2 , while the GCC graphs present a greater challenge to the models. Given a scene graph G, we construct a triplet (x, y, z), where x is the source graph, y indicates the modification query, and z represents the target graph. More specifically, we uniformly select and apply an action a from the set of all possible graph modification operations A = {INSERT, DELETE, SUBSTITUTE}. The actions  DELETE. We randomly select a node from G (denoting the source graph x), and then remove this node and its associated edges. The remaining nodes and edges are then the target graph z. As for the modification query y, it is generated from a randomly selected deletion template or by MTurk workers. These templates are based upon the Edit Me dataset (Manuvinakurike et al., 2018).

INSERT. We treat insertion as the inversion of deletion. Specifically, we produce the source graph x via a DELETE operation on G, where the target graph z is set to G. Like the deletion operator, the insertion query y is generated by either the MTurk workers, or by templates.

# SUBSTITUTE.

We replace a randomly selected node from the source graph G with a semantically similar node to get the target graph. To find the new node, we make use of the AllenNLP toolkit (Gardner et al., 2017) to get a list of candidate words based on their semantic similarity scores to the old node. More details can be found in our supplementary materials.

## Crowd-sourcing User Data

As described above, apart from using templates, we crowd-source more diverse and natural modification queries from MTurk. As depicted in figure 2, we first show the workers an example which includes a source graph, a target graph and three acceptable modification queries. Then the workers are asked to fill in their own description for the unannotated instances. We refer to the templatebased version of the datasets as "synthetic" while the user-generated contents as "user-generated".

From our preliminary trials, we notice several difficulties within the data collection process. Firstly, understanding the graphs requires some knowledge of NLP, thus not all MTurk workers can provide good modification queries. Secondly, due to deletion and parser errors, we encounter some graphs with disconnected components in the data. Thirdly, there are many overly complicated graphs which are not representative of search queries, as most of the search queries are relatively short, with just one or two objects. To mitigate these problems, we manually filter the data by removing graphs with disconnected components, lowquality instances, or excessively long descriptions (i.e. more than 5 nodes). The final dataset contains 32k examples. To test the quality of our crowd-sourced dataset, we perform a limited user study with 15 testers who are not aware of the nature of the work and how we collect the dataset. We give them a random collection of instances, each of which is a triplet of source graph, modification query, and target graph. The tester would then give a score indicating the quality of each instance based on the following two criteria: (i) how well the modification query is reflected in the target graph? and (2) how natural are the query and the graphs? Regarding the second criterion, we instruct the scorer to assess whether the query and graph are human-like, grammatically and semantically. Furthermore, as most scorers are knowledgeable in image search, they are also required to evaluate whether they think the query is plausible in a search scenario.

Figure 3 shows the score distribution from 200 randomly chosen instances. We observe that most of the quality scores of 3 or 4 are due to the modification query or graphs to be unnatural. Testers tend to give the score of 1 for semantically wrong instances (e.g the modification query does not match the changes). Overall, the testers judge the data to be good with the average score of 3.76.

## Extension: Multiple Operations

In §2.3, we have introduced our basic modification operations. From the analysis of our in-house search log, more than 95% of the queries have only one or two nodes, thus a scenario in which more than one edit operation applied is unlikely. Consequently, the instances in Modified MSCOCO and GCC are constructed with one edit operation. However, in some cases, there can be a very long search description , which leads to the possibility of longer edit operation sequences. This motivates us to create the multi-operation version of a dataset, i.e. the multi-operation graph modification (MGM) task from GCC data. Please refer to the supplementary material for the details of the data creation for MGM.

# Methodology

In this section, we explore different methods to tackle our proposed problem. By analyzing the results and comparing different models, we establish baselines and set up the research direction for future work. We start by formalizing the problem, and defining the input as well as the expected output along with the notations. We then define our encoder-decoder architecture with the focus on our novel modeling characteristics: (i) the graph encoder with graph-conditioned, sparsely connected transformer and (ii) the early and late feature fusion models for combining information from the input text and graph.

# Notations. A graph is represented by x

is the number of nodes, and x i ∈ V N where V N is the node vocabulary. The edge set is denoted by x E := {x i,j |x i , x j ∈ x N , x i,j ∈ V E } where V E is the edge vocabulary.

## Problem Formulation

We formulate the task as a conditional generation problem. Formally, given a source graph x G and a modification query y, one can produce a target graph z G by maximizing the conditional probability p(z G | x G , y). As a graph consists of a list of typed nodes and edges, we further decompose the conditional probability (You et al., 2018) as,

(1) where z N and z E respectively denote the nodes and edges of the graph z G .

Given a training dataset of input-output pairs, denoted by

, we train the model by maximizing the conditional loglikelihood CLL = Node + Edge where,

During learning and decoding, we sort the nodes according to a topological order which exists for all the directed graphs in our user-generated and synthetic datasets.

## Graph-based Encoder-Decoder Model

Inspired by the machine translation literature (Bahdanau et al., 2014;Jean et al., 2015), we build our model based on the encoder-decoder framework.

Since our task takes a source graph and a modification query as inputs, we need two encoders to model the graph and text information separately. Thus, there are four main components in our model: the query encoder, the graph encoder, the edge decoder and the node decoder. The information flow between the components is shown in Figure 4. In general, we encode the graph and text modification query into a joint representation, then we generate the target graph in two stages. Firstly, the target nodes are generated via a node-level recurrent neural network (RNN). Then we leverage another RNN to produce the target edges over the nodes.

### Graph Encoder: Sparsely Connected Transformer

The standard transformer architecture (Vaswani et al., 2017;Yang et al., 2019) relies on a grid of fully-connected self-attention to obtain the contextualized representations from a sequence of elements. In this work, we propose the graphconditioned, sparsely connected transformer to encode the information from a graph. Our idea is partially inspired by the sparse transformer through factorization (Child et al., 2019). Despite the similar name, the two methods share very few similarities in both motivations and mechanisms. The architecture of our graph encoder with the sparely connected transformer is detailed below.

Compared to natural language text, graphs are structured data, which are comprised of two main components: nodes and edges. To efficiently encode a graph, we need to encode the information not only from these constituent components, but also their interactions, namely the node-edge association and connectivity. Thus, we incorporate the information from all the edges to the nodes from which these edges are originated. More formally, our edge-aware node embedding x i can be obtained from the list of source graph nodes and edges via,

where T N and T E are the embedding tables for node and edge labels respectively, and J(i) is the set of nodes connected (both inbound and outbound) to the ith node in the graph.

After getting the edge-aware node embeddings, we employ the sparsely connected transformer to learn the contextualized embeddings of the whole graph. Unlike the conventional transformer, we do not incorporate the positional encoding into our graph inputs because the nodes are not in a predetermined sequence. Given the edge information from x E , we enforce the connectivity information by making nodes only visible to its first order neighbor. Let us denote the attention grid of the transformer as A. We then define A[x i , x j ] = f (x i , x j ) if x i,j ∈ x E or zero otherwise, where f denotes the normalized inner product function.

The sparsely connected transformer, thus, provides the graph node representations which are conditioned on the graph structure, using the edge labels in the input embeddings and sparse layers in self-attention. We denote the node representations in the output of the sparsely connected transformer by [m x 1 , .., m x |x N | ].

### Query Encoder

We use a standard transformer encoder (Vaswani et al., 2017) to encode the modification query y = (y 1 , .., y |y| ) into [m y 1 , ..., m y |y| ]. Crucially, in order to encourage semantic alignment, we share the parameters of the graph and query encoders.

### Information Fusion of Encoders

In a conventional encoder-decoder model, usually there is only one encoder. In our scenario, there are two sources of information, which require separate encoders. The most straightforward way to incorporate the two information sources is through concatenation. Concretely, the combined representation would be,

The decoder component will then be responsible for information communication between the two encoders through its connections to them. In the following, we propose more advanced methods to combine the two sources of information.

Late Fusion via Gating. To enhance the ability of the model to combine the encoders' information for a better use of the decoder, we introduce a parametric approach with the gating mechanism.

Through the gating mechanism, we aim to filter useful information from the graph based on the modification query, and vice versa.

More specifically, we add a special [CLS] token to the graph and in front of the query sentence. The representation of this token in the encoders will then capture the holistic understanding, which we denote by m x G and m y for the graph and modification query respectively. We make use of these holistic meaning vectors to filter useful information from the representations of the graph nodes m x i and modification query tokens m y j as follows, where MLP is a multi-layer perceptron, indicates an element-wise multiplication, and σ σ σ is the element-wise sigmoid function used to construct the gates g x i and g y j . The updated node m x i and token m y j are then used in the joint encoders representation of Equation 5.

We refer to this gating mechanism as late fusion since it does not let the information from the graph and text interact in their respective lower level encoders. In other words, the fusion happens after the contextualized information has already been learned.

Early Fusion via Cross-Attention. To allow a deeper interaction between the graph and text encoders, we explore fusing features at the early stage before the contextualized node m x i and token m y i representations are learned. This is achieved via cross-attention, an early fusion technique. Recall that the parameters of the graph and query encoders are shared to enable encoding of the two sources in the same semantic space. That is, we use the same transformer encoder for both sources. In cross-attention, we concatenate the x (from Equation 4) and y before rather than after the transformer encoder. As such, the encoder's input is [x, y]. In the transformer, the representation of each query token gets updated by self-attending to the representations of all the query tokens and graph nodes in the previous layer. However, the representation of each graph node gets updated by self-attending only to its graph neighbors according to the connections of the sparsely connected transformer as well as all query tokens. The final representation m is taken from the output of transformer. Figure 5 shows the information flow in the cross-attention mechanism.

# Joint embedding

## Node-level Decoder

We use GRU cells (Cho et al., 2014) for our RNN decoders. The node-level decoder is a vanilla autoregressive model described as,

where z <t denotes the nodes generated before time step t, ATTN N is a Luong-style attention (Luong et al., 2015), and m is the memory vectors from information fusion of the encoders (see §3.2.3).

## Edge-level Decoder

For the edge decoder, we first use an adjacencystyle generation (You et al., 2018). The rows/columns of the adjacency matrix are labeled by the nodes in the order that they have been generated by the node-level decoder. For each row, we have an auto-regressive decoder which emits the label of each edge to other nodes from the edge vocabulary, including a special token [NULL] showing an edge does not exist. As shown in Figure 6, we are only interested in the lower-triangle part of the matrix, as we assume that the node decoder has generated the nodes in a topologically sorted manner. The dashed upper-triangle part of the adjacency matrix are used only for parallel computation, and they will be discarded. We use an attentional decoder using GRU units for generating edges. It operates similarly to the node-level decoder using Equation 11and Equation 12. For more accurate typed edge generation, however, we incorporate the hidden states of the source and target nodes (from the node decoder) as inputs when updating the hidden state of the edge decoder:

where h E i,j is the hidden state of the edge decoder for row i and column j, and z i,j-1 is the label of the previously generated edge from node i to j -1. However, there are two drawbacks in this edge generation method. Firstly, the dummy edges in the adjacency matrix cause a waste of computation. Secondly, the edges generated by the previous rows are not conditioned upon when the edges in the next row are generated. However, it may be beneficial to use the information about the outgoing edges of the previous nodes to enhance the generation accuracy of the outgoing edges of the next node. We will analyze this hypothesis in §4. Hence, we suggest flattening the lower-triangle of the adjacency matrix. We remove the dummy edges and concatenate the rows of the lower triangular matrix to form a sequence of pairs of nodes for which we need to generate edges (Figure 7). This strategy results in using information about all previously generated edges when a new edge is generated.

# Experiments

Baselines. We consider five baselines for comparison. In "Copy Source" baseline (i), the system copies the source graph to the target graph3 . In the "Text2Text" baseline (ii), we flatten the graph and reconstruct the natural sentence similarly to the modification query. In the "Modified GraphRNN" baseline (iii), we use the breadth-first-search (BFS) based node ordering to flatten the graph4 , and use RNNs as the encoders (You et al., 2018) and a decoder similar to our systems. In the final two baselines, "Graph Transformer" (iv) and "Deep Convolutional Graph Networks" (DCGCN) (v), we use the Graph Transformers (Cai and Lam, 2019) and Deep Convolutional Graph Networks (Guo et al., 2019) to encode the source graph (the decoder is identical to ours).

Our Model Configurations. We report the results of different configurations of our model. The "Fully Connected Transformer" uses dense connections for the graph encoder. This is in contrast to "Sparse Transformer", which uses the connectivity structure of the source graph in self attention (see §3.2.1). The information from the graph and query encoders can be combined by "Concatenation", late fused by "Gating", or early fused by "Cross Attention" (see §3.2.3). The "Adjacency Matrix" style for edge decoding can be replaced with "Flat-Edge" generation (see §3.2.5).

Evaluation Metrics. We use two automatic metrics for the evaluation. Firstly, we calculate the precision/recall/F1-score of the generated nodes and edges. Secondly, we use the strict-match accuracy, which requires the generated graph to be identical to the target graph for a correct prediction.

Data Splits. We partition the synthetic MSCOCO data into 196K/2K/2K for training/dev/test, and GCC data into 400K/7K/7K for training/dev/test. We randomly split the crowdsourced user-generated data into 30K/1K/1K for training/dev/test.

## Experimental Results

Table 1 reports the results of our model and the baselines on the synthetic and user-generated datasets. From the experimental results, various configurations of our model are superior to the baselines by a significant margin. Noticeably, DCGCN and graph transformer are strong baselines, delivering SOTA performance across tasks such as AMRto-text generation and syntax-based neural machine translation (Guo et al., 2019;Cai and Lam, 2019). We believe the larger number of edge types in our task impairs their capability.

We ablate the different components of the proposed methods to appraise their effectiveness (c.f., the bottom pane of table 1). First, our hypothesis about the preference of flat-edge generation over adjacency matrix-style edge generation is confirmed. Furthermore, the two-way communication between the graph and query encoders through the gating mechanism consistently outperforms a simple concatenation in terms of both edge-level and node-level generation. Eventually the crossattention -the early fusion mechanism, leads to substantial improvement in all metrics.

We also observe that generating the graphs for the crowdsourced data is much harder than the synthetic data, which we believe is caused by diversity in semantics and expressions introduced by the annotators. Consequently, all models suffer from performance degradation.

Nevertheless, the performance trends of different Table 3: Graph-level accuracy on two multiple operations (MOPs) datasets, one with an average of 1.44 operations per query, the other with 2.01 configurations of our model are almost identical on the user-generated and synthetic data. Finally, Table 2 indicates with the increase of the complexity of graphs, the models have a difficulty in inferring the relations among nodes for GCC data, which causes a dramatic drop in terms of the edge F1 score and graph accuracy.

## Multi-Operation Performance

To study the multiple operations scenario, we create two datasets5 where the average number of the operations are 1.44 and 2.01. For each dataset, we train the baselines and our methods on the full training set. The test set is grouped into four bins according to the number of operations.

According to Table 3, all models demonstrate sharp decreases in performance with the increase of the number of operations. Our model still performs significantly better than the baselines. Having said that, for more than 1-2 operations, all models do not perform satisfactorily, prompting further research.

## Quantitative Analysis

The best configuration of our model is based on cross-attention, with flat-edge decoder, and sparse transformer. We investigate which cases this configuration outperforms the baselines. As seen in Figure 8, cross-attention is able to understand the pronoun and correctly removes the connected object and its associated relation as evidenced by the first example A. In addition, example B demonstrates when graph transformer observes a longer description, it lacks the capability of fusing the semantics between the source graph and the modification query; then certain nodes from the source graph are not preserved. We believe that the proposed approach can reduce the noise in graph generation, and retain fine-grained details better than the baselines.

# Related Work

Semantic parsing is a sequence-to-graph transduction task, mapping natural language sentences to their meaning representation, e.g. see (Buys and Blunsom, 2017;Iyer et al., 2017;Dong and Lapata, 2018); this is different from our graph conditional semantic parsing. Recently, context-dependent semantic parsing has gained attraction (Iyyer et al., 2017;Srivastava et al., 2017;Suhr et al., 2018;He et al., 2019). Our work focuses on the update of scene graphs based on users' queries, while previous works model the modifications of semantic representations in multi-turn dialogue systems. Due to their effectiveness, GCNs and graph transformer have been used as graph encoder for graphto-sequence transduction in semantic-based text generation (Bastings et al., 2017;Beck et al., 2018;Guo et al., 2019;Cai and Lam, 2019;Song et al., 2018;Wu et al., 2020).

# Conclusion

In this paper, we explore a novel problem of conditional graph modification, in which a system needs to modify a source graph according to a modification command. Our best system, which is based on graph-conditioned transformers and cross-attention information fusion, outperforms strong baselines adapted from machine translations and graph generations. The code and datasets will be released to encourage further research in this direction.

# A Training Details

Our encoder is comprised of 3 stacked sparse transformers, with 4 heads at each layer. The embedding size is 256, and the inner-layer of feed-forward networks has a dimension of 512. Both node-level and edge-level decoders are one-layer GRU-RNN with a hidden size of 256, and the size of embeddings are 256 as well. We train 30 epochs and 300 epochs for synthetic and user-generated data respectively, with batch size of 256. We evaluate the model over the dev set every epoch, and choose the checkpoint with the best graph accuracy for the inference. We run all experiments on a single Nvidia Tesla V100.

Table 4 shows that our cross-attention model is more efficient than other models in terms of GPU computing time.  

# B Performance on validation set

Table 6 shows the performance on the validation/dev set of our models and the best baseline.

In general, there is no significant difference between the performance trend in the dev set and the test set.

# C Data Statistics

As shown in Table 7, the graph size distributions of source graphs and target graphs are almost identical among the sets. With the increase in text description length, the source graphs become more complicated accordingly. According to Figure 9, the length of search queries are likely to be less than 5 tokens. Thus, in a real application, it is unlikely to encounter large graphs (>3 nodes) and long modification queries.

We plot the distributions of the number of nodes and edges on synthetic and user-generated data in Figure 10. 

# D Data Creation for Multi-operation Graph Modification

We develop a procedure to create data for the multioperation graph modification (MGM) task. First of all, we assume that MGM requires at least one operation on the source graph. Then we use four actions (terminate, ins, del, sub) paired with a heuristic algorithm to further perform operations on the modified graph. We sample an action, and execute it on the last modified graph until the terminate is sampled or we exhaust the available nodes. Intuitively, a large graph can support more modifications, while a smaller graph does not have too much freedom. In addition, we also assume that the modified nodes should not be changed again. Hence, the probability of terminate should be increased as the edit sequence gets longer, whereas the probabilities of other actions should drop. The heuristic algorithm is summarized in Algorithm 1. It is worth noting that Algorithm 1 gives us a dataset with different edit sequence lengths.

# E Mixing Synthetic and User-generated Data

Getting annotation from users is expensive, especially for a complex task like our graph modification problem. Thus, we explore the possibility of augmenting the user-generated data with synthetic data in order to train a better model. However, one needs to be careful with data augmentation using synthetic data as it inevitably has a different distribution. This is evident when we test the model trained using the synthetic data on the usergenerated data. The graph generation accuracy drops to around 20%, and adding more synthetic data does not help. To efficiently mix the data distributions, we up-sample the user-generated data Table 8 reports the results. It shows that data augmentation with up-sampling is a very effective method to leverage both sources of data, compared to transfer learning. Also, as the size of the synthetic data increases, our proposed scheme further improves the performance to a certain point where it plateaus. More specifically, the performance reaches plateau after injecting 90k instances (the data ratio of 3:1). Both up-sampling and pretraining lead to better models compared to using only synthetic or user-generated data. The graph accuracy for model trained only on user-generated data is 60.90% (see the best result from Table 1 in the main paper).

# F Templates

In Table 9, we summarize the templates used for our synthetic data.  

# G Examples from User-generated Dataset

We provides some examples of our user-generated dataset in Figure 11. A ← {terminate, ins, del, sub}, else if a == ins then 26:

s, q , t ← insertion(s, t, I)

27:

else if a == del then 28:

s, q , t ← deletion(s, t, D)

29: else 30:

s, q , t ← substitution(s, t, S)

31:

end if 32:

end while 33:

q ← concat(q, q )  

# H Alignments between Different Components

Figure 13 and Figure 14 provide the alignments between different components of our cross attention model. Indeed, the cross attention is capable of aligning the source graph to the modification query.   

# Acknowledgments

We would like to thank anonymous reviewers and all voluntary scorers for their valuable feedback and suggestions. The computational resources of this work are supported by Adobe and the Multimodal Australian ScienceS Imaging and Visualisation Environment (MASSIVE) (www.massive. org.au). This work is partially supported by an ARC Future Fellowship (FT190100039) to G. H.

