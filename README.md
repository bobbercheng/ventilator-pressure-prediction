# ventilator-pressure-prediction

## Meeting 10/8

Participants: Chris, Bobber

Our target is to get silver medal in this competation, it means Top 5%, refer https://www.kaggle.com/progression
Meeting time: 5pm Toronto/ 11pm Munich

1. self introduce.
2. Chris reviewed all public notebooks and found following things:
   1. EDA, they uses same feature EDA
   2. Model, they uses bilstm
   3. Post/process, someone uses round to improve the score

2. Problems:
   1. Chris has no enough TPU. Solution: Chris can use TPU in kaggle team while Bobber uses TPU with Colab.
   2. TPU is stop after 12 hour of running https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator-bidirectional-lstm-modification.ipynb. It reachs to Fold 5. Solution: Download checkpoint_filepath from colab and continue train

3. Next plan
   1. EDA, Chris checks if there are other useful features
   2. Model, Bobber add CNN
   3. Stack, Bobber adds stacks
   4. Post/process, Chris checks it.

Next meet time: 5pm Toronto/ 11pm Munich
