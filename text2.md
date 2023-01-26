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

first_lecturer = Lecturer('Петр', 'Иванов')
first_lecturer.courses_attached += ['Python']
print(f'Лектор {first_lecturer.name} {first_lecturer.surname}, курс {first_lecturer.courses_attached}')
#

first_reviewer = Reviewer('Мария', 'Петрова')
first_reviewer.courses_attached += ['Python']
print(f'Эксперт {first_reviewer.name} {first_reviewer.surname}, курс {first_reviewer.courses_attached}')

some_student = Student('Вася', 'Пупкин', 'м')
some_student.courses_in_progress += ['Python']
print(f'Студент {some_student.name} {some_student.surname}, курс {some_student.courses_in_progress}')

first_reviewer.rate_hw(some_student, 'Python', 9)
first_reviewer.rate_hw(some_student, 'Python', 8)
first_reviewer.rate_hw(some_student, 'Python', 10)
print(f'Оценки, выставленные студенту на курсе {some_student.grades}')
#

some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
some_student.rate_hw(first_lecturer, 'Python', 10)
print(f'Оценки, выставленные лектору на курсе {first_lecturer.grades}')


