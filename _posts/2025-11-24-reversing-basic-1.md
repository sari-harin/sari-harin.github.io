---
title:  "<리버싱 핵심 원리> Hello World! 리버싱"
excerpt: "리버스 엔지니어링 기초"

writer: sari

categories:
  - 리버스 엔지니어링
tags:
  - [입문, 리버스 엔지니어링, 보안, 리버싱 핵심 원리]

toc: true
toc_sticky: true
 
date: 2025-11-24
last_modified_at: 2025-11-24
---

# 1. Hello World! 리버싱
## 1 / HelloWorld.exe 디버깅

다음의 프로그램을 Release 모드로 빌드하고, OllyDbg로 열어보자.

```cpp
#include "windows.h"
#include "tchar.h"

int _tmain(int argc, _TCHAR* argv[])
{
	MessageBox(NULL,
		L"Hello, World!",
		L"www.reversecore.com",
		MB_OK);

	return 0;
}
```

### OllyDbg 화면
![img](https://github.com/sari-harin/blog-imgs/blob/main/reversing-1.png?raw=true)
1. **Code Window:** Disassembly code를 보여주며 loop, jump 위치 등의 정보를 표시
2. **Register Window:** CPU Register 값을 표시
3. **Dump Window:** 프로세스에서 원하는 memory 주소 위치를 표시
4. **Stack Window:** ESP Register가 가리키는 프로세스의 stack memory를 표시

### EP란?
EntryPoint: HelloWorld.exe의 실행 시작 주소를 가리킨다.