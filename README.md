class Student {
    private String lastName;
    private String firstName;
    private int grade;
    private int classroom;
    private int bus;

    public Student(String lastName, String firstName, int grade, int classroom, int bus) {
        this.lastName = lastName;
        this.firstName = firstName;
        this.grade = grade;
        this.classroom = classroom;
        this.bus = bus;
    }

    public String getLastName() {
        return lastName;
    }

    public String getFirstName() {
        return firstName;
    }

    public int getGrade() {
        return grade;
    }

    public int getClassroom() {
        return classroom;
    }

    public int getBus() {
        return bus;
    }

    @Override
    public String toString() {
        return "Учень: " + firstName + " " + lastName + ", Клас: " + grade + ", Класна кімната: " + classroom + ", Автобус: " + bus;
    }
}
class Teacher {
    private String lastName;
    private String firstName;
    private int classroom;

    public Teacher(String lastName, String firstName, int classroom) {
        this.lastName = lastName;
        this.firstName = firstName;
        this.classroom = classroom;
    }

    public String getLastName() {
        return lastName;
    }

    public String getFirstName() {
        return firstName;
    }

    public int getClassroom() {
        return classroom;
    }
}

import java.io.*;
import java.util.*;

public class SchoolSearch {
    private static List<Student> students = new ArrayList<>();
    private static List<Teacher> teachers = new ArrayList<>();

    public static void main(String[] args) {
        loadStudents("list.txt");
        loadTeachers("teachers.txt");

        Scanner scanner = new Scanner(System.in);
        while (true) {
            showMenu();
            String input = scanner.nextLine().trim();

            switch (input) {
                case "1":
                    System.out.println("Введіть прізвище учня:");
                    String lastName = scanner.nextLine().trim();
                    searchStudentByLastName(lastName);
                    break;
                case "2":
                    System.out.println("Введіть прізвище учня для пошуку автобусного маршруту:");
                    lastName = scanner.nextLine().trim();
                    searchStudentBusByLastName(lastName);
                    break;
                case "3":
                    System.out.println("Введіть прізвище викладача:");
                    String teacherLastName = scanner.nextLine().trim();
                    searchStudentsByTeacher(teacherLastName);
                    break;
                case "4":
                    System.out.println("Введіть номер автобусного маршруту:");
                    int busNumber = Integer.parseInt(scanner.nextLine().trim());
                    searchStudentsByBus(busNumber);
                    break;
                case "5":
                    System.out.println("Введіть номер класу:");
                    int classNumber = Integer.parseInt(scanner.nextLine().trim());
                    searchStudentsByClass(classNumber);
                    break;
                case "6":
                    addStudent();
                    break;
                case "7":
                    addTeacher();
                    break;
                case "8":
                    showStatistics();
                    break;
                case "9":
                    saveData("students.txt", "teachers.txt");
                    break;
                case "10":
                    System.out.println("Вихід з програми.");
                    return;
                default:
                    System.out.println("Невірна команда. Спробуйте ще раз.");
            }
        }
    }

    private static void showMenu() {
        System.out.println("\nСистема пошуку учнів. Доступні команди:");
        System.out.println("1. Пошук учня за прізвищем");
        System.out.println("2. Пошук автобусного маршруту за прізвищем учня");
        System.out.println("3. Пошук учнів за прізвищем викладача");
        System.out.println("4. Пошук учнів за номером автобуса");
        System.out.println("5. Пошук учнів за номером класу");
        System.out.println("6. Додати нового учня");
        System.out.println("7. Додати нового викладача");
        System.out.println("8. Показати статистику");
        System.out.println("9. Зберегти дані");
        System.out.println("10. Вихід з програми");
        System.out.print("Введіть номер команди: ");
    }

    private static void addStudent() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Введіть прізвище студента:");
        String lastName = scanner.nextLine().trim();
        System.out.println("Введіть ім'я студента:");
        String firstName = scanner.nextLine().trim();
        System.out.println("Введіть клас:");
        int grade = Integer.parseInt(scanner.nextLine().trim());
        System.out.println("Введіть номер класної кімнати:");
        int classroom = Integer.parseInt(scanner.nextLine().trim());
        System.out.println("Введіть номер автобуса:");
        int bus = Integer.parseInt(scanner.nextLine().trim());

        students.add(new Student(lastName, firstName, grade, classroom, bus));
    }

    private static void addTeacher() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Введіть прізвище викладача:");
        String lastName = scanner.nextLine().trim();
        System.out.println("Введіть ім'я викладача:");
        String firstName = scanner.nextLine().trim();
        System.out.println("Введіть номер класної кімнати:");
        int classroom = Integer.parseInt(scanner.nextLine().trim());

        teachers.add(new Teacher(lastName, firstName, classroom));
    }

    private static void showStatistics() {
        System.out.println("Кількість студентів: " + students.size());
        System.out.println("Кількість викладачів: " + teachers.size());
    }

    private static void saveData(String studentsFile, String teachersFile) {
        try (BufferedWriter studentWriter = new BufferedWriter(new FileWriter(studentsFile))) {
            for (Student s : students) {
                studentWriter.write(s.getLastName() + "," + s.getFirstName() + "," + s.getGrade() + "," + s.getClassroom() + "," + s.getBus());
                studentWriter.newLine();
            }
            System.out.println("Дані студентів збережені у " + studentsFile);
        } catch (IOException e) {
            System.out.println("Помилка збереження даних студентів.");
        }

        try (BufferedWriter teacherWriter = new BufferedWriter(new FileWriter(teachersFile))) {
            for (Teacher t : teachers) {
                teacherWriter.write(t.getLastName() + "," + t.getFirstName() + "," + t.getClassroom());
                teacherWriter.newLine();
            }
            System.out.println("Дані викладачів збережені у " + teachersFile);
        } catch (IOException e) {
            System.out.println("Помилка збереження даних викладачів.");
        }
    }

    private static void loadStudents(String fileName) {
        try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] data = line.split(",\\s*");
                students.add(new Student(data[0], data[1], Integer.parseInt(data[2]), Integer.parseInt(data[3]), Integer.parseInt(data[4])));
            }
        } catch (IOException e) {
            System.out.println("Помилка читання файлу " + fileName);
        }
    }

    private static void loadTeachers(String fileName) {
        try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] data = line.split(",\\s*");
                teachers.add(new Teacher(data[0], data[1], Integer.parseInt(data[2])));
            }
        } catch (IOException e) {
            System.out.println("Помилка читання файлу " + fileName);
        }
    }

    private static void searchStudentByLastName(String lastName) {
        boolean found = false;
        for (Student s : students) {
            if (s.getLastName().equalsIgnoreCase(lastName)) {
                System.out.printf("Учень: %s %s, Клас: %d, Класна кімната: %d, Автобус: %d\n",
                        s.getFirstName(), s.getLastName(), s.getGrade(), s.getClassroom(), s.getBus());
                found = true;
            }
        }
        if (!found) {
            System.out.println("Учень з прізвищем " + lastName + " не знайдений.");
        }
    }


    private static void searchStudentBusByLastName(String lastName) {
        boolean found = false;
        for (Student s : students) {
            if (s.getLastName().equalsIgnoreCase(lastName)) {
                System.out.printf("Учень: %s %s, Автобусний маршрут: %d\n", s.getFirstName(), s.getLastName(), s.getBus());
                found = true;
            }
        }
        if (!found) {
            System.out.println("Учень з прізвищем " + lastName + " не знайдений.");
        }
    }

    private static void searchStudentsByTeacher(String teacherLastName) {
        boolean found = false;
        for (Teacher t : teachers) {
            if (t.getLastName().equalsIgnoreCase(teacherLastName)) {
                for (Student s : students) {
                    if (s.getClassroom() == t.getClassroom()) {
                        System.out.printf("Учень: %s %s, Клас: %d, Класна кімната: %d, Автобус: %d\n",
                                s.getFirstName(), s.getLastName(), s.getGrade(), s.getClassroom(), s.getBus());
                        found = true;
                    }
                }
            }
        }
        if (!found) {
            System.out.println("Учні з викладачем з прізвищем " + teacherLastName + " не знайдені.");
        }
    }



    private static void searchStudentsByBus(int busNumber) {
        boolean found = false;
        for (Student s : students) {
            if (s.getBus() == busNumber) {
                System.out.printf("Учень: %s %s, Клас: %d, Класна кімната: %d, Автобус: %d\n",
                        s.getFirstName(), s.getLastName(), s.getGrade(), s.getClassroom(), s.getBus());
                found = true;
            }
        }
        if (!found) {
            System.out.println("Учні на автобусному маршруті " + busNumber + " не знайдені.");
        }
    }


    private static void searchStudentsByClass(int classNumber) {
        boolean found = false;
        for (Student s : students) {
            if (s.getGrade() == classNumber) { // Перевірка на клас
                System.out.printf("Учень: %s %s, Клас: %d, Класна кімната: %d, Автобус: %d\n",
                        s.getFirstName(), s.getLastName(), s.getGrade(), s.getClassroom(), s.getBus());
                found = true;
            }
        }
        if (!found) {
            System.out.println("Учні з класом " + classNumber + " не знайдені.");
        }
    }

}
