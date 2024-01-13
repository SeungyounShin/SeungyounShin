---
layout: distill
title: LLM이 스스로 발전할 수 있을까?
description: dd
tags: distill formatting
giscus_comments: false
date: 2023-12-30
featured: false

authors:
  - name: 신승윤


bibliography: 2018-12-22-distill.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Draft
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---

## Draft

### Self Correction
[**Reflexion**](https://ar5iv.labs.arxiv.org/html/2303.11366)

너무나도 유명한 논문이다. 이전에 틀렸으면 다시 반추(Reflection) 하면 원래 결과보다 계속 잘해지게 된다는 내용인데 현실적으로는 적용하기 어렵다. 왜냐하면, 우선 해당 논문에서는 Oracle Feedback 을 받는다. 예를 들어 HumanEval 과 같은 코딩문제에서는 Programming Judge 로 부터 맞았는지 틀렸는지를 알수가 있다. 밑에 **Large Language Models Cannot Self-Correct Reasoning Yet** 에서는 이런 Error Singal 을 흘려주는 것은 진정한 Self Correction이 아니라고 말하고 있고 나도 이 주장이 상당히 신빙성이 있다고 느껴진다. 하지만, 처음 Reflection 이라는 개념을 제안했다는 측면에서 꽤 의미 있는 논문인것 같다. 여담이지만 Neuirps 에 갔을 때 포스터에 저자가 와서 물어봤었다 ㅎㅎ 저자도 뭔가 다음 스텝을 생각하는 느낌이었다. 왜냐면 결국 성능이 Saturated 된다. 특정 성능 이상은 모델의 개별성능을 넘을 수 없다는것이다. 인간도 계속 생각하다 보면 어떤 문제는 풀 수 있지만 그래도 어떻게든 못푸는 문제가 존재하는것과 동일하다고 생각이된다. 

[**Large Language Models Cannot Self-Correct Reasoning Yet**](https://arxiv.org/abs/2310.01798)

몰랐는데 이 논문이 ICLR2024에 Submit 되었다 결과는 어떻게 될지 모르겠지만 이 당시에는 Self Correction 하는 방법론들이 우후죽순 나오고있던 때라 Self Correction 이 단순히 Accuracy 가 올라가는것만 보면안되고 맞았다가 틀려진 것과 틀렸는데 맞아진것을 같이 보면 결국은 LLM이 아직은 Self Correcting 을 못한다는 내용이다. 근데 실제 해보면 논문의 말처럼 맞았는데 틀려지는게 꽤 된다.

### Synthetic Data Self-Training

[**weak to strong(OpenAI)**](https://cdn.openai.com/papers/weak-to-strong-generalization.pdf)

많은 사람들이 대체 왜 그냥 Strong 모델을 훈련시키기 않냐고 비난을 받는 논문인데. 그치만 가만 생각해보면 이 세팅은 다른 어떤 논문보다 Synthetic data 를 진지하게 다루고 있다고 생각한다. 절대 이 논문의 세팅은 strong 모델을 제한하려는 방향이 아니다. weak LLM의 아웃풋 (즉, 사람이 될수도 있다) 을 가지고 이것을 $P_\text{data}$ 로 이용한다면, 결국 SFT를 하든 뭐를 하든 Global Maximum 인 사람 그 자체를 넘어서지 못한다. 근데 해당 논문에서는 이를 넘어설수 있음을 보였다. 하지만 아직 오점이 많다. 논문에서 이야기하는 strong LLM 이 사람을 넘어서는 것은 아니다 ($\texttt{MMLU}  \le 90\%$) 즉, 사람을 넘어서는 아주 새로운 synthetic data가 아니라 $P_\text{data}$ 를 가지고 훈련된 underfitting 된 모델 끼리의 weak-to-strong 을 논의하고 있다. 

[**Reinforced Self-Training (ReST) for Language Modeling**]()

Grow Step 과 Improving Step 으로 나뉘어 스스로 성장하는 LLM을만드는것인데 translation 에서만 실험을 한것이 아쉽다.


[**Beyond Human Data: Scaling Self-Training for Problem-Solving with Language Models**](https://ar5iv.labs.arxiv.org/html/2308.08998)

사람을 넘기위해서는 사람의 데이터를 모사하면 안됨. Synthetic Data 를 만들고 Scalar Feedback 을 만들어서 이걸 가지고 단순히 MMLU나 TriviaQA와 같은 언어쪽 도메인이 아니라 정말 Reasoning 이 필요한 MATH, Code(APPS, HumanEval) 에서의 정확도 향상을 보이는 논문임.

[**Improving Large Language Model Fine-tuning for Solving Math Problems**](https://ar5iv.labs.arxiv.org/html/2310.10047)

Pass@1 하고 Pass@K 즉 LLM 에서 여러번 Sampling 하면 성공하는 비율과 단번에 풀어버리는 비율이 수학이나 코딩쪽에서 꽤 많이 차이가 난다. 이걸 개선하는 논문인듯


[**Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models**](https://arxiv.org/abs/2401.01335)

RL로 LLM을 튜닝하는 방법은 Preference 를 필요로한다. SFT 세팅에서 같은 모델($\theta$)를 Generator, Discriminator로 생성해보고 $P_\text{data}$ 인 보통은 인간이나 GPT4가 생성한 데이터와 가까워지고 본인이 이전에 생성한것 과는 멀어지는 방향으로 학습하게된다. 수학적 수식을 통해 훈련 Loss 를 간단하게 도출한 점이 매우 훌룡한 것 같다. 