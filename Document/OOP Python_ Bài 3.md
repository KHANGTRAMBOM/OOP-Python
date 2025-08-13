# **OVERVIEW**
Chào các bạn đã trở lại với series `Lập trình hướng đối tượng`. Cùng nhau bắt đầu bài tiếp theo nhé 😁😁😁

# **NỘI DUNG CHÍNH**

## **1. THUỘC TÍNH VÀ PHƯƠNG THỨC LỚP (CLASS METHODS & VARIABLES)**

Như đã đề cập, `phương thức` và `thuộc tính` trong `lập trình hướng đối tượng` (OOP) có nhiều loại, như `phương thức` và `thuộc tính` của `thực thể` (instance). Phần này giới thiệu về `phương thức lớp` và `thuộc tính lớp`.

### **Khái niệm cơ bản**
Thông thường, để sử dụng `phương thức` hoặc `thuộc tính` của một lớp, cần khởi tạo một `đối tượng` của lớp đó. Tuy nhiên, `phương thức lớp` và `thuộc tính lớp` cho phép truy cập trực tiếp qua tên lớp mà không cần tạo `đối tượng`.

### **Đặc điểm và ứng dụng**
`Thuộc tính lớp` là các giá trị chung cho tất cả `đối tượng` của lớp, hay nói cách khác các `Thuộc tính lớp` có giá trị giống nhau và chia sẽ giá trị giữa các đối tượng thuộc lớp đó.

Trong Python, `phương thức lớp` được định nghĩa bằng decorator `@classmethod`, nhận tham số `cls` đại diện cho lớp.


```python
# Lớp car
class Car:
  # Thuộc tính lớp
  total_car = 0

  def __init__(self, name: str):
    self.name = name
    # Không dùng self vì self là của instance
    Car.total_car += 1

  # Phương thức lớp
  @classmethod
  def get_count_car(cls):
     # Dùng cls thay thế cho self
     return cls.total_car


cars = [Car("Toyota"), Car("Honda"), Car("VinFast")]

for car in cars:
  # Giá trị name của 3 đối tượng khác nhau
  # vì nó là thuộc tính instance
  print(f"car name is: {car.name:<15}", end="")
  # Giá trị của total_car của 3 đối tượng giống nhau
  # do được chia sẽ vì nó là thuộc tính lớp
  print(f"{car.get_count_car()}")



print(f"Truy cập total_car qua lớp: {Car.total_car}")
```

    car name is: Toyota         3
    car name is: Honda          3
    car name is: VinFast        3
    Truy cập total_car qua lớp: 3
    

## **2. THUỘC TÍNH VÀ PHƯƠNG THỨC STATIC (STATIC METHODS & VARIABLES)**
Về cơ bản `thuộc tính` và `phương thức` static là giống nhau, chỉ khác về mục đích sử dụng và một số ít ở `phương thức`:
1. `Phương thức` static khi khai báo sẽ có decorate là `@staticmethod` thay cho `@classmethod`
2. Tham số truyền vào không được phép có `self` và `cls`

```python
class Car:
  # Thuộc tính lớp
  total_car = 0

  def __init__(self, name: str):
    self.name = name
    Car.total_car += 1

  # Phương thức lớp
  @classmethod
  def get_count_car(cls):
     return cls.total_car
  
  # Phương thức static nó có thể là bất cứ thứ gì
  # kể cả không liên quan đến lớp
  @staticmethod
  def add(a:int , b:int):
    return a + b
  
  @staticmethod
  def minus(a:int , b:int):
    return a - b
  
  @staticmethod
  def double_count_car():
    # Có thể sử dụng các thuộc tính instance hay class bình thường
    return Car.total_car * 2
```

## **3. NẠP CHỒNG (OPERATOR OVERLOADING)**

`Nạp chồng` (Operator Overloading) cho phép tùy chỉnh, định nghĩa hành vi của  các toán tử như `+`, `-`, `*`, `/`, `[]`, hoặc các thao tác liên quan đến `str`. Điều này giúp các lớp tùy chỉnh hoạt động tự nhiên hơn với các toán tử.

`Nạp chồng` được thực hiện thông qua các `phương thức đặc biệt` (magic method), được nhận biết bởi định dạng `__tên_phương_thức__`. Ví dụ, `__add__` tùy chỉnh toán tử `+`, `__str__` tùy chỉnh cách hiển thị chuỗi. Hàm `__init__` cũng là một `magic method`, dùng để khởi tạo đối tượng.

```python
class Vector:
  def __init__(self, x, y):
    self.x = x
    self.y = y
  
  # Operator overloading cho phép cộng vector
  def __add__(self, vector: Vector):
    return Vector(self.x + vector.x,self.y + vector.y)
  
  # Operator overloading hiển thị thông tin vector
  def __str__(self):
    return f"Vecotor[{self.x}, {self.y}]"

vector_1 = Vector(3,4)
vector_2 = Vector(1,2)
vector_3 = vector_1 + vector_2
print(vector_3) # Kết quả: Vecotor[4,6]
```

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌
