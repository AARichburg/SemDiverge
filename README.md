
# Training the divergence detector model

## Prerequisites

1. Bilingual word vectors : We use [BiVec](https://github.com/lmthang/bivec) to train the vectors used in the paper, but you could use any other pre-trained vectors
2. [VDPWI and associated prerequisites](https://github.com/castorini/VDPWI-NN-Torch) : We used the Torch version (made available in this repository), but a [PyTorch version](https://github.com/castorini/Castor/tree/master/vdpwi) has recently been made available, which allows GPU-acclerated training. 
3. The word-aligned parallel corpus on which you want to apply the semantic divergence method

## Outline of steps

1. Generate synthetic training data
2. Convert embeddings and data to a uniform format
3. Train the VDPWI model

## 1. Generating synthetic data


* Main script : ``generate_synthetic_data.sh`` (calls ``create_dict.py`` and ``create_negative_examples.py``)
* This script takes in a directory containing a sample of your parallel data (5000 examples are a good idea), and optionally a directory containing your test data (i.e. data which you want to exlcude from the synthetic data generation process). It then generates synthetic training data using the procedure described in (section 3)
* For now, the inputs to the various python scripts have to follow a rigid nomenclature. Refer to the python files, or look at the example in the ``data`` directory.
*  You would want to run this script twice - once to generate training data, and one to generate tuning data

## 2. Preprocessing data and embeddings

* Main script : ``preprocess_embeddings_and_data.sh`` (calls ``add_suffix.py`` and ``vdpwi/build_vocab``)
*  What the script needs : VDPWI directory, source and target language embeddings, data to train, tune and test vdpwi (lines 3-25) 
*  The purpose of this script to massage all data in a way in which VDPWI can use them. The main thing that this script does is append language specific tags to each word in the embeddings file (lines 30-34), and each word in the data used to train, tune, and test VDPWI, along with renaming the files appropriately (lines 40-68). This allows us to use VDPWI in bilingual settings without modifying the existing code in any way. 
*  This script also converts the embeddings to a torch readable binary format  (line 37)
*   Finally, this script builds the vocab that used by the VDPWI model next (line 63)



## 3. Train VDPWI

* Main script : ``trainVDPWI.sh``
*  Easy enough! Point to the embeddings and the data generated by step 2 and launch


# Crowdsourced Data (Section 4/5)

The two crowdsourced test sets described in Section 4 and used for experiments in Section 5  can be found in the ``datasets'' directory. There are two sets -- one extracted from Commoncrawl, and the other from OpenSubtitles. The data is in tab seperated format with 4 columns.

1) English Sentence
2) French Sentence
3) Label (1 = Non-divergent, 0 = Divergent)
4) Fraction of annotators (out of 5) that voted for the majority class i.e the label


# Machine Translation Experiments (Section 6)



## Prerequisites

* [Sockeye MT Toolkit](https://github.com/awslabs/sockeye) -  The experiments in the paper were run with v1.8.3, but newer versions can also be used provided the parameters are named appropriately.

## Training the MT model

All models in the paper were trained using ``nmt_script.sh``. Before running the script, you need to set a few parameters (lines 6-15) which point to locations where you want to save the model/checkpoints and the data.

## Training data

The data used to train the MT systems is also available. We make available the entire corpora ranked according to the various methods described in the paper, but note that all methods only used a fraction of the data (50% for French-English, 90% for Vietnamese-English). The files have been named according to Tables 3 and 4 in the paper.

* [French-English OpenSubtitles corpus](https://obj.umiacs.umd.edu/semdiverge/opensubs.tar.gz) (~2.7 GB)
* [Vietnamese-English TED Talks](https://obj.umiacs.umd.edu/semdiverge/vien-ted.tar.gz) (~20 MB)

# References and contact

If you use any contents of this repository, please cite us. For any questions, write to yogarshi@cs.umd.edu.

```
@InProceedings{N18-1136,
  author = 	"Vyas, Yogarshi
		and Niu, Xing
		and Carpuat, Marine",
  title = 	"Identifying Semantic Divergences in Parallel Text without Annotations",
  booktitle = 	"Proceedings of the 2018 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long Papers)",
  year = 	"2018",
  publisher = 	"Association for Computational Linguistics",
  pages = 	"1503--1515",
  location = 	"New Orleans, Louisiana",
  url = 	"http://aclweb.org/anthology/N18-1136"
}

@InProceedings{W17-3209,
  author = 	"Carpuat, Marine
		and Vyas, Yogarshi
		and Niu, Xing",
  title = 	"Detecting Cross-Lingual Semantic Divergence for Neural Machine Translation",
  booktitle = 	"Proceedings of the First Workshop on Neural Machine Translation",
  year = 	"2017",
  publisher = 	"Association for Computational Linguistics",
  pages = 	"69--79",
  location = 	"Vancouver",
  url = 	"http://aclweb.org/anthology/W17-3209"
}


```