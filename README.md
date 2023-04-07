# CS678_FinalProject
## Introduction
This is the repository for annotated data and model for this paper: </br>

> Mandar Chaudhari and Ashlesha Deokar [How Do We Answer Complex Questions: Discourse Structure of Long-form Answers](https://arxiv.org/abs/2203.11048).

The previous authors have annotated sentence-level functional roles of long-form answers from three datasets ([NQ](https://ai.google.com/research/NaturalQuestions), [ELI5](https://facebookresearch.github.io/ELI5/explore.html) and the human demonstrations from [WebGPT](https://openai.com/blog/webgpt/)) as well as a subset of model-generated answers from [previous work](https://github.com/martiansideofthemoon/hurdles-longform-qa). We analyzed their discourse structure and trained a role classification [model](https://github.com/utcsnlp/lfqa_discourse#model) which can be used for automatic role analysis.

## Data

There are three different data file which where provided in the github repository of the authors

### Validity annotation

Validity annotation is the binary classification task of determining whether a (question, answer) pair is valid, based on a set of invalid reasons we defined. Each pair is annotated by three annotators, each of them provide a binary label indicating validity, and a list of invalid reasons.

Annotations for this task is stored in `data/validity_annotation.jsonl`, which contains 1,591 (question, answer) pairs.

Each example is a json with the following field:

* `dataset`: The dataset this QA pair belongs to, one of [`NQ`, `ELI5`, `Web-GPT`, `ELI5_MODEL`]. 
* `q_id`: The question id, same as the original NQ or ELI5 dataset.
* `a_id`: The answer id, same as the original ELI5 dataset. For NQ, we populate a dummy `a_id`. For machine generated answers, this field corresponds to the name of the model. For Web-GPT answers, this field will be 'Human demonstration'.
* `question`: The question.
* `answer_paragraph`: The answer paragraph.
* `answer_sentences`: The list of answer sentences, tokenized from the answer paragraph.
* `is_valid`: A boolean value indicating whether the qa pair is valid, values: [`True`, `False`].
* `invalid_reason`: A list of list, each list contains the invalid reason the annotator selected. The invalid reason is one of [`no_valid_answer`, `nonsensical_question`, `assumptions_rejected`, `multiple_questions`].

### Role annotation

Role annotation is the task of determining the role of each sentence in the answer paragraph of a _valid_ (question, answer) pair, identified in the validity annotation. Each paragraph is annotated by three annotators, each of them provide one out of the six roles for each sentence in the answer paragraph. For each sentence, we release the majority (or ajudicated) role, as well as the raw annotations from annotators.

Annotations for this task is stored in `data/role_annotation.jsonl`, which contains 755 (question, answer) pairs with sentence-level role annotation.

Each example is a json with the following fields:

* `dataset`: The dataset this QA pair belongs to, one of [`NQ`, `ELI5`, `Web-GPT`, `ELI5_MODEL`]. 
* `q_id`: The question id, same as the original NQ or ELI5 dataset.
* `a_id`: The answer id, same as the original ELI5 dataset. For NQ, we populate a dummy `a_id` (1). For machine generated answers, this field corresponds to the name of the model. 
* `question`: The question.
* `answer_paragraph`: The answer paragraph.
* `answer_sentences`: The list of answer sentences, tokenized from the answer paragraph.
* `role_annotation`: The list of majority role (or adjudicated) role (if exists), for the sentences in `answer_sentences`. Each role is one of [`Answer`, `Answer - Example`, `Answer (Summary)`, `Auxiliary Information`, `Answer - Organizational sentence`, `Miscellaneous`]
* `raw_role_annotation`: A list of list, each list contains the raw role annotations for sentences in `answer_sentences`.

### NQ complex questions

We also release the 3,190 NQ questions with paragraph-level answer only that are classified as complex questions in `data/nq_complex_qa.jsonl`. 

Each example is a json with the following field:
* `q_id`: The question id, same as the original NQ dataset.
* `question`: The question.
* `answer_paragraph`: The answer paragraph.
* `wiki_title`: The title of the wikipedia page the answer is extracted from.
* `split`: The split from the original NQ dataset, one of [`train`, `validation`]

## Model

The previous author has provided role classification model as well as the train/validation/test and out-of-domain data (NQ, WebGPT and ELI5-model) used in the paper, under the folder `role_classifier/data/`, with the following field: 

* `input_txt`: Input to T5 model.
* `target_txt`: Expected output with the role labels.
* `q_id`: The question id, same as those in `role_annotation.jsonl`.
* `a_id`: The answer id, same as those in `role_annotation.jsonl`.
* `target_all_labels`: A string containing all annotated roles, separated by comma for each sentence in the paragraph, used to calculate `Match-any` metric.

Below is the process to reproduce their results.

### Install the requirements
```bash
$ git clone https://github.com/mandarc64/CS678_FinalProj
$ cd role_classifier
```

This code has been tested with Python 3.9:
```bash
$ pip install requirements.txt
```
Installing the requirments.txt might give error in installing some of the python packages, but while executing I got the error  
```bash 
No modulle name ____ 
``` 
So just installing that package manually works For Example: 
```bash
pip install _____ 
```

### Download the pre-trained model
Download the model from this 
[link](https://drive.google.com/file/d/1L_DbGhFqN-KBPJeTDFCAvX3RPZELJE9R/view?usp=sharing) and place it in the `models` directory. 
Note: We need to create a folder name models under role_classifier directory and place the t5 folder in it (unzip and extract) 
For example:
`role_classifier/models/`

### Run predictions with pre-trained model 
Sentence-level prediction will be saved to `<output_dir>/test_prediction.csv`

```bash
# t5 
python run_t5_role_classifier.py --train_file data/t5/train.csv --validation_file data/t5/validation.csv --test_file data/t5/test.csv --output_dir outputs/t5/test/ --do_predict --overwrite_output_dir --evaluation_strategy epoch --predict_with_generate --num_train_epoch 0 --model_name_or_path models/t5
```
