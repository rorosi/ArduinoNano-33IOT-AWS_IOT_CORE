# Arduino NANO 33 IOT 보드 설치

아두이노 실행파일을 열어 메뉴 > 툴 > 보드 > 보드매니저를 실행한다.

![222](https://user-images.githubusercontent.com/56014938/132097584-ecdeb80c-e393-4d09-87fd-eaf713c81a7e.png)

보드매니저 검색창에서"NANO 33"이라고 검색하고 'Arduino SAMD Boards (32-bits ARM Cortex-M0+)'를 확인하고 설치한다.

![2](https://user-images.githubusercontent.com/56014938/131636574-a5411c05-9451-4eff-867c-8a4a5f25b6cc.png)


보드를 설치하고 다시 보드 옵션을 확인해보면 Arduino SAMD 하위에 Arduino NANO 33 IoT 보드를 확인할 수 있다. NANO 33 IoT를 선택하여 보드를 설정한다.

![555](https://user-images.githubusercontent.com/56014938/132097603-8edc673b-412f-45e5-b585-8ac445bb01c3.png)

드라이버 설치가 완료되면 윈도우에서는 장치 관리자에서 포트를 확인하여 설정할 수 있고, 리눅스나 맥에서는 장치 정보에서 보드가 인식되었는지 확인할 수 있다. 아두이노 개발환경에서 포트를 설정한다. 리눅스나 맥에서는 장치 정보에서 보드가 인식되었는지 확인할 수 있다.

![666](https://user-images.githubusercontent.com/56014938/132097617-7ea17970-f007-48a4-8582-28b33213cf42.png)

간단한 예제를 아두이노 나노 33 IoT에 업로드 해보자. IDE에서 파일 - 예제 - 01.Basics - Blink 예제를 사용한다. 예제를 불러오고 아두이노에 업로드하여 동작을 확인한다. 아두이노 나노 33 IoT에 'LED_BUILTIN'핀은 D13번 핀이다. 신호 단자에서 3.3/0V로 신호가 나오면서 나노 33 IoT의 주황색 LED가 점멸하는 것을 확인한다. 여기까지 확인하였다면 개발 환경을 성공적으로 구축하였다.
