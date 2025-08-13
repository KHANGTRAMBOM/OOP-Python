# **OVERVIEW**

Xin chào các bạn 👋, đây là `series mới` của tôi, tiếp nối series về `cấu trúc dữ liệu & giải thuật`. Series này sẽ mang đến cho bạn những kiến thức cơ bản về phương pháp lập trình `OOP` (`lập trình hướng đối tượng`) một cách dễ hiểu và thực tế.

## **Vậy `lập trình hướng đối tượng` (OOP) là gì?**
`Lập trình hướng đối tượng` là một kỹ thuật lập trình cơ bản, rất quan trọng và được sử dụng rộng rãi hiện nay. Nó ra đời để khắc phục các nhược điểm của `lập trình thủ tục`, bao gồm:
1. Dữ liệu và hàm xử lý tách biệt, khó liên kết logic.
2. Khó tái sử dụng và mở rộng mã.
3. Quản lý và sửa lỗi phức tạp.
4. Nguy cơ rò rỉ dữ liệu và vấn đề bảo mật.

Khi thời đại công nghệ phát triển, các phần mềm ngày càng phức tạp, đòi hỏi một cách tiếp cận lập trình hiệu quả hơn. `OOP` chính là giải pháp!

## **Bốn nguyên tắc cốt lõi của OOP**
`OOP` được xây dựng dựa trên 4 nguyên tắc quan trọng:
1. `Tính đóng gói`: Bảo vệ dữ liệu, chỉ cho phép truy cập qua các phương thức được định nghĩa.
2. `Tính kế thừa`: Cho phép tái sử dụng mã từ các lớp khác.
3. `Tính đa hình`: Linh hoạt trong việc sử dụng các đối tượng theo nhiều cách khác nhau.
4. `Tính trừu tượng hóa`: Ẩn đi các chi tiết phức tạp, chỉ hiển thị những gì cần thiết.

Chính nhờ 4 nguyên tắc này, `OOP` đã khắc phục các hạn chế của lập trình thủ tục, mở ra một kỷ nguyên mới trong lập trình.

## **Mô hình hóa thế giới thực**
Người ta thường nói `OOP` là cách mô hình hóa thế giới thực vào máy tính thông qua `class` (lớp) và `object` (đối tượng). Ví dụ, một chiếc xe hơi có thể được biểu diễn bằng một `class` với các thuộc tính như màu sắc, tốc độ, và các hành vi như chạy, dừng.

# **NỘI DUNG CHÍNH**


## **1. LỚP (CLASS)**

`Class` là một kiểu dữ liệu tự định nghĩa trong `lập trình hướng đối tượng` (OOP), tương tự như `Struct` trong `C` hay `C++`. Một `Class` bao gồm:

1. `Thuộc tính`: Đại diện cho các đặc điểm của lớp → lớp đó có những gì. Ví dụ, lớp `People` có thể có các thuộc tính như `tên`, `tuổi`, `giới tính`, `năm sinh`, v.v.
2. `Phương thức`: Đại diện cho các hành động mà lớp có thể thực hiện → lớp đó có thể làm những gì. Ví dụ, lớp `People` có thể có các phương thức như `ăn()`, `uống()`, `ngủ()`, v.v.

Nói cách khác, `Class` là một bản thiết kế để tạo ra các đối tượng cụ thể trong chương trình.

## **2. THỰC THỂ (OBJECT)**

`Object` là một thể hiện cụ thể của `Class`, giống như cách ta tạo ra một chiếc xe thật từ bản vẽ kỹ thuật. Ví dụ, từ `Class` `Car`, ta có thể tạo một `Object` là một chiếc xe cụ thể với `color` là "đỏ" và `speed` là 120 km/h. Mỗi `Object` mang các giá trị riêng cho `thuộc tính` và có thể thực thi các `phương thức` được định nghĩa trong `Class`.

Nói cách khác, nếu `Class` là bản thiết kế, còn `Object` sẽ là sản phẩm được tạo ra từ bản thiết kế đó.

## **3. HÀM KHỞI TẠO & HÀM HỦY (CONSTRUCTOR, DESTRUCTOR)**

### **Hàm khởi tạo**
Khi tạo một `Object` từ `Class`, ta cần một `hàm khởi tạo` (Constructor) để thiết lập các `thuộc tính` ban đầu cho đối tượng đó. `Hàm khởi tạo` thường có cùng tên với `Class` (hoặc `__init__` trong Python) và có thể nhận tham số để gán giá trị cụ thể. Ví dụ, trong lớp `Car`, `hàm khởi tạo` có thể gán `color` là "đỏ" và `speed` là 120 km/h cho một chiếc xe cụ thể.
```python
# Class
class car:
  # Hàm khởi tạo
  def __init__(self, color, speed):
    self.color = color
    self.speed = speed
    print("Bạn vừa tạo thành công một chiếc xe")

# Đối tượng (Object)
car_1 = car("Đỏ", 120) → Một chiếc xe màu đỏ có tốc độ 120km/h
```

### **Hàm hủy**
Như câu nói "Có sinh ắt có diệt", `hàm hủy` (Destructor) giúp giải phóng `Object` khi không còn cần thiết. Trong `OOP`, khi tạo một `Object`, hệ thống cấp phát một vùng nhớ `RAM` cho nó, `dung lượng` phụ thuộc vào các `thuộc tính` và cấu trúc của `Class`. `Hàm hủy` đảm bảo giải phóng vùng nhớ này khi `không cần` sử dụng, giúp tối ưu và tiết kiệm tài nguyên `RAM`. Ví dụ, trong C++, `hàm hủy` được gọi khi đối tượng bị xóa, trong khi ở Java, việc này thường do Garbage Collector xử lý.

```python
# Class
class car:
  # Hàm khởi tạo
  def __init__(self, color, speed):
    self.color = color
    self.speed = speed
    print("Bạn vừa tạo thành công một chiếc xe")
  
  # Hàm hủy
  def __del__(self):
    print("Tạm biệt chiếc xe yêu dấu")

# Đối tượng (Object)
car_1 = car("Đỏ", 120) → Một chiếc xe màu đỏ có tốc độ 120km/h

# Sử dụng hàm hủy
del car_1
```

## **4. INSTANCE VARIABLE & INSTANCE METHOD**

Như đã đề cập trong phần **LỚP**, mỗi `Class` bao gồm `thuộc tính` và `phương thức`. Trong `lập trình hướng đối tượng` (OOP), có nhiều loại `thuộc tính` và `phương thức` khác nhau (sẽ được đề cập trong các phần sau), nhưng phổ biến nhất là `Instance Variable` và `Instance Method`, thuộc về từng đối tượng cụ thể.

- `Instance Variable`: Là các biến chứa dữ liệu riêng của mỗi đối tượng.
- `Instance Method`: Là các phương thức thao tác trên dữ liệu của đối tượng đó.

Trong các ngôn ngữ như Python, cú pháp thường sử dụng `self` để truy cập `Instance Variable` hoặc định nghĩa `Instance Method`.

```python
# Class
class car:
  # Hàm khởi tạo
  def __init__(self, color, speed):
    # Các thuộc tính instance
    self.color = color
    self.speed = speed
    print("Bạn vừa tạo thành công một chiếc xe")
  
  # Các phương thức instance
  def run(self):
    print("Xe đang chạy")
  
  def honk(self):
    print("Ting , ting ting.......")
  
  def park(self):
    print("Dừng xe")

  # Hàm hủy
  def __del__(self):
    print("Tạm biệt chiếc xe yêu dấu")
```

## ⭐ **5. TÍNH ĐÓNG GÓI (ENCAPSULATION)**

`Tính đóng gói` giúp người dùng chỉ cần biết cách sử dụng sản phẩm mà không cần hiểu cách nó hoạt động bên trong. Điều này đảm bảo tính bảo mật, tổ chức mã tốt hơn và tăng khả năng bảo trì chương trình. `Tính đóng gói` hạn chế `kiểm soát truy cập` (access modifier) thông qua ba hình thức chính:

1. `Public`: Ai cũng có thể truy cập. Nói chính xác hơn là: các đối tượng, các lớp điều có thể sử dụng các phương thức và các thuộc tính của class.
2. `Protected`: Chỉ các `Class` có mối quan hệ `kế thừa` (sẽ đề cập sau) được truy cập.
3. `Private`: Chỉ bản thân `Class` đó được sử dụng.

```python
# Class
class car:
  # Hàm khởi tạo
  def __init__(self, color, speed):
    # Các thuộc tính instance
    public self.color = color # public
    private self.speed = speed # private
    print("Bạn vừa tạo thành công một chiếc xe")
  
  # Các phương thức instance
  # Nếu không khai báo modifield mặc định là public
  private def run(self): # private
    print("Xe đang chạy")
  
  public def honk(self): # public
    print("Ting , ting ting.......")
  
  def park(self): # public
    print("Dừng xe")

  # Hàm hủy
  def __del__(self):
    print("Tạm biệt chiếc xe yêu dấu")

car_1 = car("Đỏ", 120) → Một chiếc xe màu đỏ có tốc độ 120km/h

print("Màu của xe là: ",car_1.color) # Hợp lệ
print("Tốc đọ của xe là: ",car_1.speed) # Không hợp lệ
car_1.run() # Không hợp lệ
car_1.honk() # Hợp lệ
```

### **Tính đóng gói trong Python**
Trong `Python`, `kiểm soát truy cập` chỉ mang tính quy ước, không thực sự ngăn cản truy cập. Cách quy ước như sau:
1. `Public`: Đặt tên bình thường, ví dụ: `def run(self):`.
2. `Protected`: Thêm `_` trước tên, ví dụ: `def _run(self):`. Đây là quy ước, không thực sự hạn chế truy cập.
3. `Private`: Thêm `__` trước tên, ví dụ: `def __run(self):`. `Python` sẽ tự động `rename` thành `_[tên class]__run` (name mangling) để tránh truy cập trực tiếp.

```python
# Class
class car:
  # Hàm khởi tạo
  def __init__(self, color, speed):
    # Các thuộc tính instance
    self.color = color # public
    self.__speed = speed # private
    print("Bạn vừa tạo thành công một chiếc xe")
  
  # Các phương thức instance
  def __run(self): # private
    print("Xe đang chạy")
  
  def honk(self): # public
    print("Ting , ting ting.......")
  
  def park(self): # public
    print("Dừng xe")

  # Hàm hủy
  def __del__(self):
    print("Tạm biệt chiếc xe yêu dấu")

car_1 = car("Đỏ", 120) → Một chiếc xe màu đỏ có tốc độ 120km/h

print("Màu của xe là: ",car_1.color) # Hợp lệ
print("Tốc đọ của xe là: ",car_1.__speed) # Không hợp lệ dù đúng tên theo trong class nhưng đã bị python đổi rồi
car_1.__run() # Không hợp lệ
car_1.honk() # Hợp lệ

```
📌 **Chú ý**:
- Để tránh nhầm lẫn giữa `Class` và `Object`, hãy ôn lại phần **LỚP** và **THỰC THỂ**.
- Trong `Python`, kể cả với `Private`, nếu biết cách `name mangling` (như `_[tên class]__run`), vẫn có thể truy cập được, nhưng điều này không được khuyến khích.

## **6. TRUY CẬP THUỘC TÍNH (PROPERTIES)**

`Properties` là một phần quan trọng của `tính đóng gói` trong `OOP`, giúp kiểm soát việc truy cập và sửa đổi `thuộc tính` của `Class` thông qua các phương thức `getter` và `setter`:

1. `getter`: Lấy giá trị của `thuộc tính`. Ví dụ: `color = car_1.get_color()` trả về màu của xe.
2. `setter`: Sửa đổi giá trị của `thuộc tính`. Ví dụ: `car_1.set_color("Vàng")` thay đổi màu xe thành vàng.

Trong Python, `Properties` thường được triển khai bằng decorator `@property` để truy cập `thuộc tính` như biến thông thường, ví dụ: `car_1.color` thay vì `car_1.get_color()`.

Thông thường, chúng ta có thể để `thuộc tính` là `Public` để dễ dàng truy cập và sửa đổi. Tuy nhiên, để đảm bảo `tính đóng gói` và tăng tính chuyên nghiệp trong `OOP`, nên đặt `thuộc tính` là `Private` và sử dụng `getter`/`setter` để kiểm soát truy cập, tránh sửa đổi dữ liệu không mong muốn.

```python
# Class
class car:
  # Hàm khởi tạo
  def __init__(self, color, speed):
    # Các thuộc tính instance
    self.__color = color # chuyển từ public -> private
    self.__speed = speed # private
    print("Bạn vừa tạo thành công một chiếc xe")
  
  # Các phương thức instance

  def __run(self): # private
    print("Xe đang chạy")
  
  def honk(self):  # public
    print("Ting , ting ting.......")
  
  def park(self):  # public
    print("Dừng xe")

  # Properites
  @property # Getter
  def color(self):
    return self.__color
  
  @color.setter # Setter
  def color(self, new_color):
    self.__color = new_color
  
  @color.deleter # Deleter
  def color(self):
    del self.__color

  # Hàm hủy
  def __del__(self):
    print("Tạm biệt chiếc xe yêu dấu")

car_1 = car("Đỏ", 120) → Một chiếc xe màu đỏ có tốc độ 120km/h
color_of_car = car_1.color → Lấy màu xe
car_1.color = "Vàng" → Đổi màu xe từ "Đỏ" sang "Vàng"
```

## **7. KIỂU ẨN (TYPE HINT)**

`Python`, cũng như các ngôn ngữ kiểu động như `JavaScript`, không yêu cầu khai báo kiểu dữ liệu khi tạo `biến`. Kiểu dữ liệu được tự động xác định khi chương trình chạy, giúp cú pháp đơn giản và dễ tiếp cận, đặc biệt với người mới học. Tuy nhiên, để kiểm soát kiểu dữ liệu và tăng tính rõ ràng cho mã, chúng ta có thể sử dụng `Type Hint`.

### **Type Hint là gì?**
`Type Hint` cho phép chỉ định kiểu dữ liệu cho `biến` hoặc `hàm`, giúp mã dễ đọc, dễ bảo trì và hỗ trợ các công cụ kiểm tra kiểu như `mypy`. Ví dụ, nếu một hàm yêu cầu tham số là `int`, ta có thể dùng `Type Hint` để đảm bảo điều này.

### **Cú pháp**
- Cho biến: `[tên biến]: [kiểu dữ liệu]`.  
  Ví dụ:
```python
  age: int = 25
```
- Cho hàm: `def [tên hàm]([tên tham số]: [kiểu dữ liệu]) -> [kiểu trả về]:`.  
  Ví dụ:
```python
  def add(a: int, b: int) -> int:
    return a + b
```

### **Áp dụng vào OOP**
```python
# Class
class car:
  # Hàm khởi tạo
  # Các tham số phải là kiểu str, int
  def __init__(self, color:str , speed:int):
    # Các thuộc tính instance
    self.__color = color # chuyển từ public -> private
    self.__speed = speed # private
    print("Bạn vừa tạo thành công một chiếc xe")
  
  # Các phương thức instance
  def __run(self) -> None: # Không trả về gì cả
    print("Xe đang chạy")
  
  def honk(self) -> None:  # Không trả về gì cả
    print("Ting , ting ting.......")
  
  def park(self) -> None:  # Không trả về gì cả
    print("Dừng xe")

  # Properites
  @property
  def color(self) -> str: # Trả về kiểu str
    return self.__color
  
  @color.setter
  def color(self, new_color:str) -> None: # Đầu vào là str, không trả về
    self.__color = new_color
  
  @color.deleter
  def color(self) -> None:
    del self.__color

  # Hàm hủy
  def __del__(self) -> None:
    print("Tạm biệt chiếc xe yêu dấu")


car_1 = car("Đỏ", 120) → Một chiếc xe màu đỏ có tốc độ 120km/h
color_of_car = car_1.color → Lấy màu xe
car_1.color = "Vàng" → Đổi màu xe từ "Đỏ" sang "Vàng"
```

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌
