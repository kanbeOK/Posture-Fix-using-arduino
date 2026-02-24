# Posture-Fix-using-arduino



# 2. Linh kiện 

| Linh kiện | Số lượng | Công dụng |
| --- | --- | --- |
| **Arduino Uno/Nano** | 1 | Bộ não xử lý |
| **HC-SR04** | 1 | Cảm biến siêu âm đo khoảng cách từ mắt đến màn hình |
| **Servo SG90** | 1 | Cánh tay vẫy nhắc nhở |
| **Buzzer** | 1 | Còi phát tiếng bip |
| **LCD 16x2 (I2C)** | 1 | Hiển thị thông báo |

---

# 3. Sơ đồ kết nối

1. **Cảm biến siêu âm:** `VCC` -> 5V, `GND` -> GND, `Trig` -> D9, `Echo` -> D10.
2. **Servo:** Dây tín hiệu (Cam) -> D11, `VCC` -> 5V, `GND` -> GND.
3. **LCD:** `SCL` -> A5, `SDA` -> A4.
4. **Buzzer:** Cực dương -> D8, cực âm -> GND.

---

# 4. Code

```cpp
#include <Servo.h>
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

Servo myServo;
LiquidCrystal_I2C lcd(0x27, 16, 2);

const int trigPin = 9;
const int echoPin = 10;
long duration;
int distance;

void setup() {
  myServo.attach(11);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  lcd.init();
  lcd.backlight();
  myServo.write(0); // Vị trí nghỉ
}

void loop() {
  // Đo khoảng cách
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  if (distance < 40) { // Nếu ngồi quá gần (< 40cm)
    lcd.clear();
    lcd.print("Ngoi xa ra di!");
    tone(8, 1000, 200); // Kêu bip
    myServo.write(90);  // Vẫy tay cảnh báo
    delay(500);
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Tu the tot!");
    myServo.write(0);
  }
  delay(1000);
}

```




> **Mẹo nhỏ:** Bạn có thể thay cảm biến siêu âm bằng cảm biến độ nghiêng (Tilt sensor) gắn lên áo để đo trực tiếp độ cong của lưng.

Bạn có muốn mình hướng dẫn chi tiết hơn về cách **viết app điện thoại** để quản lý dữ liệu từ chiếc máy này không?
