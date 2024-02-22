# FCTALKER: FINE AND COARSE GRAINED CONTEXT MODELING FOR EXPRESSIVE CONVERSATIONAL SPEECH SYNTHESIS

## Introduction
This is an implementation of the following paper.
[《FCTALKER: FINE AND COARSE GRAINED CONTEXT MODELING FOR EXPRESSIVE CONVERSATIONAL SPEECH SYNTHESIS》](https://arxiv.org/pdf/2210.15360.pdf)

Yifan Hu, [Rui Liu *](https://ttslr.github.io/), [Haizhou Li](https://colips.org/~eleliha/).

## Demo Page
[Speech Demo](https://walker-hyf.github.io/FCTalker/)

## Dataset
You can download [dataset](https://drive.google.com/drive/folders/1WRt-EprWs-2rmYxoWYT9_13omlhDHcaL) from DailyTalk.

## Dependencies

This project uses `conda` to manage all the dependencies, you should install [anaconda](https://anaconda.org/) if you have not done so. 

```bash
# Clone the repo
git clone https://github.com/walker-hyf/FCTalker.git
cd $PROJECT_ROOT_DIR
```

Install dependencies
```bash
conda env create -f ./environment.yaml
```

Activate the installed environment
```bash
conda activate FCTalker
```

## Preprocessing

Run 
  ```
  python3 prepare_align.py --dataset DailyTalk
  ```
  for some preparations.

  For the forced alignment, [Montreal Forced Aligner](https://montreal-forced-aligner.readthedocs.io/en/latest/) (MFA) is used to obtain the alignments between the utterances and the phoneme sequences.
  Pre-extracted alignments for the datasets are provided [here](https://drive.google.com/drive/folders/1fizpyOiQ1lG2UDaMlXnT3Ll4_j6Xwg7K?usp=sharing). 
  You have to unzip the files in `preprocessed_data/DailyTalk/TextGrid/`. Alternately, you can [run the aligner by yourself](https://montreal-forced-aligner.readthedocs.io/en/latest/user_guide/workflows/index.html). Please note that our pretrained models are not trained with supervised duration modeling (they are trained with `learn_alignment: True`).

  After that, run the preprocessing script by
  ```
  python3 preprocess.py --dataset DailyTalk
  ```

## Training

Train your model with
```
python3 train.py --dataset DailyTalk
```
Useful options:
- Currently only single GPU training is supported.

## Inference

Only the batch inference is supported as the generation of a turn may need contextual history of the conversation. Try

```
python3 synthesize.py --source preprocessed_data/DailyTalk/val_*.txt --restore_step RESTORE_STEP --mode batch --dataset DailyTalk
```
to synthesize all utterances in `preprocessed_data/DailyTalk/val_*.txt`.


## Author

E-mail：hyfwalker@163.com
