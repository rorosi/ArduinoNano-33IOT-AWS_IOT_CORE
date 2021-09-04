# Arduino nano 33 IOT - WiFi 연결

Nano33 IoT보드에서 WiFi를 사용하기 위해서는 "WiFiNINA"라이브러리가 필요합니다.

라이브러리 관리에서 "WiFiNINA"라이브러리를 받아줍니다.

![777](https://user-images.githubusercontent.com/56014938/132098121-e48f8ea4-f4f7-49ab-8895-2ef4d7ef62f9.png)
![888](https://user-images.githubusercontent.com/56014938/132098158-f7836dd1-30f0-4334-984b-e7e1a3a8fecd.png)

이제 WiFi에 접속하는 소스코드에 대해 알아보도록 하겠습니다. 
nano33_IoT_WiFi 파일을 다운 받고 업로드 한 뒤 시리얼 모니터를 확인하면 아래 그림과 같이 접속을 확인할 수 있습니다.

![999](https://user-images.githubusercontent.com/56014938/132098163-523841ec-0ae9-4d7f-9638-de64b4610df0.png)

보시는 바와 같이 연결하고자 하는 ssid를 표시하고 연결에 성공하면 "you're connected to the network" 라는 메시지를 띄워주고 와이파이의 상태정보를 출력해줍니다.
