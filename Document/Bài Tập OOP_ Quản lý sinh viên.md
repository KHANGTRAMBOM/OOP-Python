## **Đề bài – [Hệ thống quản lý sinh viên](https://users.soict.hust.edu.vn/trungtt/uploads/tutorials/OOP%20Assignment%20Topics.pdf)**

Xây dựng hệ thống quản lý sinh viên tại một trường đại học. Hai loại sinh viên chính
mà trường quản lý là sinh viên học theo hệ tín chỉ và sinh viên học theo hệ niên chế.  
Đối với sinh viên tín chỉ, sinh viên cần phải đăng ký các môn học trong ngành học
của mình để tham gia vào khóa học. Các môn học có thể có các học phần điều kiện,
đòi hỏi sinh viên phải học trước khi có thể đăng ký học môn học này. Trong khi đó,
sinh viên niên chế phải học theo một danh sách các môn học được quy định trước
đối với lớp học đó. Mỗi môn học có điểm giữa kỳ và điểm cuối kỳ với trọng số tương
ứng với môn học đó.
Yêu cầu hệ thống phải có các chức năng quản lý sinh viên, đăng ký môn học, nhập
kết quả môn học cho sinh viên và xét điều kiện tốt nghiệp cho sinh viên.

## **PHÂN TÍCH ĐỀ BÀI**

### **Đối tượng yêu cầu**

1. `Student`: Đối tượng cơ bản và dễ nhìn thấy nhất bao gồm các thuộc tính: `name`, `age`, `id`, `gpa`, `isgraduate`.
 * `CreditStudent`: là bản cụ thể của `Student`, kế thừa từ `Student`, nó sẽ có thêm thuộc tính `credits`, `enrollment_list`
 * `YearlyStudent`: kế thừa từ `Student`, sé có thêm thuộc tính `classroom` có kiểu là `ClassRoom`.
2. `Subject`: Nó sẽ bao gồm các thuộc tính: `id`, `name`, `mid_score`, `final_score`, `percent`, `credits` như đề bài đã yêu cầu
3. `Course`: mỗi khóa học sẽ bao gồm:`id`, `subject` và môn `required`, `number` Trong đó `subject` và `required` thuộc kiểu `Subject`
4. `Enrollment`: Dùng để quản lý `đăng ký môn học`, sẽ bao gồm các thuộc tính:
`id`, `student`, `course`, `status`, `grade`, `date`.
5. `ClassRoom`: sẽ bao gồm `id`, `name` và `course_list`, và mỗi năm, kì thì sinh viên thường niên `YearlyStudent` sẽ được add vào các `classroom` khác nhau. Về bản chất danh sách học phần (`course_list`) chính là danh sách đăng ký học phần (`enrollment_list`)
6. `System` dùng để triễn khai các class trên


### **Chức năng yêu cầu**
1. **Quản lý sinh viên**
   - Thêm, sửa, xóa thông tin sinh viên.
   - Phân loại sinh viên (tín chỉ hoặc niên chế).

2. **Đăng ký môn học**
   - Với sinh viên tín chỉ: kiểm tra điều kiện tiên quyết trước khi cho phép đăng ký.
   - Với sinh viên niên chế: tự động gán danh sách môn học theo lớp.

3. **Nhập kết quả học tập**
   - Nhập điểm giữa kỳ và cuối kỳ.
   - Tính điểm tổng kết dựa trên trọng số.

4. **Xét điều kiện tốt nghiệp**
   - Kiểm tra số tín chỉ hoặc số môn học bắt buộc đã hoàn thành.
   - Đảm bảo không còn môn học nợ hoặc điểm dưới chuẩn.

![Quản lý sinh viên](https://github.com/user-attachments/assets/1201ec75-d64f-404a-a689-b8a104b9ed2d)

## **CODE**
Vì đây là `bài tập lớn`, và tôi cũng chỉ `hoàn thành mới đây` thời gian `chưa đến 1 ngay` cho nên nếu có gì `sai sót` mong các bạn `góp ý` và `bỏ qua` 😓😓😓

### **Class Student**


```python
from datetime import date
class Student:
  # Hàm khởi tạo
  def __init__(self, name: str = "", age: int = 1, id: str = "", gpa: float = 0,):
    self.__id = id
    self.__name = name

    # Đảm bảo age luôn có giá trị
    if Student.ageValid(age):
        self.__age = age
    else:
        self.__age = 1

    # Đảm bảo gpa luôn có giá trị
    if Student.gpaValid(gpa):
      self.__gpa = gpa
    else:
      self.__gpa = 0

    self.__isgraduate = False

 #================ static method ================
  @staticmethod
  def gpaValid(gpa: float) -> bool:
    if gpa > 4 or gpa < 0:
     raise ValueError("GPA phải trong khoảng từ 0 - 4")
    return True

  @staticmethod
  def ageValid(age: int) -> bool:
    if age <= 0:
     raise ValueError("Age phải lớn hơn 0")
    return True

#================ overload str ===============
  def __str__(self):
    gradute = "Đã tốt nghiệp" if self.isgraduate else "Chưa tốt nghiệp"
    return f"ID: {self.id:<10} Name: {self.name:<20} Age: {self.age:<10} GPA: {self.gpa:<10.2f} Graduate: {gradute:<20}"

#================ Hàm nhập ================
  def nhap(self):
    self.id = input("Nhập MSSV: ")
    self.name = input("Nhập họ tên: ")
    while True:
        try:
            age = int(input("Nhập tuổi: "))
            if Student.ageValid(age):
              self.age = age
              break
        except ValueError as e:
          print(e)
          print("Vui lòng nhập lại !!!")

    while True:
        try:
            gpa = float(input("Nhập gpa: "))
            if Student.gpaValid(gpa):
              self.gpa = gpa
              break
        except ValueError as e:
          print(e)
          print("Vui lòng nhập lại !!!")

#================ Hàm sửa ================
  def sua(self):
    self.nhap()

#================ Hàm tốt nghiệp =============
  def checkGraduate(self):
    if self.gpa >= 5:
      self.isgraduate = True
    return self.isgraduate
#================ property ================

#======== isgraduate ========
  # getter
  @property
  def isgraduate(self):
   return self.__isgraduate

  # setter
  @isgraduate.setter
  def isgraduate(self, new_isgraduate: bool):
   self.__isgraduate = new_isgraduate

#======== name ========
  # getter
  @property
  def name(self):
   return self.__name

  # setter
  @name.setter
  def name(self, new_name: str):
   self.__name = new_name

#======== id ========

  # getter
  @property
  def id(self):
   return self.__id

  # setter
  @id.setter
  def id(self, new_id: str):
   self.__id = new_id

#======== age ========

  # getter
  @property
  def age(self):
   return self.__age

  # setter
  @age.setter
  def age(self, new_age: int):
   if Student.ageValid(new_age):
    self.__age = new_age

#======== gpa ========

  # getter
  @property
  def gpa(self):
   return self.__gpa

  # setter
  @gpa.setter
  def gpa(self, new_gpa: float):
   if Student.gpaValid(new_gpa):
    self.__gpa = new_gpa

```

### **Class Subject**


```python
# Môn học
class Subject:
  # Hàm khởi tạo
  total_subject: int = 0
  def __init__(self, name: str="", mid_score: float=0, final_score: float=0, percent: float=0.5, credits: int=1):
    self.__id = Subject.total_subject
    self.__name = name
    self.__midscore = 0
    self.__finalscore = 0
    self.__percent = 0.5
    self.__credits = 1

    # Đảm bảo các thuộc tính có điều kiện có giá trị khi khởi tạo
    if Subject.scoreValid(mid_score):
      self.__midscore = mid_score

    if Subject.scoreValid(final_score):
      self.__finalscore = final_score

    if Subject.percentValid(percent):
      self.__percent = percent

    if Subject.creditsValid(credits):
      self.__credits = credits

    Subject.total_subject += 1

 #================ overload str ================
  def __str__(self):
     return f"ID: {self.id:<5} Name: {self.name:<20} mid score: {self.midscore:<10.2f} final score: {self.finalscore:<10.2f} average: {self.calculate():<10.2f} percent: {self.percent:<10} credits: {self.credits:<10}"

 #================ hàm chức năng ================
  # Hàm nhập
  def nhap(self):
    self.name = input("Nhập tên môn học: ")

    while True:
      try:
        mid_score = float(input("Nhập điểm giữa kì: "))
        if Subject.scoreValid(mid_score):
          self.midscore = mid_score
          break
      except ValueError as v:
        print(v)
        print("Vui lòng nhập lại !!!")

    while True:
      try:
        final_score = float(input("Nhập điểm cuối kì: "))
        if Subject.scoreValid(final_score):
          self.finalscore = final_score
          break
      except ValueError as v:
        print(v)
        print("Vui lòng nhập lại !!!")


    while True:
      try:
        percent = float(input("Nhập tỉ lệ giữa các kì: "))
        if Subject.percentValid(percent):
          self.percent = percent
          break
      except ValueError as v:
        print(v)
        print("Vui lòng nhập lại !!!")


    while True:
      try:
        credits = int(input("Nhập số tín chỉ: "))
        if Subject.creditsValid(credits):
          self.credits = credits
          break
      except ValueError as v:
        print(v)
        print("Vui lòng nhập lại !!!")

  # Hàm sửa
  def sua(self):
    self.nhap()

  # Hàm tính diễm trung bình
  def calculate(self) -> float:
    result = (self.midscore * self.percent)
    result += (self.finalscore * (1 -self.percent))
    return result

 #================ staticmethod ================
  @staticmethod
  def scoreValid(score: int):
    if score < 0 or score > 10:
      raise ValueError("Điểm phải từ 0 - 10")
    return True

  @staticmethod
  def percentValid(percent: float):
    if percent < 0 or percent > 1:
      raise ValueError("Tỉ lệ phải từ 0 - 1")
    return True

  @staticmethod
  def creditsValid(credits: int):
    if credits <= 0:
      raise ValueError("Số tín chỉ phải lớn hơn 0")
    return True

 #================ property ================
  #======== id ========
  @property
  def id(self):
    return self.__id

 #======== name ========
  @property
  def name(self):
    return self.__name

  @name.setter
  def name(self, new_name: str):
    self.__name = new_name

 #======== mid_score ========
  @property
  def midscore(self):
    return self.__midscore

  @midscore.setter
  def midscore(self, new_score: float):
    if Subject.scoreValid(new_score):
      self.__midscore = new_score

 #======== final_score ========
  @property
  def finalscore(self):
    return self.__finalscore

  @finalscore.setter
  def finalscore(self, new_score: float):
    if Subject.scoreValid(new_score):
      self.__finalscore = new_score

 #======== percent ========
  @property
  def percent(self):
    return self.__percent

  @percent.setter
  def percent(self, new_percent: Floatnumber):
    if Subject.percentValid(new_percent):
      self.__percent = new_percent

 #======== credit ========
  @property
  def credits(self):
    return self.__credits

  @credits.setter
  def credits(self, new_credits: int):
    if Subject.creditsValid(new_credits):
      self.__credits = new_credits
```

### **Class Course**


```python
# Lớp Học Phần
class Course:

  total_course :int = 0

  def __init__(self, subject: Subject, required: Subject=None, number: int=100):
    self.__id = Course.total_course
    self.__subject = subject
    self.__required = required
    self.__number = 100
    if Course.numberValid(number):
      self.__number = number

    Course.total_course +=1
 #============= overload str ===============
  def __str__(self):
    required_name = self.required.name if self.required else "Không yêu cầu"
    return f"ID: {self.id:<5} Khóa học: {self.subject.name:<30} môn học yêu cầu: {required_name:<30} Số lượng: {self.number:<10} Số tín chỉ: {self.subject.credits:<10}"

 #============= staticmethod===============

  @staticmethod
  def numberValid(number: int):
    if number <= 0:
      raise ValueError("Số lượng phải lớn hơn 0")
    return True

 #============= hàm chức năng===============
  def sua(self, subject: Subject, required: Subject):
    self.__subject = subject
    self.__required = required

 #============= property ===============
 #======id=========
  @property
  def id(self):
   return self.__id
 #======subject=========
  @property
  def subject(self):
   return self.__subject

  @subject.setter
  def subject(self, new_name: Subject):
   self.__subject = new_name

 #======required=========
  @property
  def required(self):
   return self.__required

  @required.setter
  def required(self, new_required: Subject):
   self.__required = new_required

 #======number=========
  @property
  def number(self):
   return self.__number

  @number.setter
  def number(self, new_number: int):
   if Course.numberValid(new_number):
      self.__number = new_number

```

### **Class Classroom**


```python
# Lớp lớp học
class ClassRoom:
  total_classroom :int = 0
  def __init__(self, name: str, enrollment_list):
    self.__id = ClassRoom.total_classroom
    self.__name = name
    self.__enrollment_list = enrollment_list
    ClassRoom.total_classroom += 1

  #============== overload str ====================
  def __str__(self):
    return f"{self.name:<10}"

  #==============property====================
  #=======id=========
  @property
  def id(self):
    return self.__id

 #=======name=========
  @property
  def name(self):
    return self.__name

  @name.setter
  def name(self, new_name):
    self.__name = new_name
 #=======enrollment_list=========
  @property
  def enrollment_list(self):
    return self.__enrollment_list

  @enrollment_list.setter
  def enrollment_list(self, new_enrollment_list):
    self.__enrollment_list = new_enrollment_list
```

### **Class Enrollment**


```python
# Class đăng ký học phần
class Enrollment:
  total_enroll :int = 0
  def __init__(self, student: Student, course: Course):
    self.__id = Enrollment.total_enroll
    self.__student = student
    self.__course = course
    self.__date = date.today()
    self.__status = "Registed"
    self.__grade = 0
    self.course.number -= 1
    Enrollment.total_enroll += 1

#==============hàm chức năng====================
  # Nhập điểm
  def nhap_diem(self):
    print(f"============= {self.student.name} : {self.course.subject.name} =============")
    self.course.subject.midscore = float(input("Điểm giữa kì: "))
    self.course.subject.finalscore = float(input("Điểm cuối kì: "))
    average = self.course.subject.calculate()
    if average < 5:
      self.status = "Fail"
    else:
      self.status = "Pass"
    self.grade = average

#================ static method ================
  @staticmethod
  def gradeValid(gpa: float) -> bool:
    if gpa < 0 or gpa > 10:
     raise ValueError("Grade phải nằm trong từ 0 - 10")
    return True

#============== overload str ====================
  def __str__(self):
    return f"Sinh viên: {self.student.name:<20} Tên học phần: {self.course.subject.name:<30} Đăng ký lúc: {self.date.strftime('%d/%m/%Y'):<15} Điểm: {self.grade:<10.2f}Trạng thái: {self.status:<10}"

#==============property====================
 #=======id=========
  @property
  def id(self):
    return self.__id

 #=======grade=========
  @property
  def grade(self):
    return self.__grade

  @grade.setter
  def grade(self, new_grade):
    if Enrollment.gradeValid(new_grade):
      self.__grade = new_grade


 #=======student=========
  @property
  def student(self):
    return self.__student

  @student.setter
  def student(self, new_student):
    self.__student = new_student

 #=======course=========
  @property
  def course(self):
    return self.__course

  @course.setter
  def course(self, new_course):
    self.__course = new_course

 #======= date =========
  @property
  def date (self):
    return self.__date

  @date.setter
  def date (self, new_date ):
    self.__date  = new_date

#======= date =========
  @property
  def status (self):
    return self.__status

  @status.setter
  def status (self, new_status ):
    self.__status  = new_status
```

### **Credit Student**


```python
# Sinh viên tín chỉ
class CreditStudent(Student):
  GRADUATE_THRESHOLD = 3

  def __init__(self, name: str = "", age: int = 1, id: str = "", gpa: float = 0, enrollment_list=None, credits: int = 0):
      super().__init__(name, age, id, gpa)

      if enrollment_list:
        self.__enrollment_list = enrollment_list
      else:
        self.__enrollment_list = []

      # Đảm bảo credit luôn có giá trị
      self.__credits = 0
      if CreditStudent.creditsValid(credits):
          self.__credits = credits


 #================ hàm chức năng ================
  # Đăng ký môn
  def dang_ky_mon(self, course):
    # Kiễm tra đáp ứng học phần điều kiện
    subject_required = course.required

    # Kiễm tra điều kiện tiên quyết
    if subject_required:
      passed = [e.course.subject for e in self.enrollment_list if e.status == "Pass"]
      if subject_required not in passed:
        print(f"Bạn chưa hoàn thành hoặc chưa có tín chỉ {subject_required} để có thể đăng ký môn học này")
        return False

    # Kiễm tra đã đăng ký chưa
    for enrollment in self.enrollment_list:
      if course == enrollment.course:
        print(f"Đã đăng ký môn {course.subject.name} rồi !!!")
        return False

    self.enrollment_list.append(Enrollment(self, course))
    return True

  # Xem môn đã đăng ký
  def xem_mon_dang_ky(self):
    result = "="*40 + "Enrollment List" + "="*45 + "\n"
    for enrollment in self.enrollment_list:
      result += enrollment.__str__() + "\n"
    print(result)

  # Kiễm tra điều kiện tốt nghiệp
  def checkGraduate(self):
    self.cap_nhat_diem_va_tin_chi()
    if self.credits >= self.GRADUATE_THRESHOLD:
        self.isgraduate = True
        return True

    return False

  # Cập nhật lại điễm và tín chỉ
  def cap_nhat_diem_va_tin_chi(self):
    total_credit = 0
    total_grade = 0

    # Tính điểm và tín chỉ
    for enrollment in self.enrollment_list:
        if enrollment.status == "Pass":
          credit = enrollment.course.subject.credits
          total_credit += credit
          total_grade  += enrollment.grade * credit

    self.credits = total_credit
    if total_credit > 0:
      self.gpa = (((total_grade) / total_credit) * 4) / 10

 #================ staticmethod ================

  @staticmethod
  def creditsValid(credits: int):
    if credits < 0:
      raise ValueError("Số tín chỉ phải lớn hơn 0")
    return True


 #================ property ================
 #======= credits =========
  @property #getter
  def credits(self):
    return self.__credits

  @credits.setter #setter
  def credits(self, new_credits: int):
    if CreditStudent.creditsValid(new_credits):
        self.__credits = new_credits

  #======= enrollment_list =========
  @property #getter
  def enrollment_list(self):
    return self.__enrollment_list

  @enrollment_list.setter #setter
  def enrollment_list(self, new_enrollment_list):
    self.__enrollment_list = new_enrollment_list

 #================ override ================

  # Hàm in
  def __str__(self):
    result = super().__str__()
    result += f"Collection of credits: {self.credits}"
    return result

  # Hàm nhập
  def nhap(self):
    super().nhap()
    while True:
      try:
        credits = int(input("Nhập số tín chỉ: "))
        if CreditStudent.creditsValid(credits):
          self.credits = credits
          break
      except ValueError as v:
        print(v)
        print("Vui lòng nhập lại !!!")

  # Hàm sửa
  def sua(self):
    self.nhap()
```

### **Yearly Student**


```python
# Sinh viên thường niên
class YearlyStudent(Student):
  def __init__(self, name: str = "", age: int = 1, id: str = "", gpa: float = 0, classroom=None):
      super().__init__(name, age, id, gpa)
      self.__classroom = classroom
      self.__enrollment_list = []

      if classroom:
        for course in classroom.enrollment_list:
          enrollment = Enrollment(self, course)
          self.__enrollment_list.append(enrollment)

 #================ hàm chức năng ================
  # Xem môn
  def xem_mon(self):
    result = "="*40 + "Enrollment List" + "="*45 + "\n"
    for enrollment in self.enrollment_list:
      result += enrollment.__str__() + "\n"
    print(result)

  # Cập nhật lại điễm và tín chỉ
  def cap_nhat_diem_va_tin_chi(self):
    # Số lượng tín chỉ cần tốt nghiệp
    total_credit = 0
    total_grade = 0
    for enrollment in self.enrollment_list:
      # Nếu qua môn thì cộng tín chỉ vào
      if enrollment.status == "Pass":
        credit = enrollment.course.subject.credits
        total_credit += credit
        total_grade += enrollment.grade * credit

    # Cập nhật lại GPA
    if total_credit > 0:
      self.gpa = (total_grade / total_credit) * 4 / 10

  # Kiễm tra điều kiện tốt nghiệp
  def checkGraduate(self):
    self.cap_nhat_diem_va_tin_chi()
    for enrollment in self.enrollment_list:
      # Nếu có một môn chưa đậu thì chưa tốt nghiệp được
      if enrollment.status == "Fail" or enrollment.status ==  "Registed":
        return False

    self.isgraduate = True
    return True
 #================ property ================
 #===== enrollment_list =======

  @property #getter
  def enrollment_list(self):
    return self.__enrollment_list

 #===== classroom =======

  @property #getter
  def classroom(self):
    return self.__classroom

  @classroom.setter #setter
  def classroom(self, new_classroom):
    self.__classroom = new_classroom
    if self.__classroom:
        for course in self.__classroom.enrollment_list:
          enrollment = Enrollment(self, course)
          self.__enrollment_list.append(enrollment)

 #================ override ================
  # Hàm in
  # def __str__(self):
  #   result = super().__str__()
  #   return result

  # Hàm nhập
  def nhap(self):
    super().nhap()

  # Hàm sửa
  def sua(self):
    self.nhap()
```

### **Class System**


```python
class System:
  def __init__(self, students: list, courses: list):
    self.__students = students
    self.__courses = courses

  # ========= property =============
  @property
  def students(self):
    return self.__students

  @students.setter
  def students(self, new_students):
    self.__students = new_students

  @property
  def courses(self):
    return self.__courses

  @courses.setter
  def courses(self, new_courses):
    self.__courses = new_courses


  # ========== THÊM SINH VIÊN VÀO HỆ THỐNG ================
  def them_sinh_vien(self, student):
    self.students.append(student)

  # ========== TÌM KIẾM SINH VIÊN ================
  def tim_sinh_vien(self, id: int):
    for student in self.students:
      if student.id == id:
        return student
    print("Không tìm thấy sinh viên !!!")
    return None

  # ========== TÌM KIẾM KHÓA HỌC ================
  def tim_khoa_hoc(self, course):
    for c in  self.courses:
      if c.id == course.id:
        return True
    return False

  # ========== IN TỔNG QUÁT ================
  def ham_in(self, tieu_de: str, data):
    print("="*56  + tieu_de + "="*57)
    for student in data:
      print(end="| ")
      if isinstance(student, CreditStudent):
        print(student, end =" |\n")
      else:
        print(f"{student}                         |")
    print("="*132)

  # ========== IN DANH SÁCH SINH VIÊN ================
  def danh_sach_sinh_vien(self):
    self.ham_in("Danh sách sinh viên",self.students)

  # ========== IN CHI TIẾT SINH VIÊN================
  def chi_tiet_sinh_vien(self, id: str):
    student = self.tim_sinh_vien(id)
    # Nếu sinh viên tồn tại
    if student:
      self.ham_in("Chi tiết sinh viên", [student])
      for enrollment in student.enrollment_list:
        print(enrollment)
      return

    print("Sinh viên không tồn tại !!!")

  # ========== NHẬP ĐIỂM ================
  def nhap_diem(self, id: str=None):
    # Nhập theo MSSV
    if id:
      student = self.tim_sinh_vien(id)
      if student:
        for enrollment in student.enrollment_list:
          if enrollment.status == "Registed":
            enrollment.nhap_diem()
        student.cap_nhat_diem_va_tin_chi()
      else:
        print("Sinh viên không tồn tại !!!")
    # Nhập cho toàn trường
    else:
      for student in self.students:
        for enrollment in student.enrollment_list:
          if enrollment.status == "Registed":
                enrollment.nhap_diem()
        student.cap_nhat_diem_va_tin_chi()


  # ========== XÉT TỐT NGHIỆP ================
  def xet_tot_nghiep(self, id: str=None):
    # Xét tốt nghiêp cho sinh viên theo mã
    if id:
      student = self.tim_sinh_vien(id)
      if student:
        student.checkGraduate()
        print(f"Đã xét tốt nghiệp cho sinh viên {student.name}")
      else:
        print("Sinh viên không tồn tại !!!")
    # Xết tốt nghiệp toàn trường
    else:
      graduted_list = []
      for student in self.students:
        if student.checkGraduate():
          graduted_list.append(student)

      if len(graduted_list) == 0:
        print("Không có sinh viên nào tốt nghiệp !!!")
      else:
        print("Đã xét tốt nghiệp thành công !!!")
        self.ham_in("Danh sách tốt nghiệp", graduted_list)

  # ========== ĐĂNG KÝ MÔN HỌC ================
  def dang_ky_mon_hoc(self, id, course=None,classroom=None):
    student = self.tim_sinh_vien(id)
    if student:
      # Sinh viên tín chỉ mới đăng ký theo môn
      if isinstance(student,CreditStudent):
        # Nếu khóa học tồn tại thì đăng ký
        if self.tim_khoa_hoc(course):
            # Không đáp ứng điều kiện đăng ký
            if not student.dang_ky_mon(course):
                return False
        else:
          print("Môn học không tồn tại !!!")
          return False
        print(f"Đã đăng ký môn học {course.subject.name} cho sinh viên {student.name}")
      # Sinh viên thường niên tự động đăng ký theo danh sách
      else:
        student.classroom = classroom
        print(f"Đã đăng ký môn học thành công cho sinh viên {student.name}")
      return True

    print("Sinh viên không tôn tại !!!")
    return False



# ======= Môn học (Subject) =======
math = Subject("Toán cao cấp", 0, 0, 0.6, 3)
physics = Subject("Vật lý đại cương", 0, 0, 0.5, 2)
programming = Subject("Lập trình Python",0, 0, 0.4,3)
oop = Subject("Lập trình hướng đối tượng",0, 0, 0.4, 4)
database = Subject("Cơ sở dữ liệu", 0, 0, 0.5, 3)


# ======= Học phần (Course) =======
course_math = Course(math, None)
course_physics = Course(physics, None)
course_programming = Course(programming, math)  # cần Toán cao cấp trước
course_oop = Course(oop, programming)           # cần Lập trình Python trước
course_database = Course(database, programming) # cần Lập trình Python trước

courses = [
    course_math,
    course_physics,
    course_programming,
    course_oop,
    course_database
]

# ======= Lớp học (ClassRoom) =======
# Ví dụ lớp "Khóa 22 CNTT" học 5 học phần
classroom_22 = ClassRoom("CNTT - Khóa 22", courses)

# ======= Sinh viên (Student) =======
a = CreditStudent("Nguyễn Văn A", 18, "SV001", 0)  # hệ tín chỉ
b = YearlyStudent("Trần Thị B", 18, "SV002", 0, classroom_22)  # hệ niên chế

students = [a,b]

# ======= Hệ thống =======
system = System(students, courses)
system.danh_sach_sinh_vien()
system.dang_ky_mon_hoc("SV001",course_math)
system.nhap_diem("SV002")
system.xet_tot_nghiep()
system.chi_tiet_sinh_vien("SV002")
```

    ERROR:root:Internal Python error in the inspect module.
    Below is the traceback from this internal error.
    
    

    Traceback (most recent call last):
      File "/usr/local/lib/python3.11/dist-packages/IPython/core/interactiveshell.py", line 3553, in run_code
        exec(code_obj, self.user_global_ns, self.user_ns)
      File "/tmp/ipython-input-2351436300.py", line 177, in <cell line: 0>
        system.nhap_diem("SV002")
      File "/tmp/ipython-input-2351436300.py", line 78, in nhap_diem
        enrollment.nhap_diem()
      File "/tmp/ipython-input-1556425881.py", line 18, in nhap_diem
        self.course.subject.midscore = float(input("Điểm giữa kì: "))
                                             ^^^^^^^^^^^^^^^^^^^^^^^
      File "/usr/local/lib/python3.11/dist-packages/ipykernel/kernelbase.py", line 1177, in raw_input
        return self._input_request(
               ^^^^^^^^^^^^^^^^^^^^
      File "/usr/local/lib/python3.11/dist-packages/ipykernel/kernelbase.py", line 1219, in _input_request
        raise KeyboardInterrupt("Interrupted by user") from None
    KeyboardInterrupt: Interrupted by user
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/usr/local/lib/python3.11/dist-packages/IPython/core/interactiveshell.py", line 2099, in showtraceback
        stb = value._render_traceback_()
              ^^^^^^^^^^^^^^^^^^^^^^^^
    AttributeError: 'KeyboardInterrupt' object has no attribute '_render_traceback_'
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py", line 1101, in get_records
        return _fixed_getinnerframes(etb, number_of_lines_of_context, tb_offset)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "/usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py", line 248, in wrapped
        return f(*args, **kwargs)
               ^^^^^^^^^^^^^^^^^^
      File "/usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py", line 281, in _fixed_getinnerframes
        records = fix_frame_records_filenames(inspect.getinnerframes(etb, context))
                                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "/usr/lib/python3.11/inspect.py", line 1739, in getinnerframes
        traceback_info = getframeinfo(tb, context)
                         ^^^^^^^^^^^^^^^^^^^^^^^^^
      File "/usr/lib/python3.11/inspect.py", line 1684, in getframeinfo
        filename = getsourcefile(frame) or getfile(frame)
                   ^^^^^^^^^^^^^^^^^^^^
      File "/usr/lib/python3.11/inspect.py", line 948, in getsourcefile
        module = getmodule(object, filename)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "/usr/lib/python3.11/inspect.py", line 988, in getmodule
        if ismodule(module) and hasattr(module, '__file__'):
                                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
    KeyboardInterrupt
    


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

        [... skipping hidden 1 frame]
    

    /tmp/ipython-input-2351436300.py in <cell line: 0>()
        176 system.dang_ky_mon_hoc("SV001",course_math)
    --> 177 system.nhap_diem("SV002")
        178 system.xet_tot_nghiep()
    

    /tmp/ipython-input-2351436300.py in nhap_diem(self, id)
         77           if enrollment.status == "Registed":
    ---> 78             enrollment.nhap_diem()
         79         student.cap_nhat_diem_va_tin_chi()
    

    /tmp/ipython-input-1556425881.py in nhap_diem(self)
         17     print(f"============= {self.student.name} : {self.course.subject.name} =============")
    ---> 18     self.course.subject.midscore = float(input("Điểm giữa kì: "))
         19     self.course.subject.finalscore = float(input("Điểm cuối kì: "))
    

    /usr/local/lib/python3.11/dist-packages/ipykernel/kernelbase.py in raw_input(self, prompt)
       1176             )
    -> 1177         return self._input_request(
       1178             str(prompt),
    

    /usr/local/lib/python3.11/dist-packages/ipykernel/kernelbase.py in _input_request(self, prompt, ident, parent, password)
       1218                 # re-raise KeyboardInterrupt, to truncate traceback
    -> 1219                 raise KeyboardInterrupt("Interrupted by user") from None
       1220             except Exception:
    

    KeyboardInterrupt: Interrupted by user

    
    During handling of the above exception, another exception occurred:
    

    AttributeError                            Traceback (most recent call last)

    /usr/local/lib/python3.11/dist-packages/IPython/core/interactiveshell.py in showtraceback(self, exc_tuple, filename, tb_offset, exception_only, running_compiled_code)
       2098                         # in the engines. This should return a list of strings.
    -> 2099                         stb = value._render_traceback_()
       2100                     except Exception:
    

    AttributeError: 'KeyboardInterrupt' object has no attribute '_render_traceback_'

    
    During handling of the above exception, another exception occurred:
    

    TypeError                                 Traceback (most recent call last)

        [... skipping hidden 1 frame]
    

    /usr/local/lib/python3.11/dist-packages/IPython/core/interactiveshell.py in showtraceback(self, exc_tuple, filename, tb_offset, exception_only, running_compiled_code)
       2099                         stb = value._render_traceback_()
       2100                     except Exception:
    -> 2101                         stb = self.InteractiveTB.structured_traceback(etype,
       2102                                             value, tb, tb_offset=tb_offset)
       2103 
    

    /usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py in structured_traceback(self, etype, value, tb, tb_offset, number_of_lines_of_context)
       1365         else:
       1366             self.tb = tb
    -> 1367         return FormattedTB.structured_traceback(
       1368             self, etype, value, tb, tb_offset, number_of_lines_of_context)
       1369 
    

    /usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py in structured_traceback(self, etype, value, tb, tb_offset, number_of_lines_of_context)
       1265         if mode in self.verbose_modes:
       1266             # Verbose modes need a full traceback
    -> 1267             return VerboseTB.structured_traceback(
       1268                 self, etype, value, tb, tb_offset, number_of_lines_of_context
       1269             )
    

    /usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py in structured_traceback(self, etype, evalue, etb, tb_offset, number_of_lines_of_context)
       1122         """Return a nice text document describing the traceback."""
       1123 
    -> 1124         formatted_exception = self.format_exception_as_a_whole(etype, evalue, etb, number_of_lines_of_context,
       1125                                                                tb_offset)
       1126 
    

    /usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py in format_exception_as_a_whole(self, etype, evalue, etb, number_of_lines_of_context, tb_offset)
       1080 
       1081 
    -> 1082         last_unique, recursion_repeat = find_recursion(orig_etype, evalue, records)
       1083 
       1084         frames = self.format_records(records, last_unique, recursion_repeat)
    

    /usr/local/lib/python3.11/dist-packages/IPython/core/ultratb.py in find_recursion(etype, value, records)
        380     # first frame (from in to out) that looks different.
        381     if not is_recursion_error(etype, value, records):
    --> 382         return len(records), 0
        383 
        384     # Select filename, lineno, func_name to track frames with
    

    TypeError: object of type 'NoneType' has no len()

