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

## Meeting 10/10
What we did:
1. EDA: Bobber confirmed that in range 4-11, predict pressure only use same 100 values as train data, refer to https://www.kaggle.com/bobber/cat-presure.
2. Model: Bobber added attention and residul. 4 layer Attentions improves LB to 0.158. It takes 8 hours to get result.

In progress:
1. Model: 
   1. Bobber added residul. Version 14 is bad, LB 0.17
   2. Chris mix CNN and BiLSTM
   3. Submit https://www.kaggle.com/cdeotte/ensemble-folds-with-median-0-153

Next plan:
   1. Fine tune the best public model from https://www.kaggle.com/mistag/pred-ventilator-lstm-model/notebook. Don't use it, it only can get LP 0.157
   2. Fine tune the second public mode from https://www.kaggle.com/bobber/single-bi-lstm-model-pressure-predict-gpu-infer, LP 0.152

## No meeting 10/11
What we did:
1. The organizer published https://arxiv.org/pdf/2102.06779.pdf. In the paper, they train different model with different R/C. The best MAE is 0.27 while the best in PL is 0.119. It's amazing.
2. Bobber increased CNN to 2024 but CV doesn't improve as 1024. Refer to https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator_pressure_1d_cnn_2048_attention.ipynb
3. Why we only need encoder instead encoder-decoder in this competition? Because we need to predict from hidden state or high demission features but we don't need generate different sentences like NLP.

Next plan:
1. Meet new member.
2. Model:
   1. Fine tune the second public mode from https://www.kaggle.com/bobber/single-bi-lstm-model-pressure-predict-gpu-infer, LP 0.152
   2. Combine CNN and BiLSTM - Chris
   3. Try transformer - Bobber
   4. WaveNet
   5. EfficientNet

## meeting 10/12
What we did:
1. Model: 
   1. Chris combined CNN and BiLSTM. CNN as input to BiLSTM is not good.
   2. Concate CNN and BiLSTL. It looks promising and wait for result.
   3. Bobber implemented Transformer.
      1. Without Position Encode, PL: 0.26 CV: 0.26
      2. With Position Encode. CV.9.23, PL:

Next Plan:
1. Pick up the best public BiLSTM model.
2. Conccat Transformer and BiLSTM.
3. Use embeding for R C.
4. Randon seed/state. Fine tune. tf.set_random_seed(seed_value).
  TF 2.0
  import tensorflow as tf
  tf.random.set_seed(221)
  
  ## meeting 10/20
  Sorry for no update many days.
  
  What we did:
  1. GB+Rescaling has 0.1427 in PL. Great job, Chris
  2. Implemented full Transformer https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator_pressure_transformer_V6.ipynb. However, V6 doesn't converge as Transformer-Encoder.
  3. Analyze predict https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/Analyze_predict_data.ipynb.
     1. Mean of MAE has long tail. It's normal.
     2. No zero error, 这就意味可以尝试用分类来减少误差
     3. When pressure changes dymatically, it cannot predict well.
     4. Different RC has different error distribution
     20-10 - MAE  0.15342606138845297, count: 184106
         - 20-20 - MAE  0.16133572280868774, count: 185841
         - 20-50 - MAE  0.16003437620608627, count: 243184
         - 5-10 - MAE  0.15912988705787456, count: 249386
         - 5-20 - MAE  0.11116782628501401, count: 255571
         - 5-50 - MAE  0.11376304575751983, count: 245700
         - 50-10 - MAE  0.16271191963964562, count: 418114
         - 50-20 - MAE  0.2426435067746749, count: 255953
         - 50-50 - MAE  0.2504960925707726, count: 253113

What we plan to do:
   1. Data: try to use 40 instead 80 for train - Chris
   2. Add attention to learn pressure changes - Bobber

## meeting 10/21
What we did:
1. We need to run at lest one folder to test the model.
2. Bobber is adding self attention
3. Chris is doing ensambling
4. We need to check tree based model like LGB/XGBoot for big errors like RC 50-50, 50-20

## meeting 10/22
What we did:
1. Fixed "Allocate more memory issue" for Kaggle GPU.
2. created new DB to collect the best models.
3. self attention is added, https://www.kaggle.com/bobber/ventilator-gb-rescaling-eda-v8-gpu, https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator_gb_rescaling_eda_V8_GPU.ipynb
4. Previous transformer mode is not good because it's normalize all data and loss scale. We can consider to use transformer to generate weight and multiply inputs instead of add weights.

What we plan to do:
   1. blending
   2. Complete https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator_gb_rescaling_eda_V8_GPU.ipynb
   3. Check  Temporal Fusion Transformers (TFT) for Interpretable Multi-horizon Time Series Forecasting, https://arxiv.org/abs/1912.09363

## meeting 10/23
What we did:
1. Blend. PL 0.138. Good job, Chris.
2. Finding tree-based model
3. Transformer encoder didn't improve CV or PL score.

## meeting 10/24
No meeting

## meeting 10/25
What we did:
1. explain R/C, https://www.kaggle.com/c/ventilator-pressure-prediction/discussion/276599, https://www.kaggle.com/c/ventilator-pressure-prediction/discussion/276828
2. added Auto encoder. https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/ventilator_gb_rescaling_V12_GPU.ipynb
3. LightGBM model, https://github.com/bobbercheng/ventilator-pressure-prediction/blob/master/VPP_LGBM_GPU_0.4506.ipynb.

What we plan to do:
1. Fix train issues of LightGBM model
2. Change parameters for Auto encode.
3. Try to use wavenet for 50__20, 50__50

## meeting 10/27
What we did:
1. Wavenet archives very good CV for 50__20, 50__50 but it doesn't improve public score. Wavenet has bad CV for fold#1 if whole train is used. It may be caused by that LSTM part is too powerful and LSTM part has fixed weight trained from fold #1.

What we plan to do:
1. Run LSTM model with TPU fully with 66 features. It will give us base-model with 7 fold
2. Retain LSTM and Wavenet together instead of fixing LSTM weight.
3. Retain LSTM and Wavenet from begin.

## meeting 10/28
What we did:
1. Run LSTM model with TPU fully with 66 features. It will give us base-model with 7 fold. In progress.
2. Retain LSTM and Wavenet together instead of fixing LSTM weight. No improve. CV 0.161
3. Fixed LSTM to the -4 layer instead of -3 layer, no improve. Fixed LSTM to the -3, CV 1.59xxx, a little bit improvement.
4. Change loss function to MeanSquaredError, no improve.
5. Retain LSTM and Wavenet from begin. no improve. CV 0.163
6. Increase loss of RC 50__50, 50__20 for LSTM/Wavenet model. No obvious change for both train from scratch or fix LSTM parts.
7. Increase loss of RC 50__50, 50__20 for LSTM/Transformer_encoder
