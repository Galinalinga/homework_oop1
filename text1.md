
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