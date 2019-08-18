---
layout: post
comments: true
title:  "psutil 설치 실패: CoreFoundation.h file not found"
date:   2019-08-17
tags: python
---

# psutil 설치 실패: CoreFoundation.h file not found

`pip install psutil`이 실패했다.
```
fatal error: 'CoreFoundation/CoreFoundation.h' file not found
```
진짜 없는지 찾아보니 아래와 같다.
```
$ sudo find / -name "CoreFoundation.h"
   /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/CoreFoundation.framework/Versions/A/Headers/CoreFoundation.h
```
일반적으로 Framework관련 문제가 생기면

- Command Line Tools가 없거나
- 있어도 설정경로 업데이트가 필요한 경우이다.
```
$ xcode-select --install
또는
$ xcode-select --switch /Library/Developer/CommandLineTools
또는
$ xcode-select --reset
```
위처럼 하면 대부분 해결되는데, 이번에는 아니었다.

그래서 강제로 include참조 시켜보았다.
```
cd /usr/local/include
ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/CoreFoundation.framework/Versions/A/Headers ./CoreFoundation
# 위에서 기록하지 않았지만, <IOKit/LibIOKit.h>에 대한 문제도 발생했었으므로, 같이 처리한다.
ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/IOKit.framework/Versions/A/Headers ./IOKit
```
이후, `pip install psutil`을 수행하자, 성공했다!

우아하게 해결하지 못한 생각이 들지만, 마땅히 좋은 해결책이 떠오르지 않았다. 원인을 생각해보자면,

- <CoreFoundation/CoreFoundation.h> 를 참조한 것을 미루어보아,
    - Mojave 이후로 CoreFoundation 링크위치가 변했다.
    - 또는, psutil의 include경로가 업데이트되지 않았다.
- 시스템을 업그레이드 하면서 symbolic link가 깨졌다.

정도가 될 것 같다. 우아한 해결방법이라면,

- Xcode(Command Line Tools)를 클린설치해본다(?)

다음번엔 맥북도 클린설치를 해볼까 하는 생각이 든다.