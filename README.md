# NLP-document-auto-clustering

written in Python3.6.5 and tested in CentOS Linux release 7.5.1804 (Core); (partially tested in Ubuntu 16.0.4 )

### Drop any (large amount of) document files into  "/src/files" folder,  and this script will auto analyse their contents and associate files based on content relationship and similarities

Key features:

- Multi-format parser supports almost all common document formats including pdf, word docx, powerpoint pptx, txt

- Auto-detection of file language

- NLP based file processing and feature extraction (POS tagging and tokenization, support English, French, German and most European languages)

- Visualization of file clustering results

### Python Dependencies and Prerequisites

Python 3.6.5, dependencies can be found in requirement.txt

Note: some CentOS (7.5.1804) is not shipped with tkinter(required by pyplot), need to install manually in CLI:

```
sudo yum install python36u-tkinter.x86_64
```

### How to run the code

The run.py file is under "/src" folder.  First install the pre-requisite packages, then use the following command line:

```
python3.6 run.py
```

the program will automatically start and finish the workflow

### Overall workflow

The overall workflow includes:
multi-format file parser => language detect => foreign language processing => NLP POS tokenization => key-word filtering => dimention reduction stemming/lemmatization => vecterize word space (tf-idf) => k-means clustering (auto-K selection) => visualize results

Below are step by step detailed descriptions

Step 1, a dedicated parser is used to parse any text from txt, docx, pptx, pdf files, return plain texts and file names

Step 2, auto-detect language of file content using langdetect

Step 3, there are several options to handle foreign language files

option 1. remove foreign languages and only keep English files for clustering study

option 2. perform separate clustering study, one for each language

Option 3. translate foreign language text to English (using goslate or similar package), then treat the traslated content as English text

Option 4. use word embedding to convert word space to abstract vector space (such as Word2Vec), then cluster files based on abstract vector space

currently support option 1, i.e. only cluster English files.  More features could be added in future to support other options.

Step 4, tokenize plain text with NLP tokenizer, use word-tokens to filter out the key words ("proper nouns" NNP as key words)

Step 5, reduce word-space by normalizing proper-noun words to stem form, first stemming, then lemmatization

Step 6, form overall corpus by combining file keywords, then vectorize word-space via tf-idf

Step 7, auto select optimum K value using elbow method, with a pre-set threshold on elbow gradient

apply K-Means clustering with optimum k and find category label for each file

Note: The demo files have number of features less than 1000, dimension reduction is not necessary. If large number of features cause effeciency issues, PCA could be applied prior to K-means to reduce feature dimensions

Step 8, visualize clustering results in 2-D space

reduce feature dimension to 2-D using PCA

generate 2-D scatter plot visualization, each point represents one file, points are colored based on cluster category

Step 9, generate analysis report

An analysis.txt report is auto-generated and saved under "/src" folder, it contins all proccessed info and identified info of each document file, including file names, file languages, proper nouns found, and file clustering categories

### Acceptable file format in "files" folder

All files inside files" folder will be processed

Currently most common document formats are surpoorted, including:
.txt (utf-8)
.docx
.pdf
.pptx
note: unaccepted file format may give errors
currently accept English, French and German files, other languages support are not tested

### Demo files for clustering

Some example files are included under "src/files" for easy validation

### Discussion of future improvements

- This project serves as a working demo of document auto-grouping tool which can analyse patterns among large amount of unstructured  files and gain structured insight into file relationships.  files using NLP and unsupervised clustering

- web APIs such as wiki and twitter APIs can be added to the parser to allow clustering of online contents 

- text embedded in images are not parsed, image OCR extraction module such as pytesseract could be added to the parser tool to support parsing text from embedded images in pdf, pptx and word docx

- the nltk POS tokenizer only has native support for English, French ad German has been tested and seems to work ok, however the support for foreign language can be improved by using dedicated POS tokenizers for specific language

- outlier detection/removal can be fine-tuned to improve clustering accuracy

- more sophisticated error handling mechanism could be added

