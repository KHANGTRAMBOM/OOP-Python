# **OVERVIEW**
Chào các bạn đã trở lại với series `Lập trình hướng đối tượng`. Cùng nhau bắt đầu bài tiếp theo nhé 😁😁😁

# **NỘI DUNG CHÍNH**

## **1. QUAN HỆ TỔNG HỢP (AGGREGATION)**

`Quan hệ tổng hợp` (Aggregation) là mối quan hệ `has-a` trong `lập trình hướng đối tượng` (OOP) còn được gọi là mối quan hệ `thành phần - tổng thể,` trong đó các `đối tượng thành phần` có quan hệ` kém chặt chẽ` với` đối tượng toàn thể`. Nhưng đổi lại là các `đối tượng thành phần` có thể `tồn tại độc lập` nếu `đối tượng toàn thể` bị hủy.

Trong Python, `quan hệ tổng hợp` được triển khai bằng cách sử dụng các đối tượng `thành phần` làm `thuộc tính` của `lớp toàn thể`. Điều này giúp thiết kế mã rõ ràng và dễ mở rộng.


```python
# Lớp thành phần phần
class Book:
  def __init__(self, name, author):
    self.name = name
    self.author = author

  def __str__(self):
    return f"Book: {self.name:<20} {self.author}\n"

# Lớp tổng thể
class Libary:

  def __init__(self, name: str, books: list[Book]):
    self.name = name
    # Thuộc tính books của libary được truyền từ lớp thành phần Book vào
    # để triễn khai
    self.books = books

  def __str__(self):
    count = 1
    result = ""
    print(f"===List of book at {self.name}===")
    for book in self.books:
      result += f"{count}: {book}"
      count += 1

    return result

# Tạo một list books
books = [Book("Lập trình Java", "Unknow"), Book("Lập trình Python", "Unknow"), Book("Lập trình C#", "Unknow")]

libary = Libary("Bunbo", books)
print(libary)

# Xóa libary
del libary

# books vẫn tồn tại bình thường
for book in books:
  print(book, end = "")
```

    ===List of book at Bunbo===
    1: Book: Lập trình Java       Unknow
    2: Book: Lập trình Python     Unknow
    3: Book: Lập trình C#         Unknow
    
    Book: Lập trình Java       Unknow
    Book: Lập trình Python     Unknow
    Book: Lập trình C#         Unknow
    

## **2. QUAN HỆ HỢP THÀNH (COMPOSITION)**

`Quan hệ hợp thành` (Composition) là một dạng của mối quan hệ `has-a`, với sự phụ thuộc chặt chẽ giữa `đối tượng toàn thể` và `đối tượng thành phần`. Khác với `quan hệ tổng hợp`, nếu `đối tượng toàn thể` bị hủy, các `đối tượng thành phần` cũng bị hủy.

Khác với `quan hệ tổng hợp` (aggregation), nơi `đối tượng thành phần` có thể tồn tại độc lập, `quan hệ hợp thành` yêu cầu `đối tượng thành phần` phụ thuộc hoàn toàn vào `đối tượng toàn thể`. So với `kế thừa` (mối quan hệ `is-a`), `composition` tập trung vào sự sở hữu chặt chẽ.

Trong Python, `quan hệ hợp thành` được triển khai bằng cách sử dụng `đối tượng thành phần` làm `thuộc tính` của `lớp toàn thể`. Điều này tăng tính `đóng gó` và `quản lý tài nguyên` hiệu quả trong thiết kế mã.

📌 **Lưu ý**
Khi triễn khai `Aggregation` và `Composition` đều cần sử dụng `đối tượng thành phần` làm `thuộc tính` của `lớp toàn thể`. Nhưng `Aggregation` là phải truyền từ ngoài vào thông qua tham số ở `hàm khởi tạo`, còn `Composition` phải tạo đối tượng ngay trong `hàm khởi tạo`

```python
# Lớp thành phần
class Book:
  def __init__(self, name, author):
    self.name = name
    self.author = author
  
# Lớp tổng thể
class Libary:
  # Đây là cách làm của Aggregation
  def __init__(self, name: str, books: list[Book]):
    self.name = name
    self.books = books


# Lớp tổng thể
class Libary:
  # Đây là cách làm của Composition
  def __init__(self, name: str):
    self.name = name
    self.books: list[Book] = []
```


```python
# Lớp thành phần
class Heart:
  def __init__(self):
    self.beats = True

# Lớp tổng thể
class Human:
  def __init__(self, name: str):
    self.name = name
    # Human tạo ra heart
    self.heart = Heart()

  def __str__(self):
    return f"Tôi tên là {self.name} và tim tôi còn đập!!!"
# Mỗi còn người điều có trái tim (quan hệ 'has - a')
# Nếu người chết thì trái tim cũng đi theo (quan hệ 'chủ -tớ')

human = Human("Khang")
print(human)

del human
# Sau khi xóa human, trái tim cũng không còn tồn tại nữa
```

    Tôi tên là Khang và tim tôi còn đập!!!
    

# HOÀN THÀNH SERIES RỒI!!! 🥳

Cảm ơn các bạn đã đồng hành cùng tôi trong chuỗi bài viết về `Lập trình Hướng đối tượng (OOP)`. Series này đã giới thiệu những khái niệm nền tảng như `Encapsulation (Đóng gói)`, `Inheritance (Kế thừa)`, `Polymorphism (Đa hình)`, `Abstraction (Trừu tượng hóa)`, cùng các kiến thức mở rộng như `Property`, `Multiple Inheritance`, `Type Hints`, `Duck Typing`, `Aggregation` và `Composition`...

Để thành thạo `lập trình hướng đối tượng` (OOP), ngoài việc nắm vững các nguyên tắc cơ bản như `kế thừa`, `đa hình`, `trừu tượng`, bạn cần tìm hiểu các chủ đề nâng cao như `Design Patterns` và `SOLID Principles`. Bên cạnh đó `**Hãy thường xuyên thực hành qua các dự án thực tế`**` như:

- Xây dựng hệ thống quản lý (quản lý sinh viên, quản lý sách)  
- Phát triển ứng dụng web (xây dựng các model)    

Cuối cùng, nếu có bất kỳ thiếu sót hay sai sót nào , mong các bạn thông cảm và góp ý để tôi có thể cải thiện trong tương lai! 😊

**Một lần nữa, xin chân thành cảm ơn và chúc các bạn thành công!** 🚀
