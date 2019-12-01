---
layout: post
title: [Review Paper] Stacked Attention Networks for Image Question Answering
category: Review Papers
tag: [Image QA, Stacked Attention]
---

# Stacked Attention Networks for Image Question Answering



## Paper Information

 Yang, Zichao, et al. "Stacked attention networks for image question answering." *Proceedings of the IEEE conference on computer vision and pattern recognition*. 2016. 



## Abstract

Stacked Attention Networks를 고안하여 Image Question Answering Architecture에 Attention Mechanism을 적용하였음.

Question의 semantic representation를 이용하여 정답과 관련된 이미지의 region을 찾아내는 것이 핵심.

Image QA의 정답 추론과정은 여러단계를 거쳐 이뤄지는데, 해당 논문에서는 multi-layer SAN을 이용해서 정답과 관련되는 이미지의 region을 점진적으로 추려감. 1st step: 5 regions -> 2nd step: 3 regions -> 3rd step: 1 region 이런식으로.

해당 논문에서는 4개의 Dataset (DAQUAR-ALL, DAQUAR-REDUCED, COCO-QA, VQA)으로 실험을 진행하였고, 결과적으로는 당시의 state-of-the-art approaches에 비해 적게는 0.2% 많게는 9.7%의 성능향상을 보여줌.



## Introduction

- Background Knowledge

  - Image Question Answering

    자연어만을 이용한 Question Answering과 달리, 컴퓨터비전 기술과 자연어처리 기술을 같이 사용하는 인공지능의 세부분야 중 하나이다. 어떤 이미지가 주어지고 이미지의 내용과 관련된 질문을 하면 기계는 질문에 대한 정답을 내놓는다. 

    ![vqa sample](/assets/img/vqa sample image.PNG){: width="100%" height="100%"}*Fig1. Image Question Answering sample*

  - Image Captioning

    어떤 이미지가 주어지면 그 이미지에 대한 자연어 설명을 만들어내는 것을 말한다. CNN으로 high level image feature vector를 추출하여 RNN에 input으로 넣으면 자연어 설명 텍스트가 output으로 나온다.

  - Attention Mechanism

    image captioning과 machine translation에서 활용되는 메커니즘이다. image captioning에서는 이미지의 특정부분에 집중하여 더 자세하게 이미지를 묘사하는 데에 쓰이고, machine translation에서는 어떤 단어를 번역할 때 그 단어에 주목하여 번역이 진행되도록 하는 데 쓰인다.

  

- Research Motivation

  최근의 Image QA model들은 인공신경망을 이용하여 연구되고 있다. 

  일반적으로 Convolution Neural Network (CNN)을 이용하여 이미지에서 global image feature vector를 추출하고, Long Short-Term Memory Network (LSTM)을 이용하여 질문에서 question feature vector를 추출하여 이 둘을 합친 후 정답을 추론하는 데 사용하도록 하는 구조를 보여준다. 

  당시의 image QA model들은 정답이 이미지의 세밀한 영역(fine-grained region)과 관련될 경우에 좋은 않은 성능을 보여왔다.

  해당 논문에서는 image captioning과 machine translation에서 활용되었던 attention mechanism을 image feature와 question feature를 합친 후 정답을 추론하는 과정에 적용하였다. 추론과정이 multi-step을 갖도록하여 단계를 진행할수록 정답과 관련되는 region을 추리도록 하였다.

  해당 논문에서 제안한 이 방법론을 Stacked Attention Networks (SANs)라 한다.



## Body

- Stacked Attention Networks

  - Overview

    SANs는 image model, question model, stacked attention model로 구성된다.

    ![SANs overall architecture](/assets/img/SANs overview.PNG){: width="100%" height="100%"}*Fig2. SANs overall architecture*

    

    

  - Image model

    CNN을 사용하여 이미지에서 high level image representations를 추출한다. 먼저 이미지를 448 x 448 pixels로 rescale을 하고 이미지에서 14 x 14 개의 region을 각각 512 차원의 feature vector 형태로 추출하는데, spatial information을 가지고 있는 last pooling layer에서 가지고 온다. 

    ![image model](/assets/img/image model.png)*Fig3. CNN based image model*

    마지막으로 single layer perceptron을 통과시켜 각 image feature vector를 question vector의 dimension과 동일하도록 변환시켜준다.
    $$
    v_I = tanh(W_I f_I + b_I)
    $$
    *f<sub>I</sub>*는 image feature matrix

    

  - Question model

    해당 논문에서는 LSTM을 이용한 방식과 CNN을 이용한 방식 두가지를 실험한다. 이 두 네트워크를 이용하여 질문의 semantic feature vector를 추출한다.

    - LSTM based question model

      ![lstm question model](/assets/img/lstm question model.png)*Fig4. LSTM based question model*

      LSTM은 sequence의 state를 저장하는 memory cell unit을 갖는다. LSTM은 word vector를 input으로 받아 memory cell *c<sub>t</sub>* 를 업데이트 시키고 hidden state *h<sub>t</sub>*를 output으로 내놓는다. memory cell state를 업데이트하는 과정에서 gate mechanism을 이용하는데 forget gate *f<sub>t</sub>* , input gate *i<sub>t</sub>* , output gate *o<sub>t</sub>* 3종류가 있다. 

      forget gate *f<sub>t</sub>* 는 전 단계의 *c<sub>t-1</sub>* 의 정보를 memory cell에 얼마나 반영할 것인가와 관련되고,

      input gate *i<sub>t</sub>* 는 현재 input으로 들어온 *x<sub>t</sub>* 의 정보를 memory cell에 얼마나 반영할 것인가와 관련되며,

      output gate *o<sub>t</sub>* 는 현재 memory cell의 정보를 얼마나 hidden state로 내보낼지와 관련된다.
      $$
      c_t = f_t \odot c_{t-1} + i_t \odot tanh(W_{xc}x_t + W_{hc}h_{t-1} + b_c)
      $$

      $$
      h_t = o_t \odot tanh(c_t)
      $$

      이제 question vector를 생성하는 과정을 살펴보자. 먼저 question의 각 단어들을 자신의 위치에 맞는 one-hot vector *q<sub>t</sub>* 로 표현한다 (*t*는 word의 position). *q<sub>t</sub>* 는 embedding 과정을 거쳐 vector space의 embedding vector *x<sub>t</sub>* 로 변환되고 이것이 LSTM의 input으로 사용된다. 최종적으로 LSTM의 output으로 나온 *h<sub>T</sub>* (T는 마지막 word의 position)가 question의 representation vector *v<sub>Q</sub>* 가 된다.

    $$
    x_t = W_e q_{t}
    $$

    $$
    h_t = LSTM(x_t)
    $$

    $$
    v_Q = h_T
    $$

    - CNN based question model

      ![cnn question model](/assets/img/cnn question model.png)*Fig5. CNN based question model*
      
      question의 각 단어들을 자신의 위치에 맞게 one-hot vector *q<sub>t</sub>* 로 표현하고 이것을 embedding을 시켜 word embedding vector *x<sub>t</sub>* 로 변환시킨다. 그리고 word embedding vector를 concatenate 하여 question vector를 얻는다.
      $$
      x_{1:T} = [x_1, x_2, ..., x_T]
      $$
      이제 question vector에 3가지 filter를 적용하여 convolution 연산을 진행한다. 3가지 filter의 크기는 각각 1 (unigram), 2 (bigram), 3 (trigram)이다. convolution 연산의 결과는 feature map *h<sub>c</sub>*  (c는 filter size)가 되며, 이것에 max-pooling을 적용하여 'tilde *h<sub>c</sub>*' 를 구한다. filter size c에 따라 3개의 'tilde *h<sub>c</sub>*'가 구해지면 이것들을 concatenate하여 하나의 question feature representation vector *v<sub>Q</sub>* 로 만들어준다. 
      $$
      h_c = [h_{c,1}, h_{c,2}, ..., h_{c,T-c+1}]
      $$
      
      $$
      \tilde{h_c} = max[h_{c,1}, h_{c,2}, ..., h_{c,T-c+1}]
      $$
      
      $$
      h = [\tilde{h_1}, \tilde{h_2}, \tilde{h_3}]
      $$
      
      $$
      v_Q = h
      $$
      
      

  - Stacked Attention model

    이 부분에서는 multi-step 추론을 통해 정답과 관련된 image region을 파악한다. 여러개의 attention layer가 stack의 형태로 쌓여있으며, 각 attention layer에서는 전 단계의 attention layer에서 넘어온 question vector을 가지고 이와 관련된 image feature vector를 찾는다. 그리고 question vector와 image vector를 결합하여 좀더 정제된 question vector를 만들어 그 다음 단계의 attention layer에 전달한다. 가장 마지막 단계의 attention layer에는 제일 정답과 관련된 image region을 알 수 있는 attention distribution이 담긴 question vector가 존재한다. 이것을 image feature vector와 결합하여 정답을 찾는 것에 사용한다.

  

- Experiments and Results

  ![SANs sample](/assets/img/SANs sample.PNG){: width="100%" height="100%"}*Fig6. "What are sitting in the basket on a bicycle?"에 대한 stacked attention model 각 layer에서의 결과 이미지*

  

  

## Conclusion



### Reference

[Fig1] https://github.com/facebookresearch/pythia

[Fig2~6] Yang, Zichao, et al. "Stacked attention networks for image question answering." *Proceedings of the IEEE conference on computer vision and pattern recognition*. 2016. 