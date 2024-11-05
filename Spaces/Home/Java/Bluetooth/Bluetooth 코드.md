#### RXTX 라이브러리 설치

Maven에 의존성을 추가하여 RXTX를 사용할 수 있습니다.


```xml
<dependency>
    <groupId>org.rxtx</groupId>
    <artifactId>rxtx</artifactId>
    <version>2.2</version>
</dependency>
```

코드 구현 예시 (RXTX)

```java
import gnu.io.CommPort;
import gnu.io.CommPortIdentifier;
import gnu.io.SerialPort;

import java.io.InputStream;
import java.io.OutputStream;

public class BluetoothBarcodeScannerRXTX {

    public static void main(String[] args) {
        String portName = "COM3";  // Bluetooth 장치가 연결된 포트 이름

        try {
            // 포트 열기
            CommPortIdentifier portIdentifier = CommPortIdentifier.getPortIdentifier(portName);
            if (portIdentifier.isCurrentlyOwned()) {
                System.out.println("Port is currently in use.");
            } else {
                CommPort commPort = portIdentifier.open("BarcodeScanner", 2000);

                if (commPort instanceof SerialPort) {
                    SerialPort serialPort = (SerialPort) commPort;
                    serialPort.setSerialPortParams(9600, SerialPort.DATABITS_8, SerialPort.STOPBITS_1, SerialPort.PARITY_NONE);

                    // 바코드 데이터를 읽기 위한 InputStream 생성
                    InputStream in = serialPort.getInputStream();
                    OutputStream out = serialPort.getOutputStream();

                    System.out.println("Connected to Bluetooth device on " + portName);

                    byte[] buffer = new byte[1024];
                    int len;

                    // 바코드 데이터 수신
                    while ((len = in.read(buffer)) > -1) {
                        String barcodeData = new String(buffer, 0, len);
                        System.out.println("Barcode Data: " + barcodeData);
                    }

                    in.close();
                    out.close();
                    serialPort.close();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```


#### 코드 설명

- **포트 연결**: Bluetooth 시리얼 포트(COM3)를 통해 Bluetooth Classic 바코드 스캐너에 연결합니다. 포트 이름은 실제 장치에 맞게 변경해야 합니다.
- **데이터 읽기**: `InputStream`을 사용하여 바코드 데이터를 읽습니다. 바코드 스캐너가 데이터를 보낼 때마다 이를 수신하여 출력합니다.

> **참고**: RXTX는 포트 이름이 정확히 설정되어야 하며, 운영체제마다 포트 이름이 다를 수 있습니다.



## 2. Java Bluetooth Stack (Javabluetooth.org)를 사용한 코드 구현

Java Bluetooth Stack(JSR-82)을 활용한 코드는 BlueCove와 비슷한 방식으로 작동합니다. 아래 코드는 Java Bluetooth Stack API를 사용해 장치를 검색하고, Bluetooth Serial Port Profile (SPP)를 통해 바코드 데이터를 읽는 예시입니다.

코드 구현 예시

```java
import javax.bluetooth.*;
import javax.microedition.io.Connector;
import javax.microedition.io.StreamConnection;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.ArrayList;
import java.util.List;

public class BluetoothBarcodeScannerJavaBluetooth {

    // 장치 검색을 위한 장치 탐색기
    public static List<RemoteDevice> discoverDevices() throws Exception {
        List<RemoteDevice> devices = new ArrayList<>();

        final Object inquiryCompletedEvent = new Object();

        DiscoveryListener listener = new DiscoveryListener() {
            public void deviceDiscovered(RemoteDevice btDevice, DeviceClass cod) {
                System.out.println("Device found: " + btDevice.getBluetoothAddress());
                devices.add(btDevice);
            }

            public void inquiryCompleted(int discType) {
                synchronized(inquiryCompletedEvent) {
                    inquiryCompletedEvent.notifyAll();
                }
            }

            public void servicesDiscovered(int transID, ServiceRecord[] servRecord) {}
            public void serviceSearchCompleted(int transID, int respCode) {}
        };

        synchronized(inquiryCompletedEvent) {
            boolean started = LocalDevice.getLocalDevice().getDiscoveryAgent().startInquiry(DiscoveryAgent.GIAC, listener);
            if (started) {
                System.out.println("Starting device discovery...");
                inquiryCompletedEvent.wait();
                System.out.println("Device discovery completed.");
            }
        }

        return devices;
    }

    // 장치에 연결하고 바코드 데이터 읽기
    public static void connectToDevice(String btAddress) throws Exception {
        String url = "btspp://" + btAddress + ":1";  // Bluetooth 주소 (SPP 프로파일 사용)
        StreamConnection connection = (StreamConnection) Connector.open(url);
        
        // InputStream을 통해 바코드 데이터 읽기
        InputStream inputStream = connection.openInputStream();
        OutputStream outputStream = connection.openOutputStream();

        System.out.println("Connected to device: " + btAddress);
        
        byte[] buffer = new byte[1024];
        int bytesRead;
        
        // 바코드 스캐너로부터 데이터 수신
        while ((bytesRead = inputStream.read(buffer)) != -1) {
            String barcodeData = new String(buffer, 0, bytesRead);
            System.out.println("Barcode Data: " + barcodeData);
        }
        
        inputStream.close();
        outputStream.close();
        connection.close();
    }

    public static void main(String[] args) {
        try {
            List<RemoteDevice> devices = discoverDevices();
            if (!devices.isEmpty()) {
                // 첫 번째 장치에 연결 시도 (테스트용)
                String btAddress = devices.get(0).getBluetoothAddress();
                connectToDevice(btAddress);
            } else {
                System.out.println("No Bluetooth devices found.");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```


### 코드 설명

1. **discoverDevices()** 메서드
    
    - Bluetooth 장치를 검색하고, 찾은 장치를 `RemoteDevice` 객체로 리스트에 저장합니다.
    - `DiscoveryListener`를 사용하여 장치를 발견하고 검색이 완료될 때까지 대기합니다.
2. **connectToDevice()** 메서드
    
    - 바코드 스캐너의 Bluetooth 주소를 통해 시리얼 포트 프로파일(SPP) 연결을 만듭니다.
    - `InputStream`을 통해 바코드 데이터를 수신하고, `OutputStream`으로 데이터를 보낼 수 있습니다.
3. **main() 메서드**
    
    - `discoverDevices()`를 통해 장치를 검색하고, 첫 번째 장치에 연결한 후 데이터를 읽어옵니다.