# MatchSum
Code for ACL 2020 paper: **Extractive Summarization as Text Matching**


## Dependencies
- Python 3.7
- [PyTorch](https://github.com/pytorch/pytorch) 1.4.0
- [pyrouge](https://github.com/bheinzerling/pyrouge) 0.1.3
	- You should fill your ROUGE path in metrics.py line 20 before running our code.
- [rouge](https://github.com/pltrdy/rouge) 1.0.0
	- Used in  the validation phase.
- [transformers](https://github.com/huggingface/transformers) 2.5.1

	
All code only supports running on Linux.

## Data

We have already processed CNN/DailyMail dataset, you can download it through [this link](https://drive.google.com/open?id=1FG4oiQ6rknIeL2WLtXD0GWyh6pBH9-hX), unzip and move it to `./data`. It contains two versions (BERT/RoBERTa) of the dataset, a total of six files

## Train

We use eight Tesla-V100-16G GPUs to train our model, the training time is about 30 hours. If you do not have enough video memory, you can reduce the *batch_size* or *candidate_num* in *train_matching.py*, or you can adjust *max_len* in *dataloader.py*.

You can choose BERT or RoBERTa as the encoder of **MatchSum**,  for example, to train a RoBERTa model, you can run the following command:

```
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python train_matching.py --mode=train --encoder=roberta --save_path=./roberta --gpus=0,1,2,3,4,5,6,7
```

## Test

After completing the training process, several best checkpoints are stored in a folder named after the training start time, for example, `./roberta/2020-04-12-09-24-51`. You can run the following command to get the results on test set: (only one GPU is required for testing) 

```
CUDA_VISIBLE_DEVICES=0 python train_matching.py --mode=test --encoder=roberta --save_path=./roberta/2020-04-12-09-24-51/ --gpus=0
```
The ROUGE score will be printed on the screen, and the output of the model will be stored in the folder  `./roberta/result`.

