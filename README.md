# PLM-VF: A Deep Learning Framework for Identification and Classification of Virulence Factors Using Protein Language Models

## 1. Database construction
In this study, to train the PLM-VF on an up-to-data virulence factors sequence dataset, we downloaded sequences form VFDB[1] SetB(just bacterial pathogens), Victors[2](include bacteria，viruses, parasites and fungi ) and BV-BRC[3] (just bacterial pathogens), then, constructed as the positive dataset contained 33,864 sequence. The non-virulence protein sequences were searched mainly from Uniprot Swiss-Prot[4] database with several keywords in bacteria, viruses, parasitica and fungi which limit its sequence length between 50 and 5000 amino acids. As a result, the none-virulence protein dataset contained 33,434 sequence. For the VFs or not predicted task, 1442 VFs and 1442 non-VFs were randomly selected as the independent test dataset, while the remaining were used as the training dataset. We use the 14 base categories from the VFDB category scheme as the foundational labels. We then use DIAMOND[5] and TM-Vec[6] to transform this into a multi-label classification task: (1) First, each sequence's label is converted into a 14-dimensional vector. Next, DIAMOND Cluster is used for clustering, and the union of all labels within a cluster is assigned to all sequences in that cluster ; (2)TM-Vec was utilized to perform structural similarity searches and compute the TM-Score for the top 100 nearest neighbors of each sequence. Pairs with a TM-Score greater than 0.75 were retained. Subsequently, shared labels were assigned to each sequence pair based on these results.
## 2. Model architecture

We propose PLM_VF which add a simple network as prediction head to the pLMs and fine-tuning it as supervised manner for VFs identification and category. A network propagation algorithm is adopted to exploit the sequence and structure homology information that can account for the related VFs.

![pipline](http://119.3.41.228/PLM-VF/images/home.png)

## 3. Code usage

To use the PLM-ARG codes, you first need to download the 'esm2_t30_150M_UR50D' model and put it under the fold of 'models/', and run the following command:

```python
predict.py -i data/test.fasta -o results/predict_results.txt
```

## 4. Web server
We have released a web service to process gene sequence or predicted ORF using PLM-ARG. You can find the website at http://www.unimd.org/PLM-VF. PLM-VF takes the gene sequence as the input and output including both the VFs (if the query was classified as VF) and the corresponding probability.
## 6. Dependencies
- python 3.8.13
- pytorch 2.4.1
- transformers 4.44.2
- biopython 1.83
### References
[1] Liu, B., Zheng, D., Zhou, S., Chen, L. & Yang, J. VFDB 2022: a general classification scheme for bacterial virulence factors. Nucleic Acids Res. 50, D912 (2022).

[2] Sayers, S. et al. Victors: a web-based knowledge base of virulence factors in human and animal pathogens. Nucleic Acids Res. 47, D693–D700 (2019).

[3] Olson, R. D. et al. Introducing the Bacterial and Viral Bioinformatics Resource Center (BV-BRC): a resource combining PATRIC, IRD and ViPR. Nucleic Acids Res. 51, D678–D689 (2023).

[4] The UniProt Consortium. UniProt: the Universal Protein Knowledgebase in 2023. Nucleic Acids Res. 51, D523–D531 (2023).

[5] Buchfink, B., Reuter, K. & Drost, H.-G. Sensitive protein alignments at tree-of-life scale using DIAMOND. Nat. Methods 18, 366–368 (2021).

[6] Hamamsy, T. et al. Protein remote homology detection and structural alignment using deep learning. Nat. Biotechnol. 1–11 (2023) doi:10.1038/s41587-023-01917-2.