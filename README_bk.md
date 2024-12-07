# OC_SBC_Audio

# 준비물
- Raspberry Pi 4
- USB 오디오 장치   
- 3.5mm 오디오 스피커 
- 3.5mm 오디오 마이크  
- 모니터
- 키보드 
- 마우스

# 작업 순서
1. USB 오디오 장치 연결
2. USB 오디오 장치 확인
```bash
arecord -l
``` 
3. USB 오디오 장치 확인
```bash
**** List of CAPTURE Hardware Devices ****
card 1: Microphone [Yeti Stereo Microphone], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```
card 와 device 번호를 확인한다.
4. alsa를 위한 설정 파일 생성
```bash
sudo nano /home/pi/.asoundrc
```
5. 설정 파일에 아래 내용 추가
```bash
pcm.!default {
  type asym
  capture.pcm "mic"
}
pcm.mic {
  type plug
  slave {
    pcm "hw:[card number],[device number]"
  }
}
```
card number와 device number를 위에서 확인한 번호로 변경한다.
6. 명령어로 녹음해보기  
```bash 
arecord --format=S16_LE --rate=16000 --file-type=wav out.wav
```
7. 녹음된 파일 재생해보기
```bash
aplay out.wave
```

8. gain을 조절하고 싶다면 alsamixer를 이용한다.
```bash
alsamixer
```
![alsamixer](https://pimylifeup.com/wp-content/uploads/2020/04/Raspberry-Pi-AlsaMixer-Microphone-settings.png)





# 응용
1. pyaduio 설치 
```bash
sudo apt-get update 
sudo apt-get upgrade 
sudo apt-get install portaudio19-dev 
sudo pip install pyaudio
```


# 참고 
- [Using a Microphone with a Raspberry Pi](https://pimylifeup.com/raspberrypi-microphone/)
