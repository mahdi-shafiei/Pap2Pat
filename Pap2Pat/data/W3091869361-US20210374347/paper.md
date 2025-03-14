# Introduction

Named entity recognition (NER) aims at identifying and categorizing spans of text into a closed set of classes, such as people, organizations, and locations. As a core language understanding task, NER is widely adopted in several domains, such as news (Tjong Kim Sang and De Meulder, 2003), medical (Stubbs and Uzuner, 2015), and social media (Derczynski et al., 2017). However, one of the primary challenges in adapting NER to new domains is the mismatch between the different domain-specific entity types. For example, only two out of the twenty-three entity types annotated in the I2B2 2014 (Stubbs and Uzuner, 2015) data can be found in the OntoNotes 5 (Weischedel et al., 2013) annotations. Unfortunately, obtaining NER annotations for a novel domain can be quite expensive, often requiring domain knowledge.

Few-shot classification (Vinyals et al., 2016;Bao et al., 2020) models aim at recognizing new classes based on only few labeled examples (support set) from each class. In the context of NER, these fewshot classification methods can enable rapid building of NER systems for a new domain by labeling only a few examples per entity class. Several previous studies (Fritzler et al., 2019;Hou et al., 2020) propose using prototypical networks (Snell et al., 2017), a popular few-shot classification algorithm, to address the few-shot NER problem. However, these approaches only achieve 10 ∼ 30% F1 scores on average, when transferring knowledge between different NER datasets with one or five shot examples, warranting more effective methods for the problem.

The direct adaption of existing few-shot classification methods to few-shot NER is challenging for two reasons. First, NER is essentially a structured learning problem. It is crucial to model label dependencies as shown in Lample et al. (2016) instead of directly classifying each token independently using the existing few-shot classification approaches. Second, few-shot classification models (Snell et al., 2017) typically learn to represent each semantic class by a prototype based on the labeled examples in its support set. However, for NER, unlike the entity classes, the Outside (O) class does not represent any unified semantic meaning. In fact, tokens labeled with O in a dataset actually correspond to different semantic spaces that should be separately represented in a metric-based learning framework. Consider, for example, in Fig. 1, semantic classes such as professions (e.g., 'minister') and dates (e.g., 'today') may also belong to the O class for some NER datasets. Thus, previous approaches end up learning a noisy prototype for representing O class in this low-resource setting.

In this paper, we propose a simple, yet effective method STRUCTSHOT for few-shot NER. Instead of learning a prototype for each entity class, we represent each token in the labeled examples of the support set by its contextual representation in the sentence. We learn these contextual representations from training a standard supervised NER model (Lample et al., 2016;Devlin et al., 2019), on the source domain. Whereas meta-learning approaches (Snell et al., 2017;Vinyals et al., 2016) simulate few-shot evaluation setup during training, our approach does not need to do so. This makes it possible to deploy a unified NER system supporting both classical and emerging types of entities, without the overhead of maintaining a separate few-shot system. During evaluation, STRUCTSHOT uses a nearest neighbor (NN) classifier and a Viterbi decoder for prediction. As shown in Fig. 1, for each token ("president") in the target example, the NN classifier finds its nearest token ("minister") from the support examples, instead of relying on an erroneous class-level (Outside) prototypical representation. We also improve our nearest neighbor predictions by using Viterbi decoder (Forney, 1973) to capture label dependencies.

We perform extensive in-domain and out-ofdomain experiments for this problem. We test our systems on both identifying new types of entities in the source domain as well as identifying new types of entities in various target domains in one-shot and five-shot settings. In addition to the previous evaluation setup followed by Hou et al. (2020), we propose a more standard and reproducible evaluation setup for few-shot NER by using standard test sets and development sets from benchmark datasets of several domains. In particular, we sample support sets from the standard development set and evaluate our models on the standard test set. For all our experiments, we find that our proposed systems outperform previous meta-learning systems by 6% to 16% absolute F1 score.

# Problem Statement and Setup

In this section, we formalize the task of few-shot NER and propose a standard evaluation setup to fa-cilitate meaningful comparison of results for future research.

## Few-shot NER

NER is a sequence labeling task, where each token in a sentence is either labeled as part of an entity class (e.g., Person, Location, and Organization) or O class if it does not belong to an entity. In practice, tagging schemes such as BIO or IO are adopted to represent if a token is at the beginning (B-X) or inside (I-X) of an entity X. Few-shot NER focuses on a specific NER setting where a system is trained on annotations of one or more source domains {D Formally, the task of K-shot NER is defined as follows: given an input sentence x = {x t } T t=1 and a K-shot support set for the target tag set C T , find the best tag sequence y = {y t } T 1 for x. The Kshot support set contains K entity examples (not tokens) for each entity class given by C T .

## A standard evaluation setup

Prior work (Fritzler et al., 2019;Hou et al., 2020) on few-shot NER followed few-shot classification literature and adopted the episode evaluation methodology. Specifically, a NER system is evaluated with respect to multiple evaluation episodes. An episode includes a sampled K-shot support set of labeled examples and a few sampled K-shot test sets. In addition to these prior practices, we propose a more realistic evaluation setting by sampling only the support sets and testing the model on the standard test sets from NER benchmarks.

Test set construction In the episode evaluation setting, test sets are sampled such that the different entity classs are equally distributed. This evaluation setup clearly does not account for the entity distributions in the real data.1 As a result, the reported performance scores do not reflect the effectiveness of these models when adapting to a new domain. We propose to use the original test sets of the standard NER datasets to evaluate the performance of our models. Our evaluation setup does not need to randomly sample test sets, thus, improving its reproducibility for future research.

# Support set construction

In order to test our models in the few-shot setting, we sample support sets from the standard development set of the benchmark dataset. We account for the variance of our model performance by sampling multiple support sets and reporting the average performance on the test set for these sampled support sets. We plan to release the different support sets used for evaluation in our experiments for reproducibility.

Unlike classification tasks, a sentence in NER may contain multiple entity classes. Thus, simply sampling K sentences for each entity class will result in many more entities of frequent classes than those of less frequent classes, as sampling entities of infrequent classes is more likely to also bring in entities of frequent classes than the other way around. Because of this, we utilize a greedy sampling strategy to build support sets as shown in Alg. 1. In particular, we sample sentences for entity classes in an increasing order with respect to their frequencies.

# Model

In this section, we present our few-shot NER algorithm based on structured nearest neighbor learning (STRUCTSHOT). Our method uses a NER model (Lample et al., 2016;Devlin et al., 2019) trained on the source domain, as a token embedder to generate contextual representations for all tokens. At inference, these static representations are simply used for nearest neighbor token classification. We also use a Viterbi decoder to capture label dependencies by leveraging tag transitions estimated from the source domain.

## Nearest neighbor classification for few-shot NER

The backbone of STRUCTSHOT is a simple token-level nearest neighbor classification system (NNShot). At inference, given a test example x = {x t } T 1 and a K-shot entity support set S = {(x

n=1 comprising of N sentences, NNShot employs a token embedder f θ (x) = x to obtain contextual representations for all tokens in their respective sentences. NNShot simply computes a similarity score between a token x in the test example and all tokens {x } in the support set. It assigns the token x a tag c corresponding to the most similar token in the support set:

(1)

where S c is the set of support tokens whose tags are c. In this work, we use the squared Euclidean distance, d(x, x ) = ||x -x ||2 2 for computing similarities between tokens in the nearest neighbor classification. We also perform L2-normalization on the features before computing these distances.

# Pre-trained NER models as token embedders

Most meta-learning approaches (Snell et al., 2017;Hou et al., 2020) simulate the test time setup during training. Hence, these approaches sample multiple support sets and test sets from the training data and learn representations to minimize their corresponding few-shot loss on the source domain. In this paper, we instead use a NER model trained on the source domain to learn token-level representations that minimizes the supervised cross-entropy loss. Supervised NER models typically consist of a token embedder f θ (•) followed by a linear classifier W ∈ R D×L where D is the token embedding size and L represents the number of tags.

We consider two popular neural architectures for our supervised NER model: a BiLSTM NER model (Lample et al., 2016) and a BERT-based NER model (Devlin et al., 2019). 2 For training these models on the source domain, we follow the setting from their original papers. These models are trained to minimize the cross-entropy loss (Wf θ (x), y) on the training data in the source domain. 3 At inference time, NNShot uses the BiL-Figure 2: A depiction of the extension of an abstract transition matrix. An abstract transition probability is evenly split into related target transitions, which is illustrated using the cells of the same color in their corresponding rows of the two matrices.

STM and Transformer encoders just before the final linear classification layers as token embedders.

## Structured nearest neighbor learning

Conditional random field (CRF) (Lafferty et al., 2001) is the de facto method to model label dependencies for NER. Lample et al. (2016) use BiL-STM embedder followed by a classification layer to represent token-tag emission scores and learn tag-tag transition scores by joint training a CRF layer. Adopting a similar method is challenging in the context of few-shot learning. The mismatch between the tags in the source domain and the target domain does not allow learning tag-tag transition scores of the target domain by only training on the source domain.

STRUCTSHOT addresses this challenge by using an abstract tag transition distribution estimated on the source domain data. Additionally, STRUCT-SHOT discards training phase in CRF and only makes use of its Viterbi decoder during inference. In particular, similar to Hou et al. (2020), we utilize a transition matrix that captures transition probabilities between three abstract NER tags: O, I, I-Other4 . For instance, p(O|I) and p(I|O) correspond to the transition probabilities between an entity tag and O, whereas p(I|I) and p(I-Other|I) correspond to the probabilities of transitioning from an entity tag to itself and to a different entity tag respectively. As depicted in Fig. 2, we can extend these abstract transition probabilities to an arbitrary target domain tag set by evenly distributing the abstract transition probabilities into corresponding target transitions. Our simple extension method guarantees that the resulting target transition probabilities still lead to a valid distribution. Hou et al. (2020) copy these abstract tran-sition scores to multiple specific transitions such that the resulting target transition probabilities no longer correspond to a distribution.

The key idea in STRUCTSHOT is that it estimates the abstract transition probabilities by counting the number of times a particular transition was observed in the training data. The transition probability from X to Y is

where N (X → Y) and N (• → Y) are the frequencies of the transition from X to Y and the transition from any tag to Y respectively. In practice, these abstract transitions can also be drawn from a prior distribution given domain knowledge.

For Viterbi inference, we obtain the emission probabilities p(y = c|x) for each token in the test example from NNShot. x) .

(3)

Given this abstract transition distribution p(y |y) and the emission distribution p(y|x), we use Viterbi decoder to solve the following the structured inference problem:

p(y t |x) × p(y t |y t-1 ). (4)

As the emission and transition probabilities are estimated independently, we introduce a temperature hyper-parameter τ that re-normalizes the transition probabilities to align the emission and transition scores to a similar scale.

# Experiments

In this section, we compare STRUCTSHOT against existing methods on two few-shot NER scenarios: tag set extension and domain transfer. We adopt several benchmark NER corpora in different domains for the few-shot experiments.5 

## Data

We experiment with standard NER datasets in four important domains: OntoNotes 5.0 (Weischedel et al., 2013) 

## Evaluation tasks

We evaluate few-shot NER systems on two real world scenarios. For both scenarios, we experiment with both one-shot and five-shot settings.

Tag set extension Our first set of experiments are motivated by the fact that new types of entities often emerge in some domains such as medical and social media. Thus, we evaluate the performance of our systems on recognizing new entity types as they emerge in the source domain. We mimic this scenario by splitting the entity classes of a dataset into a source set and a target set. Specifically, we randomly split the eighteen entity classes of the OntoNotes dataset into three target entity class sets: 

## Experimental settings

We have provided details of our proposed evaluation setup for few-shot NER in § 2. We report the standard evaluation metrics for NER: micro averaged F1 score. For each experiment, we sample five support sets and report the mean and standard deviation of the corresponding F1 scores. In order to establish a comprehensive comparison with prior work, we also report episode evaluation results in the Appendix.

Competitive systems We consider five competitive approaches in our experiments. We build BERT-based systems for all the methods and BiLSTM-based systems for three of them. Prototypical Network (Snell et al., 2017) is a popular few-shot classification algorithm that has been adopted in most state-of-the-art (SOTA) few-shot NER systems (Fritzler et al., 2019). Prototypical-Net+P&D (Hou et al., 2020) improves upon Prototypical Network by using the pair-wise embedding and dependency transfer mechanism.7 Sim-BERT is a nearest neighbor classifier based on the pre-trained BERT encoder without fine-tuning on any NER data. Finally, we include our proposed NNShot and STRUCTSHOT described in § 3. We use the IO tagging scheme for all of the experiments, as we find that it performs much better than BIO scheme for all the considered methods.

Parameter tuning We adopt the best hyperparameter values reported by (Yang et al., 2018) for the BiLSTM-NER models and use the default We use the pre-trained 100-dimensional GloVe vectors (Pennington et al., 2014) to initialize the word embeddings for all BiLSTM-NER models. SGD and Adam (Kingma and Ba, 2014) are utilized to optimize the BiLSTM-based and BERTbased models with learning rates 0.015 and 5 × 10 -5 respectively. We tune other parameters required by different few-shot learning methods on the source domain development sets. The transition re-normalizing temperature τ is chosen from {0.01, 0.005, 0.001}.

## Results

The results for one-shot NER and five-shot NER are summarized in Table 2 and Table 3 respectively.

As shown, our NNShot and STRUCTSHOT perform significantly better than all previous methods across all evaluation settings. By modeling label dependencies with a simple Viterbi decoder, STRUCT-SHOT boosts the performance of NNShot by 2.4% and 4% F1 scores on five-shot tag set extension and domain transfer tasks on average respectively. These performance gains are greater than the ones obtained by joint CRF training with the prototypical network (PrototypicalNet+P&D), suggesting that independently modeling transition and emission scores is a cheap but effective way to capture label dependencies. STRUCTSHOT achieves new SOTA results on the two few-shot NER tasks, outperforming the previous SOTA system (Prototypi-calNet+P&D) by 6% to 9% F1 score on one-shot setting and 11% to 16% F1 score on five-shot setting.

# BiLSTM vs. BERT as token embedder

The BERT-based systems considerably outperform BiLSTM-based systems on few-shot NER. Language model pre-training is critical for low-resource natural language processing including few-show transfer learning (Cherry et al., 2019). However, task-specific knowledge is usually more important than the general information learned via unsupervised training. For example, the topperforming BiLSTM-based systems can beat Sim-BERT by up to 15% F1 score on some few-shot NER settings. With fine-tuning on the OntoNotes data, NNShot outperforms SimBERT by 20% to 35% F1 scores across different settings, demonstrating the effectiveness of injecting task-specific information into pre-trained language models.

Tag set extension vs. Domain transfer The one-shot NER systems generally perform better on domain transfer than on tag set extension, while the five-shot systems work better on the tag set extension task. On the domain transfer task, the source entity classes overlap with some entity classes in the target domain, which benefits NER systems built under the extremely low-resource condition. However, in general, domain transfer is more challenging than tag set extension due to language variation across different domains. Not surprisingly, our five-shot NER systems are not only more accurate but also more robust than the one-shot systems.

The standard deviations reported with multiple fiveshot support sets are much lower than those obtained with one-shot support sets. This indicates that we can build more reliable few-shot NER systems given more few-shot examples in the support sets.

Episode evaluation Finally, as shown in the Appendix, the results obtained on episode evaluation are generally better than the ones reported with our proposed evaluation setup. However, the performance trend is the same, i.e., STRUCTSHOT significantly outperforms all competitors. It implies that previous studies (Fritzler et al., 2019;Hou et al., 2020) overestimate the performance of their fewshot NER systems. 

# Few-shot NER in practice

## Analysis

We perform analysis to investigate the impact of various tagging schemes and BERT fine-tuning objectives on few-shot NER.

Tagging scheme When only a few entities are available in the support sets, the conventional BIO tagging scheme can harm the performance of fewshot NER systems, as it further reduces the number of labeled instances per tag class. We experiment with both BIO and IO tagging schemes for all the few-shot NER models. The systems equipped with IO tagging scheme always outperform those with BIO scheme. In particular, STRUCTSHOT and NNShot benefit from switching from BIO scheme to IO scheme by an average of 3.2% and 3.8% F1 scores on the five-shot tag set extension and domain transfer tasks respectively.

Fine-tuning objective STRUCTSHOT exploits the standard cross-entropy loss for NER used in the original paper (Devlin et al., 2019) to fine-tune BERT on OntoNotes data. We also experiment with fine-tuning BERT using the prototypical network objective, and then utilize the encoder in STRUCT-SHOT. The results show that BERT fine-tuned with the standard NER loss performs much better than the one fine-tuned with the prototypical network loss by 12% and 9% on the five-shot tag set extension and domain transfer tasks respectively. This suggests that the popular meta-learning methods fall short in capturing effective representations for few-shot NER task.

# Discussion

In this section, we investigate two questions: 1) why STRUCTSHOT is so effective? and 2) why few-shot NER is so difficult?

# t-SNE visualization

We project token-level representations obtained from the BERT embedders onto a 2-dimensional space using t-SNE (Maaten and Hinton, 2008). Fig. 3 presents the visualization results on the CoNLL and WNUT test sets (we exclude I2B2 as it includes too many classes for visualization). Fine-tuning BERT on OntoNotes clearly improves the task-awareness with respect to both    Vinyals et al. (2016) compute support set aware similarities between a test point and the target classes. These methods have been adapted with some success to NLP tasks including text classification (Yu et al., 2018;Geng et al., 2019;Bao et al., 2020), machine translation (Gu et al., 2018), and relation classification (Han et al., 2018). Recently, Wang et al. (2019) show that simple feature transformations followed by nearest neighbor search can perform competitively with the state-of-theart meta-learning methods on standard computer vision classification datasets. Inspired by this approach, we evaluate the performance of nearest neighbor based classification against meta-learning methods.

Few-shot NER A few approaches have been proposed for few-shot NER. 

# Conclusion

We introduce STRUCTSHOT, a simple few-shot NER system that achieves SOTA performance without any few-shot specific training. We identify two weaknesses of previous systems related to their handling of O class and modeling label dependencies. Our systems overcomes these challenges with nearest neighbor learning and structured decoding. We further propose a standard evaluation setup for few-shot NER and show that STRUCTSHOT significantly outperforms prior SOTA systems on popular benchmarks across multiple domains. In the future, we want to extend our system to other few-shot sequence tagging problems such as part-of-speech tagging and slot filling.

# Acknowledgments

We thank the EMNLP reviewers for their helpful feedback. We also thank the ASAPP NLP team for their support throughout the project.

# A Appendix: Episode Evaluation Results

The main episode evaluation results for one-shot NER and five-shot NER are summarized in Table 5 and Table 6 respectively. We sample 100 evaluation episodes for each experiment. The performance trend is the same as our main results, in which STRUCTSHOT significantly outperforms all competitors. 

