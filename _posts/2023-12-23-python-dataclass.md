---
layout: post
title: dataclasses — 데이터 클래스
date: 2023-12-22 21:01:00
description: an example of a blog post with some code
tags: python
---

프로그래밍에서 `interface`를 잘 설계하는 것은 코드의 가독성과 유지 보수성을 높일 수 있다. 

Python 3.7 이상에서 사용할 수 있는 `dataclass`는 데코레이터를 통해 클래스 선언을 간소화할 수 있는데 또한 `interface` 로서의 역할에도 매우 좋다. 요즘 유행하는 [pydantic](https://docs.pydantic.dev/latest/)도 이런 dataclass 에 validation 을 할 수 있는 패키지중 하나이다.

## Dataclass의 기본 사용법

C++의 구조체와 유사한 방식으로, Python에서도 간단한 데이터 구조를 빠르게 정의할 수 있는데 x,y 를 가지는 `Point` 클래스의 예를 만들어보자.

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: float
    y: float
```

`dataclass` 데코레이터는 `__init__`, `__repr__`, `__eq__` 등의 메서드를 자동으로 추가해준다.
그렇기 때문에 인스턴스를 출력하면

```python
>>> print(Point(3,4))
Point(x=3, y=4)
```

다음과 같이 깔끔하게 포맷된 결과가 출력된다.


## `transformers` 예시

오픈소스 모델들을 쉽게 이용할 수 있는 레포인 [transformers](https://github.com/huggingface/transformers) 에서도 `Argument` 와 모델의 인풋 아웃풋와 같은 interface 를 모두 `dataclass` 로 정의해서 사용하고있다.

예를 들어 거의 모든 언어모델들의 아웃풋은 [`BaseModelOutput`](https://github.com/huggingface/transformers/blob/29e7a1e1834f331a4916853ecd58549ed78235d6/src/transformers/modeling_outputs.py#L25)를 상속해서 쓰는데 `BaseModelOutput` 를 보면

```python
@dataclass
class BaseModelOutput(ModelOutput):
    """
    ...
    """

    last_hidden_state: torch.FloatTensor = None
    hidden_states: Optional[Tuple[torch.FloatTensor]] = None
    attentions: Optional[Tuple[torch.FloatTensor]] = None

```

와 같이 `BaseModelOutput` 의 아웃풋은 보통의 `transformer` 구조에서 나올 수 있는 아웃풋들을 포함하는것을 알 수 있다. 이런식으로 Interface 를 datalcass 로 설계하면 우선 코드가 매우 간결해진다. 만약 ABC 클래스로 부터 이런 interface 를 만든다고 한다면 `__init__` 부터 여러가지 설계해야할점이 너무나 많고 코드가 방대해진다. 





