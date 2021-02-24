---
title:  "[CentOS] VM Virtualbox로 CentOS 7 설치"
excerpt: "VM Virtualbox로 CentOS 7 설치 순서 정리"

categories:
  - CentOS
tags:
  - [VM Virtualbox, CentOS]

toc: true
classes: wide

sidebar:
  nav: "docs"

date: 2021-02-24
last_modified_at: 2021-02-24
---

### 1. CentOS 7 다운
---
CentOS파일을 다운로드 받습니다.

![CentOS다운](/imgsrc/vm_CentOS7/0.JPG)

### 2. 가상머신 설정 과정
---
Oracle VM VirtualBox를 실행 후 새로 만들기를 클릭합니다.

![설치](/imgsrc/vm_CentOS7/1.JPG)

이름을 작성하고 종류를 Linux의 Red Hat으로 설치합니다.<br>
64비트가 안보일 시 BIOS의 가상화 설정을 체크하세요.

![설치](/imgsrc/vm_CentOS7/2.jpg)

메모리를 설정합니다.<br>
원하시는 크기만큼 설정하면 됩니다.

![설치](/imgsrc/vm_CentOS7/3.JPG)

하드디스크를 설정합니다.<br>
새 가상 하드 디스크 만들기로 선택해주세요.

![설치](/imgsrc/vm_CentOS7/4.JPG)

하드 디스크 파일 종류를 선택합니다.<br>
저는 VDI(VirtualBox 디스크 이미지)를 선택했습니다.

![설치](/imgsrc/vm_CentOS7/5.JPG)

파일 위치 및 크기를 설정합니다.<br>
저는 저장공간을 50GB로 설정해줬습니다.

![설치](/imgsrc/vm_CentOS7/6.JPG)

마지막으로 만들기를 누르면 다음과 같은 화면으로 나오실 겁니다.<br>
이제 설정을 클릭해주세요.

![설치](/imgsrc/vm_CentOS7/7.JPG)

우선 시스템에서 포인팅 장치를 'USB 태블릿'으로 변경해줬습니다.

![설치](/imgsrc/vm_CentOS7/8.JPG)

다음으로 디스크를 선택해줍니다.<br>
컨트롤러:IDE에서 속성에 있는 디스크 아이콘을 클릭 후 다운로드 받은 CentOS7의 iso파일을 선택해줍니다.

![설치](/imgsrc/vm_CentOS7/9.JPG)

마지막으로 네트워크에서는 어댑터 2개를 다음과 같이 설정해줬습니다.

![설치](/imgsrc/vm_CentOS7/10.jpg)
![설치](/imgsrc/vm_CentOS7/11.jpg)

3. CentOS 설치 과정
---
![설치](/imgsrc/vm_CentOS7/설치1.JPG)
![설치](/imgsrc/vm_CentOS7/설치2.JPG)
![설치](/imgsrc/vm_CentOS7/설치3.JPG)
![설치](/imgsrc/vm_CentOS7/설치4.JPG)
![설치](/imgsrc/vm_CentOS7/설치5.JPG)
![설치](/imgsrc/vm_CentOS7/설치6.JPG)
![설치](/imgsrc/vm_CentOS7/설치7.JPG)
![설치](/imgsrc/vm_CentOS7/설치8.JPG)
![설치](/imgsrc/vm_CentOS7/설치9.JPG)
![설치](/imgsrc/vm_CentOS7/설치10.JPG)
![설치](/imgsrc/vm_CentOS7/설치11.JPG)
![설치](/imgsrc/vm_CentOS7/설치12.JPG)
![설치](/imgsrc/vm_CentOS7/설치13.JPG)

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```