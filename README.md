# Factorized Bilinear Multi-Aspect Attention (FBMA)

Code for the paper: "[Event Detection using Hierarchical Multi-Aspect Attention](https://dl.acm.org/doi/10.1145/3308558.3313659)" by
Sneha Mehta, Mohammad Raihanul Islam, Huzefa Rangwala, Naren Ramakrishnan.

The model presented in the paper can be used for general text classification tasks, especially for large texts. Model assigns relative importances to different sentences and different weights to different words in each sentence.

### Setup
All commands must be run from the project root. The following environment variables can be optionally defined:

```
bash
export HOME_DIR="project path"
export PYTHONPATH=$PYTHONPATH:.
```

### Install dependencies

```
bash
sudo apt install python3-dev python3-virtualenv
virtualenv -p python3 --system-site-packages env3
. env3/bin/activate
pip install -r requirements.txt
```

### Dataset Prep
For licensing reasons we cannot make the datasets in the paper available. Your own data should be prepared as described below. Data should be split into `train.csv`, `val.csv` and `test.csv` files.  Each file should contain tokenized, lemmatized text and the class label. Sentences in the text should be separated by a `<s>` markup.

Example record:
Considering there are two classes:

```
baghdad national iraqi news agency nina the armed forces have killed dozens of terrorists including three suicide bombers during repelling an attack by daesh on gonna village in sharqat <s> a source in the defense ministry told the national iraqi news agency/ nina that daesh terrorist gangs attacked this morning the military units stationed in the district of sharqat khanokah village where they were addressed and inflicted them heavy losses killing gonna large number of the terrorist enemy including 3 suicide bombers and dismantled 20 explosive devices planted by terrorists to hinder the progress of the armed forces end,0
```


### Train Word Embeddings

```
python train_word2vec.py --path <path_to_store_embeddings> \
--data_dir <path_to_processed_data> \
--dim <embedding_dimension>
```
Should generate word2vec.100d.[x]k.w2v file, where [x] refers to the vocab size.


## Training

Creates dictionary, trains and saves the model, runs inference on the test set.

```
python main.py \
--data_dir <path_to_processed_data> \
--emb_path <path_to_word2vec.100d.[x]k.w2v_file> \
--aspects 16 \
--epochs 40 \
--experiment <dataset_name>
```

data_dir should contain `train.csv`, `val.csv` and  `test.csv` files.

## Monitoring training progress

You can point tensorboard to the training folder (by default it is `--train_dir=./runs`) to monitor the training process:

```
bash
tensorboard --port 6007 --logdir ./runs
```

## Performance Evaluation

Running the main script will create `stats` and `output` directories. It will store the performance of the current run inside `stats/experiment` (Note: Experiment name can be changed using the `--experiment` flag). It will create `EVAL` and `TEST` directories inside the experiment directory containing `eval.json` and `test.json`. Performance metrics of the model such as precision, recall, f1 etc. can be found inside these files.



## Citing this work

```
@inproceedings{Mehta:2019:EDU:3308558.3313659,
 author = {Mehta, Sneha and Islam, Mohammad Raihanul and Rangwala, Huzefa and Ramakrishnan, Naren},
 title = {Event Detection Using Hierarchical Multi-Aspect Attention},
 booktitle = {The World Wide Web Conference},
 series = {WWW '19},
 year = {2019},
 isbn = {978-1-4503-6674-8},
 location = {San Francisco, CA, USA},
 pages = {3079--3085},
 numpages = {7},
 url = {http://doi.acm.org/10.1145/3308558.3313659},
 doi = {10.1145/3308558.3313659},
 acmid = {3313659},
 publisher = {ACM},
 address = {New York, NY, USA},
 keywords = {Event Encoding, Hierarchical Attention, Multi-Aspect Attention, Neural Networks},
}
```
