# INTRODUCTION

Deep learning models have been actively used in recent music information retrieval (MIR) research. Although the spirit of deep learning is end-to-end learning, however, various assumptions are made during making design choices of deep learning models.

Regarding assumptions on spectrograms, the most popular form of music audio representation in deep learning, the time-axis is often considered to be the axis of sequence while the frequency-axis is the axis of feature. For example, in [1,2], recurrent layers were applied to model a spectrogram as a sequence of spectra. In [3], convolutional layers were used to aggregate features over time after the first convolutional layer models multiple frames of spectra as feature. On the other hand, in [4], two-dimensional convolutional layers were used, equating the frequencyand time-axes. There are also hybrid approaches, such as convolutional recurrent neural networks (CRNN) [5] and convolutional Transformer [6], in which recurrent layers or Transformer are applied along the time axis.

In spectrograms, it is well known that there are meaningful spectral patterns. Different music components exist in different frequency ranges, and there is a very strong spectral correlation called harmonics. Since a normal convolutional layer can model local patterns only, several approaches have been proposed to model harmonics along the frequency axis. Harmonic Constant-Q Transform (HCQT) is a novel multi-channel time-frequency representation that was proposed to overcome the limitation by improving the input representation of audio [7]. Harmonic CNNs (convolutional neural networks) [8] are designed to model the harmonic pattern by modifying the convolutional filters. However, these solutions only model some of the spectral patterns, reminding the need for a more general solution with higher flexibility.

Transformers have successfully demonstrated their ability to model the sequential data with long-term (inter-) dependency and invariance. This is achieved by multiple aspects of Transformers. First, the key-query mechanism enables modeling the relationship of every combination of the instances. Second, positional encoding helps the model to take the order of instances into account. Through stacked attention layers, the input sequence is transformed into a sequence of representations that are based on the inter-dependency of the input. The prior works in audio analysis are mostly based on a similar, naive approach where Transformer is used as a temporal feature aggregator that acts similar to RNNs (recurrent neural networks), with few exceptions such as [9].

Recently, Transformer in Transformer (TNT), a variant of Transformer that arranges two Transformers in a hierarchical manner, was proposed [10] for image recognition. In TNT, an inner (lower-level) Transformer is applied to extract the local pixel-level embeddings, and then the pixel-level embeddings are projected to the patch-level embedding space which is later handled by an outer (higherlevel) Transformer to summarize a global representation. One can simply apply TNT for audio by treating a song as an image, which is comprised of a sequence of frames (patches) while the frequency bins within frames are con-sidered as pixels. However, from our pilot study, we find this approach results in unstable training and only achieves similar performance compared to using the original Transformer. This is possibly because the amount of training data is insufficient or the interpretation of frequency sequences is different from that of pixel sequences. Therefore, non-trivial modifications from the original idea of TNT should be made.

In this paper, we propose SpecTNT, a time-frequency transformer that models spectrograms as a sequence along both time-and frequency-axes. Similar to TNT, SpecTNT uses two Transformers hierarchically. However, the temporal local embeddings extracted from the inner Transformer are not directly sent to the outer Transformer. Instead, a special token called frequency class token (FCT) is appended to aggregate the important spectral features of each frame. The FCT is then projected to the global (temporal) embedding space to enable the information exchange across the time axis. This design allows the important local information is passed to the outer Transformer through FCT while reducing the dimensionality of the data flow compared to the original TNT. As a result, it helps SpecTNTs to perform well on audio-related tasks even with smaller datasets.

Our contributions can be summarized as follows: (1) to the best of our knowledge, our work is the first attempt to leverage TNT-based architecture to learn the representations for audio; (2) we propose SpecTNT, a novel modification of TNT to better fit the music data for MIR tasks; (3) we conduct extensive experiments to demonstrate the capability of SpecTNT in various MIR tasks -vocal melody extraction, music auto-tagging, and chord recognition.

# RELATED WORK

In this section, we review the literature in music tagging, vocal melody extraction, and chord recognition -three well-defined MIR tasks adopted in the experiments to evaluate SpecTNT. Due to space limitation, we focus on the recent trends since the adoption of deep learning approaches.

Music tagging is a multi-label classification task that annotates a music audio clip with various types of labels such as genres (rock, jazz), instruments (vocal, guitar, drums), and mood (happy, sad) [11]. Since a CNN-based approach has been first introduced [3], various advanced architectures have been used including a two-dimensional CNN [4], a sample-level CNN [12], and a two-dimensional ResNet [13]. Due to the open nature of the tag set, among MIR tasks, music tagging is relatively a vague task -The exact mechanism of annotating tags is not fully known. This aspect suits well for the fundamental motivation of deep learning, which is, to reduce inductive bias and let the data speak [14].

The goal of vocal melody extraction is to estimate the F0 frequency of the (dominant) vocal track in given mixtures. Various deep learning methods have been adopted: a fully-connected neural network with Hidden Markov Model [15], a bidirectional long short-term memory network [1], a CNN [7], encoder-decoder networks [16,17] Figure 1. The block diagram of the whole SpecTNT. The details of positional encoding and SpecTNT module are illustrated in Figure 2 and3, respectively. and a CRNN [18]. Recently, a frequency-temporal attention module was introduced in [19] to learn the relevant regions for predictions. Some special representations are proposed including HCQT [7], a combination of frequency and periodicity [20], and source-separated tracks [21,22].

Chord recognition is a MIR task to "produce a timevarying symbolic representation of the signal in terms of chord labels" [23]. Compared to music tagging, we clearly understand how chords of music signals can be decided -They are based on the combination of the present musical notes. Therefore, models have been designed to take advantage of note representations such as constant-Q transform (CQT) or chromagram. The early deep learningbased chord recognition models are based on a RNN [24] and a CNN [25]. Later, a CRNN has been used in [23] to combine the merits of RNNs and CNNs. More recently, (bi-directional) Transformer was used, achieving state-ofthe-art performance [26,27].

# METHODS

As illustrated in Figure 1, the proposed SpecTNT architecture consists of a convolutional module, positional encoding, SpecTNT module, and output module.

The input time-frequency representation is first processed with a stack of convolutional layers for local feature aggregation. Then, the positional information is added to the data. In the SpecTNT module, the intermediate representation is fed into a stack of SpecTNT blocks. Lastly, the output module projects the final embedding into the desired dimension for different tasks. We detail each module in the following subsections.

## Convolutional module

The purpose of this convolutional module is to employ different strategies for generating intermediate representations with pooling or striding convolution techniques depending on the nature of the task. Let the input timefrequency representation be S ∈ R T ×F ×K where T is the number of time-steps, F is the number of frequency bins, and K is the number of channels. S is first passed into a stack of convolutional layers. We utilize the residual unit proposed in [28] to be the basic building block of the convolutional module. The representation after the convolutional module is denoted as S = [S 1 , S 2 , ..., S T ] ∈ R T × F × K , where F , T , and K are the numbers of frequency bins, time-steps, and channels, respectively. 

## Frequency Class Token

As depicted in Figure 2, Frequency class token (FCT) is an embedding vector initialized with all zeros to serve as the placeholder and defined as c t = 0 1× K . Let S t ∈ R F × K denote the input data at each time-step t. The input data and FCT are concatenated as following:

Here, the role of FCT c t is similar to the classification token [29]. It is expected to extract spectral features from each frequency bin of the t-th frame during the spectral self-attention in the later stages.

## Positional encoding

In the original Transformer paper, a sinusoidal positional encoding was added to the input sequence to make the following layers aware of the order of input elements [30].

From a similar motivation, we adopt a learnable positional embedding to encode the sequence order of frequency bins. We encode the positional information of frequencies by adding the frequency positional embedding (FPE) to the data S . FPE is a learnable matrix E φ ∈ R ( F +1)× K . The addition process is done at each time-step t:

where ⊕ is the element-wise addition, and the resulting FCTs are denoted by Ĉ = [ĉ 1 , ĉ2 , ..., ĉ T ]. Then, the resulting representation Ŝt is able to carry information about pitch and timbre to the following attention layers. For example, a pitch in the signal can lead to high energy at a specific frequency bin, and the positional embedding makes FCT aware of the position of that frequency

## Transformer in Transformer (TNT)

Inspired by the architecture in [10], we design a SpecTNT block to handle audio data, as depicted in Figure 3. The SpecTNT block holds two data flows: spectral embedding (SE) and temporal embedding (TE). The two data flows are respectively processed with two Transformer encoders, Because the SpecTNT block is repeated multiple times in the SpecTNT module, we introduce a notation l to specify the layer index for both SE and TE.

In the following sections, we explain each component of a SpecTNT block (Section 3.4.1 through Section 3.4.3) and the entire procedure (Section 3.4.4).

### Temporal Embedding

In the proposed model, we introduce the temporal embedding (TE) to distribute the information of FCTs across the time axis. We can write the TE at layer l as:

where e l t ∈ R 1×D is a TE vector at time t and D is the number of features. In practice, TE is a learnable matrix and is initialized randomly as E 0 ∈ R T ×D prior to entering the first SpecTNT block.

There are two bridges between the spectral and temporal data flows. We use FCTs, the first frequency bin of SEs, for this communication. First, TE sends information to FCTs by passing e l t to a linear projection layer. Then, the projected D-dimensional vectors are added to FCTs (Figure 3-(a)). Second, after spectral transformer encoder (Figure 3-(c)), FCTs (purple arrays) are projected back to K-dimension (Figure 3-(d)). Note that TE also has a skipconnection (Figure 3-(b)).

### Spectral Embedding

The output from the positional encoding, Ŝ, will serve as an input SE for the first SpecTNT block and is denoted as Ŝ0 . As mentioned above, SE includes FCTs, which help aggregate useful spectral information from the local. As a general notation, we write the data flow of SE as:

where l = 0, 1, ..., L, and c l t and Ŝl t are respectively the FCTs of l-th layer and spectral data at time-step t. Then, SE can interact with the TE through FCTs, so the local spectral features can be processed in a temporal and global manner.

### Transformer Encoder

A Transformer encoder is composed of three components: multi-head self-attention (MHSA), feed-forward network (FFN), and layer normalization (LN).

Self-attention (SA) [30] plays the pivotal role in a Transformer encoder. It takes three inputs: Q ∈ R T ×dq , K ∈ R T ×d k and V ∈ R T ×dv which represent the queries, keys, and values, respectively. T is the number of timesteps, and d q , d k and d v indicate the dimension of features for Q, K, and V , respectively. The output is the weighted sum over the values based on the dot product similarity between queries and keys at the corresponding time-step.

The MHSA module [30] is an extension of SA. It splits the three inputs Q, K and V along their feature dimension into h number of "heads" and performs multiple SA's, each on a head, in parallel. The outputs of heads are then concatenated and linearly projected into the final output. The FFN module has two linear layers with a GELU activation function in the middle. We also adopt the pre-norm residual units [31] to stabilize the training.

With the three components, the Transformer encoder (either spectral or temporal) is built and denoted by

where the operations within it can be written as

### Stacking SpecTNT Blocks

We stack three SpecTNT blocks for the SpecTNT module. The module starts with inputting the initial SE, Ŝ0 , and the initial TE, E 0 , to the first SpecTNT block. For a SpecTNT block, there are four steps. First, each FCT vector in Ŝl-1 is updated by adding the linear projection of the associated TE vector (Figure 3-(a)):

where Linear(•) is a shared linear layer. Second, the SE Sl-1 (with the updated FCTs [c l-1 t ] T t=1 at the first row) is passed through the spectral Transformer (Figure 3-(c)):

Ŝl = SpecEnc( Sl-1 ).

Third, each FCT vector in Ŝl is linearly projected and added back to the corresponding TE vector (Figure 3-(d)):

Finally, we propose to encode only the updated TE (i.e., Ẽl-1 = [ẽ l-1 t ] T t=1 ), instead of TE + SE, with the temporal Transformer:

This operation builds up the relationship along the time axis and is the key role that leads to better model and data efficiency. We consider the temporal Transformer only needs to see the information of the frequency bins which are attended by the FCT and such design largely reduces the size of the model and also improves the performance on smaller datasets in preliminary experiments. Table 1. Settings of SpecTNT for different tasks, where p f and p t represent the pooling ratio we apply along the frequency and time axis in the convolutional module, k and d are the feature dimension, h k and h d denotes the number of heads for the spectral and temporal transformer encoder respectively, and finally, o d represents the output dimension, T indicates frame-wise predictions.

## Output Module

The output TE of the 3rd SpecTNT block, E 3 , can be used towards the final output. For frame-wise prediction tasks such as vocal melody extraction and chord recognition, we feed each TE vector e 3 t into a shared fully-connected layer with sigmoid or softmax function for final output. For song-level prediction tasks such as music tagging, we initialize a temporal class token l (l = 0) concatenated at the front of E l :

1 , e l 2 , ..., e l T ],

Note that l does not have an associated FCT in SE, but is for aggregating TE vectors along the time axis. Finally, we feed 3 to a fully-connected layer, followed by a sigmoid layer, to get the probability output.

# EXPERIMENTS

In this section, we evaluate SpecTNT on various types of MIR tasks to demonstrate its effectiveness and versatility. We choose three MIR tasks -music tagging, vocal melody extraction, and chord recognition.

## Implementation

SpecTNT is implemented using Pytorch [32]. Due to the difference in dataset sizes and the natures of tasks, we use different hyper-parameters for the tasks as shown in Table 1. All models include dropout with a rate of 0.15 in the Transformers of the TNT modules. We use AdamW [33] as the learning optimizer. The initial learning rates are set to 10 -3 for vocal melody extraction, 5 × 10 -4 for music tagging and chord recognition, and a weight decay of 5 × 10 -3 is set for all the tasks. For the input representation of music tagging, we resample the audio at the 22,050 Hz and use an input length of 4.54 second. Log-magnitude mel-spectrograms are computed with 128 mel filter banks, 1024 samples of Hann window, and a hop size of 512 samples. For vocal melody extraction, input waveforms are re-sampled at the 16,000 Hz sample rate. We take 3-second segments input and their log-magnitude spectrograms are computed with 2048 samples of Hann window and a hop size of 320 samples. For chord recognition, we try two types of input representation. The first input type is 24-dimensional chroma features with a frame rate of 46 ms [34]. Out of the whole track, we use 400 frames as an input. The second input type is CQT, which is computed from a 18.2 second audio at the 22,050 Hz sample rate. The CQT includes six octaves starting from C1 (32.70 Hz) with 24 bins per octave, and is based on a hop size of 2048.

## Ablations Study

To validate the design choices we make, we consider three various models by progressively removing the components of SpecTNT as follows.

A1: Remove the operation of (a) in Figure 1 (i.e., Eq. 7) and initialize the FCTs as learnable vectors.

A2: Neglect the FCTs but use the full spectral embeddings for operations (a) and (c) in Figure 1 (i.e., Eq. 7 and Eq. 9). The resulting model can be seen as using the original TNT block [10].

A3: Remove the data flow of spectral embedding, so the model is reduced to the original Transformer [30] for aggregating the input sequence in a traditional way.

In the following evaluations of different tasks, we will include the results of the three variants for comparison.

# Datasets

Million song dataset (MSD) [35] consists of one million audio previews and a subset of it has crowd-sourced music tags. Typically, a subset with the 50 most frequent tags are used with randomly split train/validation/test sets [4]. However, these tags are noisy and the random split without considering artist overlaps may cause unintended information leakage. Therefore, we take advantage of manually cleaned 50 tags from a previous work [36] and split the dataset based on artist names so that there is no overlapped artists among the training/validation/test sets. As a result, we use 233,147 tracks, of which 70%, 15%, and 15% are allocated for training, validation, and test sets, respectively. During training, we apply random data augmentation to the input waveform following the pipeline introduced in [37].

Baseline Models Two baselines methods are compared. The first is CNNSA [6], which employs a convolutional front-end and a transformer encoder to aggregate the temporal feature. The second baseline [13] uses 7-layer shortchunk CNN with residual connection, followed by a fullyconnect layer for final output. This model has shown stateof-the-art performance in music auto-tagging. We utilize the original implementation of [13] to train the baseline under the same configuration as our proposed model.

Evaluation Metrics Area Under Precision Recall Curve (PR-AUC) and Area Under Receiver Operating Characteristic curve (ROC-AUC) are used.

# Results

The results of music auto-tagging are summarized in Table 2. SpecTNT outperforms prior state-ofthe-art models in both metrics. In the ablation study, A1 performs the worst, while A2 and A3 show similar results to SpecTNT. This can be explained from the perspec- tive of data distribution: the top 50 tags of MSD dataset are mostly related to genre and style, both of which need enough temporal information to characterize. In A1, the process of updating FCTs with TEs is removed and this may interfere the temporal information flow being shared with the spectral data and cause the performance drop. By looking into the precision scores of individual tags where SpecTNT outperforms A3, we observe that instrumental tags such as "piano" and "guitar" can benefit from SpecTNT, because they may require more spectral information to model well. This shows the benefit of adding the spectral transformer. Also, the smaller performance difference among SpecTNT, A2, and A3 indicates that the size of MSD dataset might be enough to support architectures with less prior knowledge. That is, A2 and A3 are able to sufficiently learn from MSD the useful information without further utilizing FCTs to interact with the temporal embeddings.

## Vocal Melody Extraction

Datasets We use two datasets to train the models: MIR1K [38], which includes 1000 Chinese karaoke clips, and a 48-song subset of MedleyDB [39] that includes vocal tracks. Since the training sets are relative small, we adopt a pipeline with four steps of augmentation techniques. There is a chance for each step to be applied to a training sample: i) pitch-shifting by up to ±2 semitones (with 100% chance), ii) replacing the original background track with a randomly selected, different background track (with 50% chance), iii) changing the gain of the vocal within [-4, 2] dB (with 100 % chance), and iv) completely removing the background track (with 10% chance). We choose three test sets for evaluation: ADC2004, MIREX05, and MedleyDB. For ADC2004 and MIREX05, we only use the samples that have melody sung by human voice. This results in 12 samples from ADC2004 and 9 samples from MIREX05. For MedleyDB, we only use the songs that have singing voice included in their "MELODY2" annotations, yielding 12 songs. The groundtruth pitches are obtained from the MELODY2 annotations within the intervals marked as "female singer" or "male singer." These 12 songs are not included in training.

Baseline Models We compare our model with two baseline models. The first baseline is the joint detection and classification model (JDC) [18] based on CRNN. We use the most representative architecture, called "Main" in [18]. The second baseline is the frequency-temporal attention network (FTANet) [19], which is a CNN-based model that employs attention mechanism along the frequency and time axis. We re-implemented JDC and FTANet using Pytorch and used the suggested hyper-parameters in [18,19]. Both models are trained under the same configuration (e.g., data split and augmentation process) as our model.

Evaluation Metrics Overall Accuracy (OA), Raw Pitch Accuracy (RPA), and Voice Recall (VR) are adopted for evaluation. We use mir_eval library [40] to compute the performance values with a tolerance range of 50 cents.

Results Table 3 shows the results for vocal melody extraction. SpecTNT outperforms the baselines by a large margin in terms of RPA and VR. To the best of our knowledge, this is the first attempt to apply Transformers to this task and the results demonstrate its superiority over the CNN and CRNN counterparts. It is worth noting that FTANet is trained with an input representation specifically designed for pitch detection [20], but our model works well with spectrogram input. In addition, A3 shows the largest performance drop, and this demonstrates the usefulness of spectral Transformer when training on smaller data.

# Datasets

We use the Billboard dataset to evaluate SpecTNT for the chord recognition task. The dataset contains 890 pieces selected from the Billboard chart slots [34]. Following [27], duplicates pieces are first removed to leave 739 unique pieces in total. The official release of the dataset only comes with 24-D chroma vectors, which might be insufficient to fully demonstrate the effectiveness of SpecTNT. Therefore, we manually collected the audio files based on the provided meta-data. Due to the potential version mismatch between our audio files and that for official chord annotations, we applied dynamic time warping (DTW) [41] to validate each song. Specifically, we first replicated the chroma features of the official release using Sonic Annotator [42] on our audio files, and then calculated the alignment cost between the two versions of chromagrams for each song using DTW. We selected 462 songs with the lowest alignment costs. The songs with ID's smaller than 1000 are used for training and the remaining for testing. To augment the training data (chroma and audio), we shifted the pitches by up to ±6 semitones. For evaluation, we adopt the "maj/min" label set with 25 classes, where 24  Baseline Models We compare to three baseline models: i) CR2 model from [23], which is a CRNN-based model, ii) a bi-directional Transformer (BTC) [26], and iii) Harmony Transformer (HT) [27]. BTC and HT are known to be the current state-of-the-art models for chord recognition. For CR2 and BTC, we use the official implementations with the suggested default settings for both chromagram and CQT inputs. For HT, we report the chromagrambased results in [27], since the train/test data split and data augmentation are very similar to us. We did not conduct experiments using HT with CQT input because non-trivial modifications are required for the model.

# Evaluation Metrics

The Weighted Chord Symbol Recall (WCSR) score is reported as evaluation metric. WCSR is the percentage of correctly identified frames and can be computed by tc ta × 100(%), where t c is the duration of the correctly predicted segments, and t a is the total duration of the test segments.

Results Table 4 shows the results for chord recognition. For "Chroma" case, the full Billboard dataset is used. For "CQT" case, the 462 songs with audio are used. From the results, SpecTNT can outperform all the baselines except HT (with chromagram input). However, HT may benefit from joint training with an additional segmentation loss, so the comparison could be unfair. Compared to BTC and CR2, SpecTNT achieves better performance for both types of input. For the ablation study, since we used less data for CQT input, A2, which is the largest model, may suffer from over-fitting and thus performs the worst.

# CONCLUSION

We proposed SpecTNT, a novel Transformer architecture that models spectrograms along both the time and frequency axes. The introduction of FCT enables effective communication between the spectral embeddings and temporal embeddings, maximizing the benefit of Transformer encoder for flexible, local, and global modeling. In experiments, SpecTNT has demonstrated state-of-the-art performance in music tagging and vocal melody extraction and shown competitive performance in chord recognition. For future work, we plan to apply SpecTNT to other MIR tasks, such as beat tracking and structure segmentation.

