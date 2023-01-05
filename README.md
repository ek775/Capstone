# Automating Subject Indexing for Arxiv
Subject indexing for scientific articles in databases is currently a labor intensive task that requires people to label subjects and update subject databases regularly at great expense. MeSH term indexing for PubMed, for example, costs tens of millions of dollars each year in labor to comb through the database for new subject headings. Machine Learning with Natural Language Processing offers a potential route to automate this process, yielding immense cost savings and rapidly increasing the pace of new subject heading indexing for use by researchers. To do this, two models are necessary to perform two key tasks: identify the subject headings of newly added articles, and analyze the database to update subject drift and add new subjects. The first task can be accomplished using supervised learning techniques and the latter only by unsupervised techniques, hence we can refer to them as the "supervised" and "unsupervised" arms.

The hypothetical stakeholder for this project is Cornell University Library, which, maintains the arXiv database that data was pulled from for this project*. The objective for this project was to develop a moodel that could categorize scientific papers by their subject headings using the text data from their abstracts--in essence, the supervised arm of a fully automated indexing system.

*This project was done for a capstone at Flatiron School and is for academic purposes only. I am not affiliated with Cornell University Library or arXiv, and neither entity has endorsed this work.

# Data

The data for this project was accessed via Kaggle for ease of access. The dataset available from the link below includes over 1.5 million abstracts and their associated subject headings that were collected from the arXiv database between 1993 and 2019. Due to resource constraints, a subset of the dataset was used that included the 10 most common subject headings and abstracts associated with them.

The data file used for this project was too large to be included in the repository even after compression (>100MB limit), however, it is publicly available from the URL below:
https://www.kaggle.com/datasets/tayorm/arxiv-papers-metadata?resource=download&select=arxiv-oai-af.tsv

# Methods

An iterative modeling approach was used to develop a model for this project based on Recurrent Neural Network architecture. 

RNNs offer a few distinct advantages over more traditional natural language processing methods. Primarily, RNNs are used because they are capable of retaining information about the order of words in addition to their frequency and document location in a tf-idf matrix. The basic structure of the models explored here are based on the use of Gated Recurrent Units (GRUs) in both feed forward and bi-directional layer structures. The modeling process here was accomplished primarily with the use of the Keras API for the Tensorflow library, as well as some Latent Semantic Analysis that was performed using the Scikit Learn library to tune the embedding layer.

The intial model was constructed using a single embedding layer to vectorize the abstracts, a single feed-forward GRU layer, and a single dense layer leading to the output layer with softmax activation. Due to initial problems with gradient stability, the initial model went through a series of different activation functions and architecture changes prior to being used for iterative modeling.

Due to the complexity of RNNs, it is recommended to parallelize model training using GPUs rather than CPUs. To that end, AWS EC2 services were used for this project, however, AWS restricts the use of GPU hardware and requires users to request access. I requested access and was denied, thus, the model training is a bit slower than was ideal and I could not make use of the full dataset. Instead, EC2 was utilized to perform the training using CPU resources.

# Results

The best model that resulted from this process yielded a test accuracy of 51.7% with training and validation loss values failing to converge. It is worth noting that validation accuracy does not generally increase, but rather oscillates between 49% and 81% during training, suggesting that gradient stability is the cause of the overfitting observed. 

# Discussion

Compared to the baseline of 18% accuracy based on the frequency of the most common class in the data, this is still a significant improvement, however, studies on automated indexing for MeSH terms using similar architecture have attained accuracies between 60% and 80%, suggesting that the final model developed here still has significant room for improvement. At present, this model is not ready for use and needs further development. Future areas for improvement should focus on gradient stability to improve model training, as well as obtaining additional resources to enable training across the full dataset.

# Notes and References

Although I saved the models produced during this project, they have not been uploaded to github following the initial model. Because of the large number of parameters, no other model past the initial one was able to be compressed and uploaded to github under the 100MB file limit.

Koutsomitropoulos DA, Andriopoulos AD. Automated MeSH Indexing of Biomedical Literature Using Contextualized Word Representations. Artificial Intelligence Applications and Innovations. 2020 May 6;583:343–54. doi: 10.1007/978-3-030-49161-1_29. PMCID: PMC7256379.

# Repository Guide

|-gitignore                                        -- ignored files
|-Kousomitropoulos et al 2020.pdf                  -- recent study on automating MeSH terms
|-LICENSE                                          -- use license
|-Presentation.pdf                                 -- presentation slides
|-Project Workbook.ipynb                           -- jupyter notebook containing source code
|-README.md                                        -- the README file
|-auto_sub_index.yml                               -- environment used on the EC2 server
|-models.tar.gz                                    -- compressed version of initial model
