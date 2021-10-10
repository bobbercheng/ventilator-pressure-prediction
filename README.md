# ventilator-pressure-prediction

## Meeting 10/7

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
   2. Model, Bobber add CNN.
   3. Stack, Bobber adds stacks
   4. Post/process, Chris checks it.

Next meet time: 5pm Toronto/ 11pm Munich


## Meeting 10/8
What we did:
1. EDA, Chris found https://www.kaggle.com/danofer/ts-windows-feature-engineering-ventilators.
2. Model, Bobber added CNN and merged aboved EDA, https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator_pressure_1d_cnn.ipynb. CX: 0.38475, LP 0.345

In Progress:
1. Run BiLSTM with Colab.

Next Plan:
1. Model
   1. Chris will check https://www.kaggle.com/lucamassaron/rescaling-layer-for-discrete-output-in-tensorflow
   2. Bobber will try to add attention or combine CNN and BiLSTM

## Meeting 10/9
What we did:
1. EDA: Bobber found pressure only have 950 values. In range 4-11, pressure only have 100 values and R/C only have 10 combination.
1. Model
   1. Chris checked https://www.kaggle.com/lucamassaron/rescaling-layer-for-discrete-output-in-tensorflow and median_round. It improved PL 0.001

Next Plan:
1. Model
   1. Bobber add simple attention. 0.158. https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator-bidirectional-lstm-modification_v3.ipynb
   2. Bobber will add new cat output what have 101 types corresponding 100 values of pressure.
   3. Chris try to combine CNN and BiLSTM.

