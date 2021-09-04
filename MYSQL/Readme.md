# 참고

[![](https://user-images.githubusercontent.com/56014938/132097012-02068278-1238-400c-8c98-97cb4fa19318.png)](https://github.com/ChuckBell/MySQL_Connector_Arduino/wiki)

## 준비사항
* MySQL 서버 및 MySQL 사용자 수정 권한
* 아두이노 nano 33 IOT or 와이파이 모듈

# MySQL 서버 설정

## 아두이노 IDE에 라이브러리 추가

아두이노 IDE에 툴 -> 라이브러리 관리에서 MySQL Connector Arduino 라이브러리를 추가한다.

![123](https://user-images.githubusercontent.com/56014938/132096868-0e7ac7bd-75c1-4bfa-9e79-f2448d4c5ee7.png)


# 외부 접속 설정

GRANT 명령어를 이용하여 외부에서 접속이 가능하게 허용해준다.

## 모든 IP 대역에서 허용

```
GRANT ALL PRIVILEGES ON *.* TO '아이디'@'%' IDENTIFIED BY '패스워드';
```

## 아두이노 구현 예시

INSERT 예제
```
#include <MySQL_Connection.h>
#include <MySQL_Cursor.h>
#include <SPI.h>
#include <WiFiNINA.h>

#include "arduino_secrets.h" 
///////please enter your sensitive data in the Secret tab/arduino_secrets.h
char ssid[] = SECRET_SSID;        // your network SSID (name)
char pass[] = SECRET_PASS;    // your network password (use for WPA, or use as key for WEP)

IPAddress server_addr(121, 173, 239, 121);    // IP of the MySQL *server* here
char user[] = "root";              // MySQL user login username
char password[] = "password!";        // MySQL user login password
char query[] = "INSERT INTO test_arduino.hello_arduino (message) VALUES ('Hello, Arduino!')";

int status = WL_IDLE_STATUS;

WiFiClient client;
MySQL_Connection conn((Client *)&client);

void setup() {
  //Initialize serial and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }

  // check for the WiFi module:
  if (WiFi.status() == WL_NO_MODULE) {
    Serial.println("Communication with WiFi module failed!");
    // don't continue
    while (true);
  }

  String fv = WiFi.firmwareVersion();
  if (fv < WIFI_FIRMWARE_LATEST_VERSION) {
    Serial.println("Please upgrade the firmware");
  }

  // attempt to connect to WiFi network:
  while (status != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    // Connect to WPA/WPA2 network. Change this line if using open or WEP network:
    status = WiFi.begin(ssid, pass);

    // wait 10 seconds for connection:
    delay(10000);
  }
  Serial.println("Connected to WiFi");
  printWifiStatus();
  if (conn.connect(server_addr, 3306, user, password)) {
      delay(1000);
  }
  else
    Serial.println("Connection failed.");
}

void loop() {
  delay(2000);

  Serial.println("Recording data.");

  // Initiate the query class instance
  MySQL_Cursor *cur_mem = new MySQL_Cursor(&conn);
  // Execute the query
  cur_mem->execute(query);
  // Note: since there are no results, we do not need to read any data
  // Deleting the cursor also frees up memory used
  delete cur_mem;
}


void printWifiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your board's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}
```
```
SELECT 예제

#include <MySQL_Connection.h>
#include <MySQL_Cursor.h>
#include <SPI.h>
#include <WiFiNINA.h>

#include "arduino_secrets.h" 
///////please enter your sensitive data in the Secret tab/arduino_secrets.h
char ssid[] = SECRET_SSID;        // your network SSID (name)
char pass[] = SECRET_PASS;    // your network password (use for WPA, or use as key for WEP)

IPAddress server_addr(121, 173, 239, 121);    // IP of the MySQL *server* here
char user[] = "root";              // MySQL user login username
char password[] = "password!";        // MySQL user login password
char query[] = "SELECT number FROM arduino.info";

int status = WL_IDLE_STATUS;

WiFiClient client;
MySQL_Connection conn((Client *)&client);

void setup() {
  //Initialize serial and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }

  // check for the WiFi module:
  if (WiFi.status() == WL_NO_MODULE) {
    Serial.println("Communication with WiFi module failed!");
    // don't continue
    while (true);
  }

  String fv = WiFi.firmwareVersion();
  if (fv < WIFI_FIRMWARE_LATEST_VERSION) {
    Serial.println("Please upgrade the firmware");
  }

  // attempt to connect to WiFi network:
  while (status != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    // Connect to WPA/WPA2 network. Change this line if using open or WEP network:
    status = WiFi.begin(ssid, pass);

    // wait 10 seconds for connection:
    delay(10000);
  }
  Serial.println("Connected to WiFi");
  printWifiStatus();
  if (conn.connect(server_addr, 3306, user, password)) {
      delay(1000);
  }
  else
    Serial.println("Connection failed.");
}

void loop() {
  row_values *row = NULL;
  long head_count = 0;

  delay(1000);

  // Initiate the query class instance
  MySQL_Cursor *cur_mem = new MySQL_Cursor(&conn);
  // Execute the query
  cur_mem->execute(query);
  // Fetch the columns (required) but we don't use them.
  column_names *columns = cur_mem->get_columns();

  // Read the row (we are only expecting the one)
  do {
    row = cur_mem->get_next_row();
    if (row != NULL) {
      head_count = atol(row->values[0]);
    }
  } while (row != NULL);
  // Deleting the cursor also frees up memory used
  delete cur_mem;

  Serial.println(head_count);

  delay(500);
}


void printWifiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your board's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}
```

## arduino.secrets.h
```
#define SECRET_SSID ""
#define SECRET_PASS ""
```
 
|                                                            |                                                        |
|------------------------------------------------------------|--------------------------------------------------------|
| MySQL_Connection                                           | MySQL에 접속할 때 사용하는 클래스                      |
| MySQL_Cursor                                               | MySQL에 접속할 인스턴스를 지정하는 클래스              |
| (MySQL_Connection).connect(서버IP, 포트, 계정명, 비밀번호) | MySQL에 접속하는 함수 (boolean)                        |
| (MySQL_Cursor)->execute(쿼리구문)                          | MySQL 구문을 실행(=전송)하는 함수 (boolean             |
| column_names                                               | MySQL 데이터의 각 열의 목록을 저장하는 자료형          |
| (MySQL_Cursor)->get_columns()                              | 해당 MySQL 인스턴스에서 각 열을 가져옴. (column_names) |
| row_values                                                 | DB의 특정 열에서 각 행에 따라서 정보를 저장하는 자료형 |
| (MySQL_Cursor)->get_next_row()                             | DB에서 행을 하나씩 가져와 저장하는 함수(row_values)    |
| (MySQL_Cursor)->close()                                    | 해당 인스턴스를 삭제하는 함수(void)      
