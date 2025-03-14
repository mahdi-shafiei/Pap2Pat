# DESCRIPTION

## FIELD

- relate DNA data storage

## BACKGROUND

- motivate DNA data storage
- limitations of DNA data storage

## SUMMARY

- introduce method embodiment
- introduce system embodiment
- introduce computer-readable media embodiment
- describe first pass processing
- describe second pass processing
- preview detailed description

## DETAILED DESCRIPTION

### Example 1—Overview

- introduce flexible decoding technologies
- describe error profile of DNA data storage
- discuss concatenated codes
- explain ordering of strands
- describe decoding process
- discuss various approaches to decoding
- highlight limitations of clustering approach
- introduce flexible approach combining solitary-strand and cluster-based approaches

### Example 2—Example Terminology

- define polynucleotides
- explain encoding digital information in DNA
- discuss advantages of DNA data storage
- define DNA strands
- explain interactions between strands
- define complementary and complementarity
- discuss hybridizing
- explain artificial synthesis of DNA
- define primer
- discuss amplification reaction mixture
- explain PCR technique
- discuss PCR thermocycler
- define melting temperature
- discuss primer design
- explain PCR amplification prior to sequencing
- discuss selectively amplified DNA
- explain provision of amplified DNA to DNA sequencer

### Example 3—Example System Implementing Flexible Decoding in DNA Data Storage

- introduce system implementing flexible decoding
- describe encoding digital file
- explain representation of digital file
- discuss nucleotide synthesizer
- describe sequencer
- explain decoder
- discuss functionality of decoder
- highlight goal of encoding/decoding process

### Example 4—Example Method Implementing Flexible Decoding in DNA Data Storage

- encode digital file
- synthesize nucleotide strands
- sequence nucleotide strands
- decode noisy reads using flexible decoding

### Example 5—Example DNA Data Storage Scenarios

- describe binary data representation in DNA
- discuss errors in DNA data storage

### Example 6—Example Digital File

- describe digital file
- explain conversion to quaternary data
- discuss decoding to recover original digital data

### Example 7—Example Nucleotide Symbol Strings

- describe nucleotide symbol strings
- explain logical arrangement of strings
- discuss inclusion of address information

### Example 8—Example Encodings

- introduce encodings
- describe transformational encodings
- explain constrained encodings
- discuss application of constrained encodings

### Example 9—Example Redundancy

- introduce redundancy information
- define inner redundancy
- define outer redundancy
- describe error correction using inner redundancy
- describe error correction using outer redundancy
- motivate use of redundancy in DNA data storage
- describe Reed Solomon code
- describe LT code
- describe LDPC code
- describe Hamming code
- describe cyclical redundancy check (CRC)
- describe checksum
- describe file integrity digests
- describe correction of substitution errors
- describe correction of insertion/deletion errors
- motivate use of redundancy in nanopore sequencing
- describe flexible decoding system
- introduce decoder system 300
- describe noisy reads
- describe address region
- describe underlying data region
- describe redundancy region
- describe solitary-strand-based selection
- describe cluster-based trace reconstruction
- describe conflict resolution processing
- describe additional decoding
- describe decompression
- introduce method 400 of flexible decoding
- receive nucleotide symbol strings
- perform solitary-strand-based processing
- perform cluster-based trace reconstruction
- output ordered string map
- describe amplification
- introduce string position map
- describe mapping nucleotide symbol strings to positions
- describe ordering using quaternary alphabet
- describe error correction using redundancy information
- describe decoding process
- introduce noisy reads
- describe pre-processing of nucleotide symbol strings
- describe de-randomization
- describe unwinding constrained encoding
- describe quality information
- describe confidence score
- introduce solitary-strand-based processing
- describe seriatim processing
- describe cluster-based processing
- motivate flexible decoding
- illustrate limitations of cluster-based approach
- present example sequencing data
- evaluate sequencing data using Hamming distance
- demonstrate superiority of flexible-based approach
- introduce trace reconstruction problem
- define mathematical notation for trace reconstruction
- model noise in sequencing technologies
- describe synthetic test data creation
- discuss sequencing-by-synthesis technology
- discuss nanopore sequencing technology
- illustrate architecture for trace reconstruction system
- describe DNA synthesis process
- define oligonucleotide synthesizer
- discuss DNA storage library structure
- describe DNA pool implementation
- discuss sequencing process using polynucleotide sequencer
- describe PCR amplification process
- discuss errors introduced by PCR
- describe polynucleotide sequencer operation
- categorize errors introduced by sequencing
- describe sequencing-by-synthesis technology details
- describe nanopore sequencing technology details
- mention other sequencing technologies
- conclude trace reconstruction system description
- introduce sequencing technologies
- describe error profiles
- introduce quality information
- describe polynucleotide sequencer output
- introduce trace reconstruction system
- describe system implementation options
- illustrate system operation
- describe noisy reads
- introduce consensus output sequence
- describe converter operation
- highlight system flexibility
- introduce alignment anchor
- describe position of comparison
- introduce look-ahead window
- determine plurality consensus base
- identify variant reads
- determine look-ahead window consensus
- classify substitution errors
- move position of comparison
- illustrate substitution error identification
- describe deletion error identification
- determine look-ahead window consensus
- classify deletion errors
- move position of comparison
- illustrate deletion error identification
- describe insertion error identification
- determine look-ahead window consensus
- classify insertion errors
- move position of comparison
- illustrate insertion error identification
- discuss multiple error types
- introduce indeterminant errors
- describe indeterminant error identification
- illustrate indeterminant error
- highlight error classification limitations
- discuss error correction techniques
- highlight system advantages
- discuss system applications
- highlight system flexibility
- summarize system operation
- conclude system description
- introduce handling reads with indeterminant errors
- discard read with indeterminant error
- limitations of discarding reads
- use bias or tiebreaker to force classification
- application of bias based on error profile
- use quality information to classify ambiguous errors
- omit low-confidence base calls
- illustrate skipping over inactive trace
- generate consensus output sequence
- identify position of comparison
- introduce delay in inactive trace
- evaluate inactive trace to identify matching sequence
- create consensus look ahead sequence
- seek candidate position
- calculate position of comparison
- use search window to evaluate multiple positions
- define comparison sequence
- define match-backwards region
- define match-forwards region
- compare sub-sequences of inactive trace and comparison sequence
- determine if candidate position represents a match
- handle no match within search window
- handle match within search window
- set position of comparison for trace reconstruction
- handle multiple candidate positions
- select candidate position to restart trace reconstruction
- repeat process for multiple indeterminant errors
- apply technique to multiple reads in a cluster
- vary active traces at different alignment locations
- conclude handling reads with indeterminant errors
- introduce alignment anchors
- motivate use of alignment anchors
- describe function of alignment anchors
- explain benefits of alignment anchors
- describe creation of alignment anchor sequence
- discuss arrangement of alignment anchors
- illustrate example of single alignment anchor
- illustrate example of multiple alignment anchors
- explain how alignment anchors divide DNA strand
- describe how alignment anchors act as additional ends
- illustrate example of biased spacing of alignment anchors
- explain importance of unique alignment anchor sequences
- describe how partial consensus sequences are generated
- explain how partial consensus sequences are joined
- describe how alignment anchor is used to create full consensus output sequence
- explain how base calls corresponding to alignment anchor are removed
- describe trace reconstruction system
- illustrate block diagram of trace reconstruction system
- describe processing unit(s) of trace reconstruction system
- describe memory of trace reconstruction system
- describe input/output devices of trace reconstruction system
- explain how trace reconstruction system is connected to polynucleotide sequencer
- describe sequence data interface
- describe randomization module
- explain how randomization module increases randomness in synthetic DNA strands
- describe clusterization module
- explain how clusterization module clusters reads
- describe importance of clustering
- explain how poorly formed clusters can occur
- describe types of clustering techniques
- explain how trace reconstruction system analyzes clusters
- describe dependence of number of reads in a cluster on depth of sequencing
- provide example of number of reads per cluster with Nanopore sequencing
- conclude description of trace reconstruction system
- introduce read alignment module
- align reads at position of comparison
- advance position of comparison based on error types
- generate reverse alignment
- combine forward and reverse alignments
- introduce alignment anchor module
- identify alignment anchor sequences
- divide reads into sub-reads
- align sub-reads
- reduce effects of accumulated mistakes
- introduce variant read identification module
- determine consensus base call
- label variant reads
- introduce error classification module
- classify error types
- introduce consensus output sequence generator
- determine consensus output sequence
- assemble consensus output sequences for sub-sequences
- combine consensus output sequences
- remove alignment anchor sequences
- introduce error correction module
- apply additional error correction techniques
- decode consensus output sequence
- introduce conversion module
- convert consensus output sequence to binary data
- introduce example trace reconstruction processes
- reversibly randomize binary data
- cluster sequence data
- receive reads for further analysis
- identify position of comparison
- describe process for correcting insertion, deletion, and substitution errors
- implement process using trace reconstruction system
- receive sequence data via sequence data interface
- perform clustering using clustering technique
- classify reads as representing same DNA strand
- determine consensus base call
- compare base call to consensus
- identify variant read
- compare consensus string to variant read
- determine error type
- advance position of comparison
- determine error type
- advance position of comparison
- determine error type
- advance position of comparison
- consider error profile
- determine reliability of variant read
- omit or use variant read
- determine if end of read is reached
- advance position of non-variant reads
- advance position of variant read
- determine single consensus output sequence
- recover binary data from DNA
- randomize binary data
- receive and cluster reads
- select and analyze cluster
- introduce Example 9
- align clustered set of reads
- determine consensus base call
- identify variant read
- identify consensus string of base calls
- determine error type for variant read
- move position of comparison
- determine error type for variant read
- move position of comparison
- determine error type for variant read
- move position of comparison
- generate single consensus output sequence
- convert consensus output sequence to binary data
- introduce process 2500
- identify start of portion of read with bursty error
- identify end of portion of read with bursty error
- omit portion of read with bursty error
- generate consensus output sequence with remaining portions
- introduce process 2600
- identify indeterminate error in read
- define search window
- calculate edit distances for sub-sequences
- determine if match is found
- remove read from consideration
- select one of multiple matching subsequences
- identify single sub-sequence with edit distance less than threshold
- select candidate position within matching sub-sequence
- set position of comparison
- determine consensus output sequence at position of comparison
- introduce Example 27
- convert digital information to nucleotide symbol string
- synthesize DNA molecule
- store DNA molecule in DNA storage library
- introduce Example 28
- prepare DNA strands for sequencing
- sequence DNA strands with polynucleotide sequencer
- generate reads from DNA strands
- introduce errors into data
- categorize errors as substitution, insertion, or deletion
- determine base call for each position in read
- introduce sequencing-by-synthesis technology
- amplify DNA on solid surface
- perform sequential sequencing
- introduce nanopore sequencing technology
- immerse nanopore in conducting fluid
- apply potential across nanopore
- measure change in current as DNA molecule passes through nanopore
- represent DNA sequence as change in current
- describe SMRT technology
- describe Helicos True Single Molecule Sequencing (tSMS) technique
- describe SOLiD technology
- describe chemFET array sequencing
- describe electron microscope sequencing
- discuss error rates of sequencing technologies
- describe quality information in sequencing output
- describe polynucleotide sequencer output
- describe trace reconstruction system
- describe computing device for trace reconstruction
- describe network connections for trace reconstruction
- describe output of trace reconstruction system
- describe computing system architecture
- describe processing units
- describe memory
- describe software implementation
- describe hardware logic components
- describe storage
- describe input devices
- describe output devices
- describe communication connections
- describe computer-executable instructions
- describe program modules
- describe computer-readable media
- describe storing actions
- describe cloud computing environment
- describe cloud computing services
- describe computing devices
- describe hybrid scenarios
- describe method implementation
- describe example embodiments
- describe clause 1
- describe clause 2
- describe clause 3
- describe clause 4
- describe clause 5
- describe clause 6
- describe clause 7
- describe clause 8
- describe clause 9
- describe clause 10
- describe clause 11
- describe clause 12
- describe clause 13
- describe clause 14
- describe clause 15
- describe clause 16
- describe clause 17
- describe clause 18
- describe clause 19
- describe example alternatives

