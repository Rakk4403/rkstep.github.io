---
layout: post
comments: true
title:  "Blockchain의 기본구조 구현해보기"
date:   2018-06-03
tags: blockchain python
---

블럭체인에 대한 내용을 PPT정도로 접했기 때문에, 자세한 사항은 모른상태였는데, 모처럼 노닐다가 아래의 포스팅을 보았다.

http://ecomunsing.com/build-your-own-blockchain

최근에 `PEP 484`를 적용해보려고 기웃기웃 거리던 참에, 블럭체인의 기본적인 구현도 따라해보고 새로운 문법도 적용해보고자 [프로젝트](https://github.com/Rakk4403/blockchain-basic-practice)를 생성했다.

## 블럭체인 기본구조

예제의 블럭체인을 간단하게 정리해보자면, 아래와 같다.
* 현재 블럭의 Content를 Hash input으로 Hash value를 생성하고, 현재 블럭의 Hash값으로 사용한다.
* 현재 블럭의 Content는 다음 요소들을 포함한다.
  * 현재 블럭의 순서번호
  * 이전 블럭의 Hash value
  * 현재 블럭의 트랜잭션 수
  * 현재 블랙의 트랜잭션 목록

위 내용을 토대로 블럭을 생성하고, 재귀적으로 이용하여 체인을 형성한다. 코드결과를 출력해보면 아래와 같은 내용을 볼 수 있다.

```python
# genesis block(제일 첫 블럭)
{'contents': {'block_number': 0,  # 순서번호
              'parent_hash': None,  # 이전 hash value
              'txn_count': 1,  # 트랜잭션 수
              'txns': [{'Alice': 50, 'Bob': 50}]},  # 트랜잭션 목록
 'hash': '68c7fda20371bd94a5a8d10e80c622cf28aa8f53a58b803e88a024107d6ee475'}  # 현재 블럭에서 생성된 hash value
# 두번째 블럭
{'contents': {'block_number': 1,
              'parent_hash': '68c7fda20371bd94a5a8d10e80c622cf28aa8f53a58b803e88a024107d6ee475',
              'txn_count': 5,
              'txns': [{'Alice': 2, 'Bob': -2},
                       {'Alice': 3, 'Bob': -3},
                       {'Alice': -1, 'Bob': 1},
                       {'Alice': 2, 'Bob': -2},
                       {'Alice': 3, 'Bob': -3}]},
 'hash': '0797b05c4a731e5ca48f6b8ceea18dd5548c1320085b92d8a176ba5886264bf3'}
```

두 블럭이 연속된 체인이고, `contents`를 토대로 현재 블럭의 hash value가 정해지기 때문에, 위변조가 어렵다는 것이다. 개념으로만 듣다가 코드로 보니 좀 더 명확하게 정리할 수 있어서 좋았다.

## Python 환경설정(PEP 484) in Emacs

[귀도 형의 트윗](https://twitter.com/gvanrossum/status/1001869119937961984)을 보다가 `mypy`의 존재를 알게 되었는데, Emacs에서도 `flycheck-mypy`를 사용할 수 있었다! 게다가 [Wiki도 이미 친절하게 코드조각](http://wikemacs.org/wiki/Python#MyPy_checks)이 있어서 편하게 적용해볼 수 있었음 ㅎㅎ
