---
layout: post
title: Stacked Attention Networks for Image Question Answering
category: Review Papers
tag: [Image QA, Stacked Attention]
---

# Stacked Attention Networks for Image Question Answering



## 논문 정보

 Yang, Zichao, et al. "Stacked attention networks for image question answering." *Proceedings of the IEEE conference on computer vision and pattern recognition*. 2016. 



## 개요

Stacked Attention Networks를 고안하여 Image Question Answering Architecture에 적용하였음.

Question의 semantic representation를 이용하여 정답과 관련된 이미지의 region을 찾아내는 것이 핵심.

Image QA의 정답 추론과정은 여러단계를 거쳐 이뤄지는데, 해당 논문에서는 multi-layer SAN을 이용해서 정답과 관련되는 이미지의 region을 점진적으로 추려감. 1st step: 5 regions -> 2nd step: 3 regions -> 3rd step: 1 region 이런식으로.

해당 논문에서는 4개의 Dataset (DAQUAR-ALL, DAQUAR-REDUCED, COCO-QA, VQA)으로 실험을 진행하였고, 결과적으로는 당시의 state-of-the-art approaches에 비해 적게는 0.2% 많게는 9.7%의 성능향상을 보여줌.



## 서론

- 배경지식

  - Image Question Answering

    자연어만을 이용한 Question Answering과 달리, 컴퓨터비전 기술과 자연어처리 기술을 같이 사용하는 인공지능의 세부분야 중 하나이다. 어떤 이미지가 주어지고 이미지의 내용과 관련된 질문을 하면 기계는 질문에 대한 정답을 내놓는다. 

    ![vqa sample](/assets/img/vqa sample image.PNG)*Image Question Answering sample  출처: https://github.com/facebookresearch/pythia*

    

  

- 연구동기

  최근의 Image QA model들은 인공신경망을 이용하여 연구되고 있다. 

  일반적으로 Convolution Neural Network (CNN)을 이용하여 이미지에서 global image feature vector를 추출하고, Long Short-Term Memory Network (LSTM)을 이용하여 질문에서 question feature vector를 추출하여 이 둘을 합친 후 정답을 추론하는 데 사용하도록 하는 구조를 보여준다. 

  당시의 image QA model들은 정답이 이미지의 세밀한 영역(fine-grained region)과 관련될 경우에 좋은 않은 성능을 보여왔다.

  해당 논문에서는 image captioning과 machine translation에서 활용되었던 attention mechanism을 image feature와 question feature를 합친 후 정답을 추론하는 과정에 적용하였다. 추론과정이 multi-step을 갖도록하여 단계를 진행할수록 정답과 관련되는 region을 추리도록 하였다.

  해당 논문에서 제안한 이 방법론을 Stacked Attention Networks (SANs)라 한다.



## 본론

- Stacked Attention Networks

  - Overview

    SANs는 image model, question model, stacked attention model로 구성된다.

    ![SANs overall architecture](/assets/img/SANs overview.PNG)*SANs overall architecture  출처: 본 논문*

    

    

  - Image model

    CNN을 사용하여 이미지에서 high level image representations를 추출한다. 이미지에서 14 x 14 개의 region을 각각 feature vector 형태로 추출한다.

  - Question model

    해당 논문에서는 CNN을 이용한 방식과 LSTM을 이용한 방식 두가지를 실험한다. 이 두 네트워크를 이용하여 질문의 semantic feature vector를 추출한다.

  - Stacked Attention model

    이 부분에서는 multi-step 추론을 통해 정답과 관련된 image region을 파악한다. 여러개의 attention layer가 stack의 형태로 쌓여있으며, 각 attention layer에서는 전 단계의 attention layer에서 넘어온 question vector을 가지고 이와 관련된 image feature vector를 찾는다. 그리고 question vector와 image vector를 결합하여 좀더 정제된 question vector를 만들어 그 다음 단계의 attention layer에 전달한다. 가장 마지막 단계의 attention layer에는 제일 정답과 관련된 image region을 알 수 있는 attention distribution이 담긴 question vector가 존재한다. 이것을 image feature vector와 결합하여 정답을 찾는 것에 사용한다.

  

- 실험 및 결과

  ![SANs sample](/assets/img/SANs sample.PNG)*"What are sitting in the basket on a bicycle?" 질문에 대한 stacked attention model 각 layer에서의 결과 이미지  출처: 본 논문*

  

  

## 결론


