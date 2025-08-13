# **OVERVIEW**
Chào các bạn đã trở lại với series `Lập trình hướng đối tượng`. Cùng nhau bắt đầu bài tiếp theo nhé 😁😁😁

# **NỘI DUNG CHÍNH**

## ⭐⭐ **1. KẾ THỪA (INHERITANCE)**

### **TỔNG QUAN**
`Kế thừa` trong `OOP` giống như việc con cái thừa hưởng tài sản từ cha mẹ ngoài đời. Trong `OOP`, nếu lớp `A` kế thừa từ lớp `B`, ta gọi `A` là `subclass` (lớp con) và `B` là `base class` (lớp cha). Lớp con sẽ thừa hưởng tất cả `thuộc tính` và `phương thức` của lớp cha, giúp tái sử dụng mã một cách hiệu quả.

Tuy nhiên, việc kế thừa phụ thuộc vào `khả năng truy cập` của `thuộc tính` và `phương thức` trong lớp cha. Ví dụ, các thành phần `Public` và `Protected` thường được kế thừa, trong khi `Private` (như `__attribute` trong Python) bị hạn chế do `name mangling`. Điều này đảm bảo `tính đóng gói` khi sử dụng `kế thừa`.

`Kế thừa` giúp tăng khả năng tái sử dụng mã và tiết kiệm thời gian phát triển. Khi nhiều lớp có điểm chung (dựa vào mối quan hệ `has - a`, `is -a`), thay vì xây dựng từng lớp riêng lẻ, ta có thể tạo một `base class` chứa các chức năng chung. Các `subclass` sẽ kế thừa những chức năng này và bổ sung các đặc điểm riêng. Ví dụ, lớp `Vehicle` có `thuộc tính` `speed` và `phương thức` `move()`, còn lớp `Car` kế thừa và thêm `thuộc tính` `color`.

👉 Nói cách khác, `kế thừa` trong `OOP` là `tái sử dụng` và `mở rộng`. Trong Python, ta sử dụng cú pháp `class A(B):` để triển khai `kế thừa`.


```python
# Lớp cha
class Animal:
  # Tạo các thuộc tính và phương thức chung
  def __init__(self, name: str):
    self.name = name
    print("Bạn vừa tạo một đối tượng kiểu Animal!!!")

  def eat(self) -> None:
    print(f"{self.name} is eating")

  def sleep(self) -> None:
    print(f"{self.name} is sleeping")

  def move(self) -> None:
    print(f"{self.name} is moving")

# Lớp con
class Fish(Animal):
  def __init__(self, name: str):
    self.name = name
    print("Bạn vừa tạo một đối tượng kiểu Fish!!!")

  # Cá thì có thể bơi → mở rộng từ Animal
  def fly(self) -> None:
    print(f"{self.name} is flying")

# Lớp con
class Bird(Animal):
  def __init__(self, name: str):
    self.name = name
    print("Bạn vừa tạo một đối tượng kiểu Bird!!!")

  # Chim thì có thể bơi → mở rộng từ Animal
  def swim(self) -> None:
    print(f"{self.name} is swiming")


# Tạo 3 đối tượng tương ứng
animal = Animal("Con vật") # Bạn vừa tạo một đối tượng kiểu Animal!!!
fish = Fish("Con cá")      # Bạn vừa tạo một đối tượng kiểu Fish!!!
bird = Bird("Con chim")   # Bạn vừa tạo một đối tượng kiểu Bird!!!

print("="*43)
print("Gọi các phương thức chung")
animal.eat()
fish.eat()
bird.eat()
print("="*43)
print("Gọi các phương thức mở rộng")
fish.fly()
bird.swim()
animal.swim() # báo lỗi vì đó là phương thức của lớp con
```

    Bạn vừa tạo một đối tượng kiểu Animal!!!
    Bạn vừa tạo một đối tượng kiểu Fish!!!
    Bạn vừa tạo một đối tượng kiểu Bird!!!
    ===========================================
    Gọi các phương thức chung
    Con vật is eating
    Con cá is eating
    Con chim is eating
    ===========================================
    Gọi các phương thức mở rộng
    Con cá is flying
    Con chim is swiming
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    /tmp/ipython-input-2418940656.py in <cell line: 0>()
         50 fish.fly()
         51 bird.swim()
    ---> 52 animal.swim() # báo lỗi vì đó là phương thức của lớp con
    

    AttributeError: 'Animal' object has no attribute 'swim'


### **PHƯƠNG THỨC SUPER()**

Trong `OOP`, phương thức `super()` được sử dụng để gọi các `phương thức` của lớp cha từ lớp con. `super()` không chỉ giúp tái sử dụng mã từ lớp cha mà còn cho phép lớp con mở rộng hoặc tùy chỉnh hành vi của các `phương thức` đó.

`super()` thường được dùng trong hàm khởi tạo (`__init__`) hoặc các `phương thức` khác để gọi phiên bản tương ứng của lớp cha.


```python
# Lớp cha
class Shape:
  def __init__(self, color: str):
    self.color = color

  def Describe(self):
    print(f"This shape has {self.color} color")

# Lớp con
class Rectangle(Shape):
  def __init__(self, color, height, width):
    # Sử dụng super để gọi hàm khởi tạo của lớp cha
    super().__init__(color)
    self.height = height
    self.width = width

  # Mở rộng Describe từ lớp cha, có thể giống hoặc khác tên
  def Describe(self):
    super().Describe()
    print("And this is a rectangle")

# Lớp con
class Square(Shape):
  def __init__(self, color, height,):
    # Sử dụng super để gọi hàm khởi tạo của lớp cha
    super().__init__(color)
    self.height = height

  # Mở rộng Describe từ lớp cha, có thể giống hoặc khác tên
  def Describe(self):
    super().Describe()
    print("And this is a square")


# Tạo 3 đối tượng
shape = Shape("Đỏ")
rectangle = Rectangle("Xanh", 3, 4)
square = Square("Vàng", 5)

print("Gọi hàm describe tương ứng từng đối tượng")
print("="*30)
print("Đối tượng Shape")
shape.Describe()
print("="*30)
print("Đối tượng Rectangle")
rectangle.Describe()
print("="*30)
print("Đối tượng Square")
square.Describe()
```

    Gọi hàm describe tương ứng từng đối tượng
    ==============================
    Đối tượng Shape
    This shape has Đỏ color
    ==============================
    Đối tượng Rectangle
    This shape has Xanh color
    And this is a rectangle
    ==============================
    Đối tượng Square
    This shape has Vàng color
    And this is a square
    

### **MỞ RỘNG (OVERRIDE & OVERLOAD)**

Điều gì xảy ra nếu lớp cha và lớp con có `phương thức` cùng tên? Khi tạo một đối tượng từ lớp con và gọi `phương thức` đó, `phương thức` nào sẽ được thực thi? 🤔 Trong `OOP`, hệ thống ưu tiên gọi `phương thức` được định nghĩa gần nhất trong chuỗi kế thừa, tức là `phương thức` của lớp con sẽ được gọi trước.

```python
# Lớp cha
class A:
  def Hello(self):
    print("Hello from A")

# Lớp con
class B(A):
  def Hello(self):
    print("Hello from B")

class C(A):
  pass
  
b = B()
b.Hello() # Kết quả "Hello from B" gọi hàm Hello ở lớp B
c = C()
c.Hello() # Kết quả "Hello from A" gọi hàm hello ở lớp A
```

#### **Override (Ghi đè)**
`Override` xảy ra khi lớp con định nghĩa một `phương thức` có cùng tên và cùng `danh sách tham số` với lớp cha, nhưng khác `nội dung`. Điều này cho phép lớp con `thay đổi` hoặc `mở rộng` hành vi của `phương thức` từ lớp cha.

```python
# Lớp cha
class A:
  def Hello(self):
    print("Hello from A")

# Lớp con
class B(A):
  # Override
  def Hello(self):
    print("Hello from B")

b = B()
b.Hello() # Kết quả "Hello from B"
```

#### **Overload (Nạp chồng)**
`Overload` xảy ra khi lớp định nghĩa nhiều `phương thức` có cùng tên nhưng khác `danh sách tham số` (số lượng hoặc kiểu dữ liệu). Lưu ý rằng Python không hỗ trợ `overload` theo cách truyền thống như `C++` hay `Java`, nhưng có thể mô phỏng bằng tham số mặc định hoặc các kỹ thuật khác.

```python
# Lớp cha
class A:
    def Hello(self):
        print("Hello from A")

# Lớp con
class B(A):
    # Overload nhưng trong Python `Overload` thật ra cũng chỉ là định nghĩa lại
    # như Override thôi

    def Hello(self, name: str):
        print(f"Hello {name} from B")

b = B()
b.Hello("Khang")  # Hello Khang from B
```

## **2. ĐA KẾ THỪA (MULTIPLE INHERITANCE)**

`Đa kế thừa` là một dạng mở rộng của `kế thừa` trong `lập trình hướng đối tượng` (OOP), cho phép một lớp kế thừa từ nhiều lớp cha, từ đó nhận được tất cả `thuộc tính` và `phương thức` của các lớp cha. Trong Python, cú pháp để lớp `A` kế thừa từ `B` và `C` là: `class A(B, C):`.
```python
# Lớp cha
class Father:
  def __init__(self, money: int):
    self.money = money

# Lớp cha
class Mother:
  def __init__(self, food: str):
    self.food = food

# Lớp con
class Children(Father, Mother):
  def __init__(self, money: int, food: str):
    Father.__init__(money)
    Mother.__init__(food)
```
### **Vấn đề logic**
`Đa kế thừa` có thể gây ra các vấn đề logic, bao gồm:
1. `Diamond problem` (vấn đề kim cương): Xảy ra khi một lớp cha (`A`) được kế thừa bởi hai lớp (`B` và `C`), và một lớp khác (`D`) kế thừa cả `B` và `C`, tạo thành cấu trúc giống viên kim cương. Điều này dẫn đến xung đột về `thuộc tính` hoặc `phương thức`. Python giải quyết vấn đề này bằng `Method Resolution Order` (MRO), một thuật toán xác định thứ tự ưu tiên của các lớp cha.

  ```python
                                                          A
                                                        /   \
                                                      B       C
                                                        \   /
                                                          D

  ```
2. `Circular inheritance` (vòng lặp kế thừa): Xảy ra khi `A` kế thừa `B`, `B` kế thừa `C`, và `C` lại kế thừa `A`, gây lỗi logic. Python sẽ phát hiện và báo lỗi trong trường hợp này.

### **So sánh với các ngôn ngữ khác**
Do tính phức tạp của `đa kế thừa`, các ngôn ngữ như `Java` cấm nó cho các lớp, sử dụng `interface` để thay thế, trong khi `C++` hỗ trợ nhưng dễ gây lỗi nếu không quản lý tốt. Thay vào đó, chúng sử dụng:
- `Abstract class` (lớp trừu tượng): Một lớp không thể khởi tạo trực tiếp, dùng để định nghĩa các phương thức chung.
- `Interface`: Một tập hợp các phương thức mà lớp con phải triển khai.

Python hỗ trợ `đa kế thừa` linh hoạt nhờ MRO, nhưng cần sử dụng cẩn thận để tránh làm mã phức tạp hoặc khó bảo trì.

### **Lưu ý khi sử dụng đa kế thừa**
- Sử dụng `super()` để gọi `phương thức` từ các lớp cha một cách chính xác, đặc biệt trong các tình huống phức tạp.
- Kiểm tra MRO bằng `ClassName.__mro__` để hiểu thứ tự kế thừa.
- Ưu tiên thiết kế đơn giản, tránh lạm dụng `đa kế thừa` để giữ mã dễ đọc và bảo trì.


```python
# Diamond Problem
class A:
  def hello(self):
    print("Hello from A")


class B(A):
  def hello(self):
    print("Hello from B")

class C(A):
  def hello(self):
    print("Hello from C")

class D(B,C):
  pass

d = D()
# Trong các ngôn ngữ C++ và Java nó sẽ báo lỗi do mơ hồ
# không biết nên gọi của B hay C. Nhưng trong Python MRO
# nó sẽ sắp xếp thứ tự thành D → B → C → A → Object nên nó
# sẽ gọi hello() của lớp B
d.hello()
```

    Hello from B
    


```python
# Circular inheritance
class A1(C1):
  def hello(self):
    print("Hello from A")

class B1(A1):
  def hello(self):
    print("Hello from B")

class C1(B1):
  def hello(self):
    print("Hello from C")

# Nó sẽ báo lỗi logic
c = C1()
c.hello()
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    /tmp/ipython-input-2296123697.py in <cell line: 0>()
          1 # Circular inheritance
    ----> 2 class A1(C1):
          3   def hello(self):
          4     print("Hello from A")
          5 
    

    NameError: name 'C1' is not defined


## ⭐⭐⭐ **3. ĐA HÌNH (POLYMORPHISM)**

`Đa hình` (Polymorphism) trong `lập trình hướng đối tượng` (OOP) có nghĩa là "nhiều hình dạng", cho phép một `phương thức` thực hiện các hành vi khác nhau tùy thuộc vào lớp của đối tượng. Nói cách khác, cùng một tên `phương thức` có thể dẫn đến các hành động khác nhau dựa trên lớp của đối tượng `(override, overload)`.

**Ví dụ:**
```python
class Animal:
  def speak(self):
    print("Animal is speaking")


class Dog(Animal):
  def speak(self):
    print("Dog is speaking")

class Cat(Animal):
  def speak(self):
    print("Cat is speaking")


animals = [Dog(), Cat(), Animal()]

for a in animals:
    a.speak()

# Kết quả:
# Dog is speaking
# Cat is speaking
# Animal is speaking
```

`Đa hình` không chỉ áp dụng cho `phương thức` mà còn cho `đối tượng`. Một `đối tượng` của lớp con có thể được đối xử như `đối tượng` của lớp cha. Trong `OOP`, một `đối tượng` của lớp con có thể được gán vào biến kiểu lớp cha (gọi là `upcasting`).

**Ví dụ:**
```python
class Animal:
  def speak(self):
    print("Animal is speaking")

class Dog(Animal):
  def speak(self):
    print("Dog is speaking")

class Cat(Animal):
  def speak(self):
    print("Cat is speaking")

def animal_speak(animal: Animal):
  animal.speak()

animals = [Dog(), Cat(), Animal()]

for a in animals:
    animal_speak(a) # Tham số yêu cầu là Animal nhưng các lớp kế thừa từ Animal
                    # vẫn có thể truyền vào được

# Kết quả:
# Dog is speaking
# Cat is speaking
# Animal is speaking
```

### **Duck Typing**

Trong Python, `đa hình` với `đối tượng` không chỉ dựa vào kế thừa mà còn thông qua cơ chế `duck typing`.

> "Nếu nó trông giống vịt và kêu như vịt, thì nó là vịt!"

Điều này có nghĩa là các `đối tượng` từ các lớp `khác nhau` có thể được sử dụng trong cùng một ngữ cảnh, miễn là chúng có các `phương thức` cần thiết.

Khác với các ngôn ngữ tĩnh như `Java` hay `C++`, Python không yêu cầu khai báo kiểu rõ ràng. `Duck typing` giúp mã linh hoạt hơn, nhưng cần cẩn thận để đảm bảo các `đối tượng` có `phương thức` phù hợp, tránh lỗi khi chạy. Cơ chế này làm cho `đa hình` trong Python trở nên mạnh mẽ và dễ sử dụng.

```python
class People:
  def speak(self):
    print("Human is speaking")

class Car:
  def speak(self):
    print("Ting Ting Ting.....")

class Duck():
  def speak(self):
    print("Quak Quak Quak.....")

objects = [People(), Car(), Duck()]

for o in objects:
    o.speak() # Không cần quan tâm o thuộc lớp gì chỉ cần có đúng phương thức
              # thì o chính là 'nó'

# Kết quả:
# Human is speaking
# Ting Ting Ting.....
# Quak Quak Quak.....
```

## ⭐⭐⭐⭐ **4. TRỪU TƯỢNG (ABSTRACTION)**

`Trừu tượng` (Abstraction) là nguyên tắc cuối và cốt lõi trong `lập trình hướng đối tượng`,nó giúp ẩn các chi tiết triển khai phức tạp và chỉ cung cấp giao diện cần thiết. `Abstraction` thường được dùng để khắc phục các vấn đề logic trong `đa kế thừa`, đảm bảo các lớp con tuân theo một khuôn mẫu chung.

📌 **Ví dụ:** Một bản thiết kế xây nhà yêu cầu có: tường, mái nhà, hồ bơi, phòng gym, phòng karaoke (khuôn mẫu) mà không nói rõ là làm hay xây như thế nào (ẩn các chi tiết triễn khai).

### **Lớp trừu tượng và Interface**
`Abstraction` và `Interface` quy định các `phương thức` và đôi khi cả `thuộc tính` `bắt buộc` mà các `lớp con` phải triển khai, cụ thể:
1. `Abstraction` có thể chứa cả `phương thức` trừu tượng (không có nội dung) và `phương thức` cụ thể (có nội dung).
2. `Interface` (trong các ngôn ngữ như `Java`) chỉ chứa các `phương thức` trừu tượng (không có nội dung).

`Cả hai` đều là `bản thiết kế trừu tượng`, `không` dùng để `tạo đối tượng` mà chỉ để các lớp con `kế thừa`.

> 📌 `Abstract là lớp ảo (không thể tạo đối tượng), Interface là hợp đồng (bắt buộc lớp con định nghĩa lại phương thức`

### **Phương thức trừu tượng**
Các `phương thức trừu tượng` trong lớp trừu tượng chỉ định nghĩa "có thể làm gì" mà không cung cấp triển khai cụ thể. Ví dụ, một lớp trừu tượng `Shape` có thể yêu cầu mọi lớp con như `Circle` hay `Rectangle` phải triển khai `phương thức` `calculate_area()`. Các lớp con phải định nghĩa lại các `phương thức` này để phù hợp với nhu cầu của mình.

### **Ứng dụng trong đa kế thừa**
Trong `đa kế thừa`, `trừu tượng` giúp giảm xung đột (như `diamond problem`) bằng cách yêu cầu các lớp con triển khai các `phương thức` bắt buộc theo cách riêng.

### **Abstraction và Interface trong Python**
Trong Python, `trừu tượng` được hỗ trợ qua module `abc` (Abstract Base Class), đảm bảo mã rõ ràng và dễ bảo trì.

Python không có `Interface` chính thức như `Java`, nhưng có thể mô phỏng `Interface` bằng cách sử dụng `lớp trừu tượng` với tất cả các `phương thức` đều là trừu tượng. Các lớp con kế thừa phải định nghĩa lại các `phương thức` này để đáp ứng yêu cầu cụ thể.



```python
# Tạo lớp trựu tượng bằng cách kế thừa ABC
# một lớp được Python xây dựng để tạo các lớp trừu tượng
from abc import ABC, abstractmethod

# Vì không được dùng để tạo đối tượng nên không cần tạo
# hàm khởi tạo đối với interface, abstract có thể tạo
# được
class Shape(ABC):

  # Thuộc tính trừu tượng, tạo nó thông qua property (ít phổ biến so với
  # phương thức)
  @property
  @abstractmethod
  def name(self):
    pass

  # Phương thức trừu tượng có thể hoặc không ghi nội dung
  # chỉ áp dụng cho abstract
  @abstractmethod
  def area(self):
    pass

  @abstractmethod
  def perimeter(self):
    pass


# Lớp con kế thừa lớp ảo
class Circle(Shape):
  # Hàm khởi tạo
  def __init__(self, radius: int):
    self.radius = radius

  # Kế thừa và viết lại các phương thức và thuộc tính ảo
  @property # getter
  def name(self):
    return "Hình tròn"

  # Kế thừa và viết lại các phương thức và thuộc tính ảo
  def area(self):
    return 3.14 * self.radius * self.radius

  # Kế thừa và viết lại các phương thức và thuộc tính ảo
  def perimeter(self):
    return 3.14 * 2 * self.radius

# Lớp con kế thừa lớp ảo
class Rectangle(Shape):
  # Hàm khởi tạo
  def __init__(self, width: int, height: int):
    self.width = width
    self.height = height

  # Kế thừa và viết lại các phương thức và thuộc tính ảo
  @property  # getter
  def name(self):
    return "Hình chữ nhật"

  # Kế thừa và viết lại các phương thức và thuộc tính ảo
  def area(self):
    return self.height * self.width

  # Kế thừa và viết lại các phương thức và thuộc tính ảo
  def perimeter(self):
    return self.height * 2 + self.width * 2

shapes = [Rectangle(10, 5), Circle(7)]
for shape in shapes:
  print(f"name of shape: {shape.name:<15}      area: {shape.area():<15}      perimeter {shape.perimeter()}")
```

    name of shape: Hình chữ nhật        area: 50                   perimeter 30
    name of shape: Hình tròn            area: 153.86               perimeter 43.96
    

### **THINKING ABOUT ABSTRACTION**

#### **Nếu tôi muốn có cả getter và setter ở lớp abstract thì sao ?**
Vì cần `set` (thay đổi giá trị) cho `thuộc tính` nên phải tạo hàm khởi tạo `__init__` để gán giá trị ban đầu cho thuộc tính này, từ đó có thể sử dụng `getter` để lấy giá trị khi cần.


```python
from abc import ABC, abstractmethod
class Shape(ABC):
  def __init__(self, color):
      self.color = color
  
  @property
  @abstractmethod
  def color(self):
    pass
  
  @property
  @abstractmethod
  def color(self, new_color: str):
    pass

class Circle(Shape):
  def __init__(self, color, radius):
    # Hoàn toàn có thể gọi hàm khởi tạo của lớp ảo
    super().__init__(color)
    self.radius = radius
  
  # Định nghĩa lại getter, setter
  @property
  def color(self):
    return self.color
  
  @color.setter
  def color(self, new_color):
    self.color = new_color
```

#### **Nếu khi làm vậy lớp ảo có hàm khởi tạo giả sử người dùng cố ý tạo đối tượng thì sao**
Do Python có cú pháp khá `lỏng lẻo` nên chuyện này cũng có thể xảy ra.  
1. Thứ nhất, nếu trong `lớp ảo` không có hàm khởi tạo, nó sẽ gọi hàm khởi tạo mặc định không tham số của `object`.

  ```python
  def __init__(self):
    pass
  ```
2. Thứ hai, trong `lớp ảo` keyword chính là `@abstractmethod`. Nếu một `lớp ảo` (thậm chí kế thừa từ `ABC`) mà không có `@abstractmethod` phía trên tên các `phương thức`, thì `lớp đó` không phải là `lớp ảo` và ta hoàn toàn có thể tạo đối tượng từ lớp đó.

  ```python
  from abc import ABC, abstractmethod
  class AbstractClass(ABC):
    # Luôn có một hàm khởi tạo mặc định dù không khai báo

    # dùng kí hiệu @abstractmethod
    @abstractmethod
    def do_something(self):
      pass
    
  a = AbstractClass() # Báo lỗi
  a.do_something()
  ```
  ```python
  from abc import ABC, abstractmethod
  class AbstractClass(ABC):

    # Không dùng kí hiệu @abstractmethod
    def do_something(self):
      print("I am doing something !!!")
    
  a = AbstractClass() # Tạo được luôn đối tượng
  a.do_something() # Kết quả: I am doing something!!!
  ```

👉 Do các `phương thức` và `thuộc tính` của `lớp ảo` có thể được định nghĩa hoặc không, nên nếu `lớp ảo` chỉ chứa các `phương thức` và `thuộc tính` được khai báo (không định nghĩa), thì nó sẽ biến từ `ảo` thành `thật`. Còn dối với `Interface` tất cả phải điều là `ảo`

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌
