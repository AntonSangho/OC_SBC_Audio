# 🎬 공학도서관 오리지널 컨텐츠

# 🎙️ 라즈베리파이 음성녹음기 만들기

## 📝 프로젝트 소개
이 프로젝트는 라즈베리파이와 USB 사운드카드를 활용하여 음성녹음기를 만드는 과정을 설명합니다. 리눅스의 arecord 명령어를 사용하여 음성을 녹음하고 재생하는 방법을 배울 수 있습니다.

## 📚 사전학습
이 프로젝트를 시작하기 전에 아래 내용을 먼저 공부하고 오시면 좋아요.

- 라즈베리파이 기초
  - 터미널 사용법
  - 리눅스 기본 명령어
  - nano 에디터 사용법

- 리눅스 오디오 시스템
  - ALSA 시스템 이해
  - 오디오 장치 개념
  - 사운드카드 설정 방법

## 🛠 준비물
- 라즈베리파이 4
- [USB 사운드카드](https://www.coupang.com/vp/products/7385257424?itemId=19082120502&vendorItemId=86204453520&src=1042503&spec=10304025&addtag=400&ctag=7385257424&lptag=10304025I19082120502V86204453520&itime=20241207214919&pageType=PRODUCT&pageValue=7385257424&wPcid=17322756292575334962775&wRef=&wTime=20241207214919&redirect=landing&gclid=Cj0KCQiAgdC6BhCgARIsAPWNWH2DjF-Ix3_BathFJO5cjvngrNWVf-o5w_qE6KtEfy6BNueDt1IQGWkaAtK9EALw_wcB&mcid=f50cfb496fcd4558af00dc6e3221ed1b&campaignid=21307631972&adgroupid=)
- 마이크
- 스피커 또는 이어폰
- USB 전원 어댑터
- SD 카드

## 💾 실습 코드
### 1. 오디오 장치 설정 파일 (.asoundrc)

```bash
pcm.!default {
    type asym
    capture.pcm "mic"
}

pcm.mic {
    type plug
    slave  {
        pcm  "hw:3, 0"  # hw:카드번호,장치번호
      }
}
```

> **주의**: 'hw:3, 0'에서 3은 카드 번호입니다. 실제 사용하는 사운드카드의 번호로 변경해야 합니다.

## 🚀 시작하기
1. 오디오 장치 확인하기
   ```bash
   arecord -l
   ```
   - USB 사운드카드 연결 전에는 장치가 표시되지 않습니다
   - 연결 후 카드 번호와 장치 번호를 메모해두세요

2. USB 사운드카드 연결
   - USB 사운드카드를 라즈베리파이에 연결
   - 마이크를 사운드카드 마이크 단자에 연결
   - 스피커를 사운드카드 스피커 단자에 연결

3. 사운드카드 설정
   ```bash
   nano ~/.asoundrc
   ```
   - 위의 설정 코드를 복사하여 붙여넣기
   - 카드 번호와 장치 번호를 확인한 번호로 수정
   - Ctrl + S로 저장, Ctrl + X로 종료

4. 음성 녹음하기
   ```bash
   arecord --format=S16_LE --rate=16000 --file-type=wav out1.wav
   ```
   - --format=S16_LE: 16비트 리틀 엔디안 포맷으로 녹음
   - --rate=16000: 16kHz 샘플링 레이트 설정
   - --file-type=wav: WAV 파일 형식으로 저장
   - out1.wav: 저장될 파일 이름
   - 녹음 종료: Ctrl + C

5. 녹음 파일 재생하기
   ```bash
   aplay out1.wav
   ```
   - 연결된 스피커나 이어폰으로 녹음된 음성 확인
   - 볼륨이 너무 작거나 크면 다음 단계로 진행

6. 볼륨 조절하기
   ```bash
   alsamixer
   ```
   - F6 키를 눌러 USB PnP Sound Device 선택
   - 방향키로 조절하려는 항목 선택
   - 위/아래 방향키로 볼륨 조절
   ![alsamixer](https://camo.githubusercontent.com/d5204d698762182d3f4c145030d6ef746d384441a8000cc5ab0171090eb3cb1e/68747470733a2f2f70696d796c69666575702e636f6d2f77702d636f6e74656e742f75706c6f6164732f323032302f30342f5261737062657272792d50692d416c73614d697865722d4d6963726f70686f6e652d73657474696e67732e706e67)
   - ESC 키로 종료

## 🔍 문제해결
- 녹음이 안돼요
  - USB 사운드카드가 제대로 연결되었는지 확인해보세요.
  - `arecord -l` 명령어로 사운드카드가 인식되는지 확인해보세요.
  - .asoundrc 파일의 카드 번호가 맞는지 확인해보세요.

- 소리가 너무 작아요
  - `alsamixer`에서 마이크 볼륨을 확인해보세요.
  - 마이크가 사운드카드에 제대로 연결되었는지 확인해보세요.
  - 마이크와 입력 소리의 거리를 확인해보세요.

- 재생이 안돼요
  - 스피커나 이어폰이 제대로 연결되었는지 확인해보세요.
  - `alsamixer`에서 스피커 볼륨을 확인해보세요.
  - 녹음 파일이 제대로 저장되었는지 확인해보세요.

## 🌟 이렇게 업그레이드 해볼 수 있어요
- 음성 메모 프로그램 만들기
  - 날짜와 시간을 파일명에 자동으로 넣기
  - 버튼으로 녹음 시작/종료하기
  - 녹음 파일 자동 정리하기

- 음성 품질 개선하기
  - 노이즈 제거 필터 추가하기
  - 다양한 오디오 포맷 지원하기
  - 녹음 레벨 자동 조절하기

- 추가 기능 개발하기
  - 음성을 텍스트로 변환하기
  - 웹 인터페이스 추가하기
  - 원격 녹음 기능 추가하기

## 📚 참고 자료
- [ALSA 프로젝트 문서](https://www.alsa-project.org/wiki/Documentation)
- [arecord 명령어 설명서](https://linux.die.net/man/1/arecord)
- [라즈베리파이 오디오 설정 가이드](https://www.raspberrypi.org/documentation/computers/os.html#audio-configuration)

> **주의사항**: 
> - 녹음 전 항상 마이크 볼륨을 확인하세요.
> - 중요한 녹음은 반드시 테스트 후 진행하세요.
> - 오디오 설정 파일 수정 시 백업을 먼저 해두세요.
> - USB 사운드카드는 전원을 충분히 공급받을 수 있는 포트에 연결하세요.


