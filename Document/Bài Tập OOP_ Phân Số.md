## **CHỦ ĐỀ: XÂY DỰNG VÀ PHÁT TRIỂN LỚP ĐỐI TƯỢNG PHÂN SỐ**
Xây dựng hệ thống xử lý phân số.
Mỗi phân số bao gồm tử số và mẫu số, luôn được rút gọn về dạng tối giản. Hệ thống cần hỗ trợ:
1. Tạo phân số với giá trị `mặc định` hoặc `nhập từ bàn phím`.
2. `Lấy` và `thay đổi giá trị` tử số, mẫu số.
3. `Rút gọn phân số tự động` sau khi nhập hoặc sau các phép toán.
4. Thực hiện các phép toán `cộng`, `trừ`, `nhân`, `chia` giữa hai phân số.
5. `So sánh hai phân số` với các phép so sánh: lớn hơn, nhỏ hơn, bằng, không bằng, lớn hơn hoặc bằng, nhỏ hơn hoặc bằng.
6. Thực hiện các `phép gán toán học`: +=, -=, *=, /=.
7. `Chuyển đổi phân số` sang `số thực` (double hoặc float).
8. (Tùy chọn nâng cao) Thực hiện phép toán giữa `phân số và một số nguyên`.

Chương trình phải `xử lý ngoại lệ` khi `mẫu số bằng 0`, đảm bảo chương trình `không bị dừng đột ngột` và `cho phép nhập lại` dữ liệu hợp lệ.

## **CODE**


```python
# Lớp phân số
class PhanSo:
  # Mặc định tử số là 0 và mẫu số là 1
  def __init__(self, tuso: int=0, mauso: int=1):
    self.__tuso = tuso
    # Kiểm tra mẫu số
    if PhanSo.isValid(mauso):
      self.__mauso = mauso
      self.rutgon()

  # =========== static method ================

  @staticmethod
  # Hàm tính ước chung lớn nhất
  def UCLN(a: int, b: int) -> int:
    if b == 0:
      return a if a != 0 else 1

    if a % b == 0:
      return b
    return PhanSo.UCLN(b, a % b)

  @staticmethod
  # Hàm Kiểm tra mẫu có đang là 0 hay không
  def isValid(mauso: int) -> bool:
    if mauso == 0:
      raise ValueError("Mẫu số phải lớn hơn không")
    return True

  # =========== overload str để in ra phân số ================
  def __str__(self):
    if self.tuso == 0:
      return "0"
    return f"{self.tuso}/{self.mauso}"

  # =========== property chuyển đổi phân số sang số thực (float) ===============
  @property
  # Hàm tính giá trị của phân số, ví dụ: 1/4 = 0.25
  def value(self):
    return self.tuso / self.mauso

  # =========== property tuso ================
  @property
  # Getter
  def tuso(self) -> int:
    return self.__tuso

  @tuso.setter
  # Setter
  def tuso(self, new_tuso: int) -> None:
    self.__tuso = new_tuso
    self.rutgon()

  # =========== property maso ================
  @property
  # Getter
  def mauso(self) -> int:
    return self.__mauso

  @mauso.setter
  # Setter
  def mauso(self, new_mauso: int) -> None:
    if PhanSo.isValid(new_mauso):
      self.__mauso = new_mauso
      self.rutgon()

  # =========== hàm nhập phân số ================
  def nhap(self):
    while True:
     try:
        self.tuso = int(input("Nhập tử số: "))
        self.mauso = int(input("Nhập mẫu số: "))
        # self.isValid(self.mauso) self.mauso là setter đã có bao gồm isValid rồi
        # nếu gọi thêm lần nữa sẽ thừa
        self.rutgon()
        break
     except ValueError as e:
        print("error: ",e)  # in thông báo lỗi
        print("Vui lòng nhập lại!")


  # =========== hàm rút gọn phân số ================
  def rutgon(self):
    if self.tuso == 0:
      return

    tuso = self.tuso
    mauso = self.mauso
    # Kiểm tra xem phân số có bao nhiêu số âm
    negative = 2

    # Số đầu vào bắt buộc là dương khi tính UCLN
    if tuso < 0:
      tuso *= -1
      negative -= 1

    if mauso < 0:
      mauso *= -1
      negative -= 1

    ucln = PhanSo.UCLN(tuso, mauso)

    # Tiến hành rút gọn phân số
    tuso = tuso // ucln
    mauso = mauso // ucln

    # Chỉ có tử số hoặc mẫu số là âm
    # ví dụ 3/-4 -> -3/4, -1/2 -> -1/2
    if negative == 1:
      tuso *= -1

    self.__tuso = tuso
    self.__mauso = mauso

  # ========== hàm phép tính + - * / ==============

  # Phép cộng
  def __add__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    tu_so = self.tuso * phanSo.mauso + phanSo.tuso * self.mauso
    mau_so = self.mauso * phanSo.mauso

    return PhanSo(tu_so, mau_so)

  # Phép trừ
  def __sub__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    tu_so = (self.tuso * phanSo.mauso) - (phanSo.tuso * self.mauso)
    mau_so = self.mauso * phanSo.mauso

    return PhanSo(tu_so, mau_so)

  # Phép nhân
  def __mul__(self, phanSo):
    if isinstance(phanSo, int):
      phanSo = PhanSo(phanSo)

    tu_so = self.tuso *  phanSo.tuso
    mau_so = self.mauso * phanSo.mauso
    return PhanSo(tu_so, mau_so)

  # Phép chia
  def __truediv__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    # Tử số bằng 0 sẽ đưa xuống mẫu khi chia -> trường hợp chia 0
    try:
        if phanSo.tuso == 0:
            raise ZeroDivisionError("Không thể chia cho phân số có giá trị bằng 0")

        tu_so = self.tuso * phanSo.mauso
        mau_so = self.mauso * phanSo.tuso
        return PhanSo(tu_so, mau_so)

    except ZeroDivisionError as e:
        print(f"error: {e}")
        return None

  # ========== hàm phép tính toán học +=, -=, *=, /= ==============

  # Phép cộng +=
  def __iadd__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    tu_so = self.tuso * phanSo.mauso + phanSo.tuso * self.mauso
    mau_so = self.mauso * phanSo.mauso

    self.__tuso = tu_so
    self.__mauso = mau_so
    self.rutgon()

    return self

  # Phép trừ -=
  def __isub__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    tu_so = self.tuso * phanSo.mauso - phanSo.tuso * self.mauso
    mau_so = self.mauso * phanSo.mauso

    self.__tuso = tu_so
    self.__mauso = mau_so
    self.rutgon()

    return self

  # Phép nhân *=
  def __imul__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    tu_so = self.tuso *  phanSo.tuso
    mau_so = self.mauso * phanSo.mauso

    self.__tuso = tu_so
    self.__mauso = mau_so
    self.rutgon()

    return self

  # Phép chia /=
  def __itruediv__(self, phanSo):
    if isinstance(phanSo, int):
       phanSo = PhanSo(phanSo)

    # Tử số bằng 0 sẽ đưa xuống mẫu khi chia -> trường hợp chia 0
    try:
        if phanSo.tuso == 0:
            raise ZeroDivisionError("Không thể chia cho phân số có giá trị bằng 0")

        tu_so = self.tuso * phanSo.mauso
        mau_so = self.mauso * phanSo.tuso

        self.__tuso = tu_so
        self.__mauso = mau_so

    except ZeroDivisionError as e:
        print(f"error: {e}")

    return self

  # ========== hàm toán tử so sánh ==============

  def __eq__(self, other): # =
    if isinstance(other, PhanSo):
      return self.value == other.value

  def __lt__(self, other): # <
    if isinstance(other, PhanSo):
      return self.value < other.value

  def __gt__(self, other): # >
    if isinstance(other, PhanSo):
      return self.value > other.value

  def __le__(self, other): # <=
    if isinstance(other, PhanSo):
      return self.value <= other.value

  def __ge__(self, other): # >=
    if isinstance(other, PhanSo):
      return self.value >= other.value
```

## **Test CODE**


```python
print("Khởi tạo mặc định")
a = PhanSo()
print(f"Phân số mặc định: {a}")
print("="*35)
####
print("Khởi tạo đầy đủ")
b = PhanSo(3, 4)
print(f"Phân số đầy đủ: {b}")
print("="*35)
#####
print("Khởi tạo bằng cách nhập phân số")
c = PhanSo()
c.nhap()
a = c
print(f"Phân số bằng cách nhập {c}")
print("="*35)
#####
print("So sánh phân số")
print(f"a = b = {a == b}")
print(f"a > b = {a > b}")
print(f"a < b = {a < b}")
print(f"a >= b = {a >= b}")
print(f"a <= b = {a <= b}")
print("="*35)
#####
print("Thực hiện phép tính")
print(f"a + b = {a + b}")
print(f"a - b = {a - b}")
print(f"a * b = {a * b}")
print(f"a / b = {a / b}")
print("="*35)
#####
print("Thực hiện phép tính toán học")
a += b
print(f"a + b = {a}")
a -= b
print(f"a - b = {a}")
a *= b
print(f"a * b = {a}")
a /= b
print(f"a / b = {a}")
print("="*35)
#####
print("Thực hiện phép tính với số nguyên")
songuyen = 5
print(f"a + {songuyen} = {a + songuyen}")
print(f"a - {songuyen} = {a - songuyen}")
print(f"a * {songuyen} = {a * songuyen}")
print(f"a / {songuyen} = {a / songuyen}")
print("="*35)
print(f"Kết quả chia với không: {b / PhanSo(0,5)}")
```

    Khởi tạo mặc định
    Phân số mặc định: 0/1
    ===================================
    Khởi tạo đầy đủ
    Phân số đầy đủ: 3/4
    ===================================
    Khởi tạo bằng cách nhập phân số
    Nhập tử số: 8
    Nhập mẫu số: 12
    Phân số bằng cách nhập 2/3
    ===================================
    So sánh phân số
    a = b = False
    a > b = False
    a < b = True
    a >= b = False
    a <= b = True
    ===================================
    Thực hiện phép tính
    a + b = 17/12
    a - b = -1/12
    a * b = 1/2
    a / b = 8/9
    ===================================
    Thực hiện phép tính toán học
    a + b = 17/12
    a - b = 2/3
    a * b = 1/2
    a / b = 4/6
    ===================================
    Thực hiện phép tính với số nguyên
    a + 5 = 17/3
    a - 5 = -13/3
    a * 5 = 10/3
    a / 5 = 2/15
    ===================================
    error: Không thể chia cho phân số có giá trị bằng 0
    Kết quả chia với không: None
    
