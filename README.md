# FCTalker: Fine and Coarse Grained Context Modeling for Expressive Conversational Speech Synthesis

## Introduction
This is an implementation of the following paper.
> [FCTALKER: FINE AND COARSE GRAINED CONTEXT MODELING FOR EXPRESSIVE CONVERSATIONAL SPEECH SYNTHESIS.](https://arxiv.org/abs/2210.15360)
> Submitted to ICASSP'2023

Yifan Hu, [Rui Liu *](https://ttslr.github.io/), Guanglai Gao, [Haizhou Li](https://colips.org/~eleliha/).
 

[![arXiv](https://img.shields.io/badge/arXiv-Paper-<COLOR>.svg)](https://arxiv.org/abs/2210.15360)

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
## About Fine-Grained Encoder
The Fine-Grained Encoder in this source code directly uses the pre-trained model of [TOD_BERT](https://huggingface.co/TODBERT/TOD-BERT-JNT-V1/tree/main). You can easily load the pretrained model using huggingface [Transformers](https://github.com/huggingface/transformers) library using the AutoModel function. 
```
import torch
from transformers import *
tokenizer = AutoTokenizer.from_pretrained("TODBERT/TOD-BERT-JNT-V1")
tod_bert = AutoModel.from_pretrained("TODBERT/TOD-BERT-JNT-V1")
```

The source code for the Fine-Grained Encoder can be viewed in [modules.py](https://github.com/walker-hyf/FCTalker/blob/3752d8528d2c4956ff7e30038a5b6e70383c6aa1/model/modules.py#L854).


## Citation

```bash
@misc{https://doi.org/10.48550/arxiv.2210.15360,
  doi = {10.48550/ARXIV.2210.15360},
  url = {https://arxiv.org/abs/2210.15360},
  author = {Hu, Yifan and Liu, Rui and Gao, Guanglai and Li, Haizhou},
  keywords = {Computation and Language (cs.CL), Sound (cs.SD), Audio and Speech Processing (eess.AS), FOS: Computer and information sciences, FOS: Computer and information sciences, FOS: Electrical engineering, electronic engineering, information engineering, FOS: Electrical engineering, electronic engineering, information engineering},
  title = {FCTalker: Fine and Coarse Grained Context Modeling for Expressive Conversational Speech Synthesis},
  publisher = {arXiv},
  year = {2022},
  copyright = {Creative Commons Attribution 4.0 International}
}
```

## Author

E-mail：hyfwalker@163.com

## References
- [keonlee9420's DailyTalk](https://github.com/keonlee9420/DailyTalk)
- [jasonwu0731's ToD-BERT](https://github.com/jasonwu0731/ToD-BERT)
