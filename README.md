# ДЗ  Задание № 1 Наследование

class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def add_courses(self, course_name):
        self.finished_courses.append(course_name)


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

class Lecturer(Mentor):
    pass

class Reviewer(Mentor):
    pass

print(Lecturer.mro())
print(Reviewer.mro())

first_lecturer = Lecturer('Петр', 'Иванов')
first_lecturer.courses_attached += ['Python']

print(f'Имя лектора - {first_lecturer.name}')
print(f'Фамилия лектора - {first_lecturer.surname}')
print(f'Преподает курс по {first_lecturer.courses_attached}')

first_reviewer = Reviewer('Мария', 'Петрова')
first_reviewer.courses_attached += ['Python']

print(f'Имя эксперта - {first_reviewer.name}')
print(f'Фамилия эксперта - {first_reviewer.surname}')
print(f'Проверяет работы по {first_reviewer.courses_attached}')



# # ДЗ  Задание № 2 Атрибуты и взаимодействие классов
#
class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def add_courses(self, course_name):
        self.finished_courses.append(course_name)

    def rate_hw(self, lector, course, grade):
        if isinstance(lector, Lecturer) and course in lector.courses_attached and course in self.courses_in_progress:
            if course in lector.grades:
                lector.grades[course] += [grade]
            else:
                lector.grades[course] = [grade]
        else:
            return 'Ошибка'

class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'
# # создали лектора
first_lecturer = Lecturer('Петр', 'Иванов')
first_lecturer.courses_attached += ['Python']
print(f'Лектор {first_lecturer.name} {first_lecturer.surname}, курс {first_lecturer.courses_attached}')
#
# # создали эксперта
first_reviewer = Reviewer('Мария', 'Петрова')
first_reviewer.courses_attached += ['Python']
print(f'Эксперт {first_reviewer.name} {first_reviewer.surname}, курс {first_reviewer.courses_attached}')
# # создали студента
some_student = Student('Вася', 'Пупкин', 'м')
some_student.courses_in_progress += ['Python']
print(f'Студент {some_student.name} {some_student.surname}, курс {some_student.courses_in_progress}')
# # эксперт выставляет оценки студенту
first_reviewer.rate_hw(some_student, 'Python', 9)
first_reviewer.rate_hw(some_student, 'Python', 8)
first_reviewer.rate_hw(some_student, 'Python', 10)
print(f'Оценки, выставленные студенту на курсе {some_student.grades}')
#
# # студент выставляет оценки лектору
some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
print(f'Оценки, выставленные лектору на курсе {first_lecturer.grades}')





# ДЗ  Задание № 3 Полиморфизм и магические методы

class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def add_courses(self, course_name):
        self.finished_courses.append(course_name)

    def rate_hw(self, lector, course, grade):
        if isinstance(lector, Lecturer) and course in lector.courses_attached and course in self.courses_in_progress:
            if course in lector.grades:
                lector.grades[course] += [grade]
            else:
                lector.grades[course] = [grade]
        else:
            return 'Ошибка'

    def average(self):
        total = 0
        counter = 0
        for value in self.grades.values():
            for i in value:
                total += i
                counter += 1
        if counter != 0:
            return round(total / counter, 2)

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за домашние задания: ' \
              f'{self.average()} \nКурсы в процессе изучения: {self.courses_in_progress}' \
              f'\nЗавершенные курсы: {self.finished_courses}'
        return res

    def __lt__(self, other):
        if self.average() > other.average():
            return f'Лучший студент - {self.name} {self.surname}'
        else:
            return  f'Лучший студент - {other.name} {other.surname}'


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def average(self):
        total = 0
        counter = 0
        for value in self.grades.values():
            for i in value:
                total += i
                counter += 1
        if counter != 0:
            res = round(total / counter, 2)
            return res

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за лекции: {self.average()}'
        return res

    def __lt__(self, other):
        if self.average() > other.average():
            return f'Лучший лектор - {self.name} {self.surname}'
        else:
            return f'Лучший лектор - {other.name} {other.surname}'


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname}'
        return res


first_lecturer = Lecturer('Петр', 'Иванов')
first_lecturer.courses_attached += ['Python']
second_lecturer = Lecturer('Ирина', 'Федорова')
second_lecturer.courses_attached += ['Python']

some_student = Student('Вася', 'Пупкин', 'м')
some_student.finished_courses += ['Введение в программирование']
some_student.courses_in_progress += ['Python']
some_student.courses_in_progress += ['Git']

another_student = Student('Витя', 'Комаров', 'м')
another_student.finished_courses += ['Введение в программирование']
another_student.courses_in_progress += ['Python']
another_student.courses_in_progress += ['Git']

first_reviewer = Reviewer('Мария', 'Петрова')
first_reviewer.courses_attached += ['Python']

print('Проверяющий:')
print(first_reviewer)
print()

first_reviewer.rate_hw(some_student, 'Python', 9)
first_reviewer.rate_hw(some_student, 'Python', 8)
first_reviewer.rate_hw(some_student, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)

first_reviewer.rate_hw(another_student, 'Python', 8)
first_reviewer.rate_hw(another_student, 'Python', 8)
first_reviewer.rate_hw(another_student, 'Python', 9)
some_student.rate_hw(second_lecturer, 'Python', 10)
some_student.rate_hw(second_lecturer, 'Python', 9)
some_student.rate_hw(second_lecturer, 'Python', 10)


print('Лекторы:')
print(first_lecturer)
print(second_lecturer)
print()
print('Студенты:')
print(some_student)
print(another_student)
print()
print(first_lecturer.__lt__(second_lecturer))
print(some_student.__lt__(another_student))

ДЗ  Задание № 4 Полевые испытания

class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def add_courses(self, course_name):
        self.finished_courses.append(course_name)

    def rate_hw(self, lector, course, grade):
        if isinstance(lector, Lecturer) and course in lector.courses_attached and course in self.courses_in_progress:
            if course in lector.grades:
                lector.grades[course] += [grade]
            else:
                lector.grades[course] = [grade]
        else:
            return 'Ошибка'

    def average(self):
        total = 0
        counter = 0
        for value in self.grades.values():
            for i in value:
                total += i
                counter += 1
        if counter != 0:
            return round(total / counter, 2)

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за домашние задания: ' \
              f'{self.average()} \nКурсы в процессе изучения: {self.courses_in_progress}' \
              f'\nЗавершенные курсы: {self.finished_courses}'
        return res

    def __lt__(self, other):
        if self.average() > other.average():
            return f'Лучший студент - {self.name} {self.surname}'
        else:
            return  f'Лучший студент - {other.name} {other.surname}'

    # def student_list(self):
    #     if self
    #     = [some_student, another_student]


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def average(self):
        total = 0
        counter = 0
        for value in self.grades.values():
            for i in value:
                total += i
                counter += 1
        if counter != 0:
            res = round(total / counter, 2)
            return res

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nЛекции по курсу: ' \
              f'{self.courses_attached} \nСредняя оценка за лекции: {self.average()}'
        return res

    def __lt__(self, other):
        if self.average() > other.average():
            return f'Лучший лектор - {self.name} {self.surname}'
        else:
            return f'Лучший лектор - {other.name} {other.surname}'


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nПроверка домашних заданий по курсу: ' \
              f'{self.courses_attached}'
        return res


first_lecturer = Lecturer('Петр', 'Иванов')
first_lecturer.courses_attached += ['Python']
second_lecturer = Lecturer('Ирина', 'Федорова')
second_lecturer.courses_attached += ['Git']

some_student = Student('Вася', 'Пупкин', 'м')
some_student.finished_courses += ['Введение в программирование']
some_student.finished_courses += ['English for IT']
some_student.courses_in_progress += ['Python']
some_student.courses_in_progress += ['Git']

another_student = Student('Витя', 'Комаров', 'м')
another_student.finished_courses += ['Введение в программирование']
another_student.courses_in_progress += ['Python']
another_student.courses_in_progress += ['Git']

first_reviewer = Reviewer('Мария', 'Петрова')
first_reviewer.courses_attached += ['Python']
second_reviewer = Reviewer('Ольга', 'Сидорова')
second_reviewer.courses_attached += ['Git']

print('Проверяющий:')
print(first_reviewer)
print(second_reviewer)
print()

first_reviewer.rate_hw(some_student, 'Python', 9)
first_reviewer.rate_hw(some_student, 'Python', 8)
first_reviewer.rate_hw(some_student, 'Python', 10)
second_reviewer.rate_hw(some_student, 'Git', 9)
second_reviewer.rate_hw(some_student, 'Git', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 9)
some_student.rate_hw(first_lecturer, 'Python', 10)
another_student.rate_hw(first_lecturer, 'Python', 10)
another_student.rate_hw(first_lecturer, 'Python', 10)

first_reviewer.rate_hw(another_student, 'Python', 8)
first_reviewer.rate_hw(another_student, 'Python', 8)
first_reviewer.rate_hw(another_student, 'Python', 9)
second_reviewer.rate_hw(another_student, 'Git', 7)
second_reviewer.rate_hw(another_student, 'Git', 10)

some_student.rate_hw(second_lecturer, 'Git', 10)
some_student.rate_hw(second_lecturer, 'Git', 9)
some_student.rate_hw(second_lecturer, 'Git', 10)
another_student.rate_hw(second_lecturer, 'Git', 9)
another_student.rate_hw(second_lecturer, 'Git', 10)

print('Лекторы:')
print(first_lecturer)
print(second_lecturer)
print()
print('Студенты:')
print(some_student)
print(another_student)
print()
print(first_lecturer.__lt__(second_lecturer))
print(some_student.__lt__(another_student))

student_list = [some_student.grades, another_student.grades]
lecturer_list = [first_lecturer.grades, second_lecturer.grades]
print(student_list)
print(lecturer_list)

# courses_in_progress = ['Python']
def average_rating_for_course(course, list_s):
    total = 0
    counter = 0
    for student in list_s:
        for course in student:
            students_rating = average_rating_for_course(list_s)
            total += students_rating # сумма всех оценок
            counter += 1 # количество оценок
            average_rating = round(total / counter, 2)
    return average_rating

average_rating_for_course('Git', student_list)