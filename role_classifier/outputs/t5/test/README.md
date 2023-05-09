---
tags:
- generated_from_trainer
model-index:
- name: test
  results: []
---

<!-- This model card has been generated automatically according to the information the Trainer had access to. You
should probably proofread and complete it, then remove this comment. -->

# test

This model is a fine-tuned version of [models/t5/](https://huggingface.co/models/t5/) on an unknown dataset.
It achieves the following results on the evaluation set:
- eval_loss: 0.3848
- eval_Answer: 0.5183
- eval_AuxiliaryInformation: 0.3966
- eval_Answer-Organizationalsentence: 0.6667
- eval_Answer(Summary): 0.6063
- eval_Miscellaneous: 0.7595
- eval_Answer-Example: 0.7103
- eval_accuracy: 0.5725
- eval_macro_f1: 0.6096
- eval_runtime: 1206.0149
- eval_samples_per_second: 0.051
- eval_steps_per_second: 0.007
- step: 0

## Model description

More information needed

## Intended uses & limitations

More information needed

## Training and evaluation data

More information needed

## Training procedure

### Training hyperparameters

The following hyperparameters were used during training:
- learning_rate: 5e-05
- train_batch_size: 8
- eval_batch_size: 8
- seed: 42
- optimizer: Adam with betas=(0.9,0.999) and epsilon=1e-08
- lr_scheduler_type: linear
- num_epochs: 0.0

### Framework versions

- Transformers 4.27.4
- Pytorch 2.0.0+cpu
- Datasets 2.11.0
- Tokenizers 0.13.2
