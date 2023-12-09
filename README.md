import java.util.*;

class Course {
    private String courseId;
    private String courseName;
    private int maxCapacity;
    private int currentCapacity;
    private Set<String> enrolledStudents;

    public Course(String courseId, String courseName, int maxCapacity) {
        this.courseId = courseId;
        this.courseName = courseName;
        this.maxCapacity = maxCapacity;
        this.currentCapacity = 0;
        this.enrolledStudents = new HashSet<>();
    }

    public String getCourseId() {
        return courseId;
    }

    public String getCourseName() {
        return courseName;
    }

    public int getMaxCapacity() {
        return maxCapacity;
    }

    public int getCurrentCapacity() {
        return currentCapacity;
    }

    public boolean enrollStudent(String studentId) {
        if (currentCapacity < maxCapacity) {
            enrolledStudents.add(studentId);
            currentCapacity++;
            return true;
        } else {
            System.out.println("Course is already full!");
            return false;
        }
    }

    public boolean dropStudent(String studentId) {
        if (enrolledStudents.remove(studentId)) {
            currentCapacity--;
            return true;
        } else {
            System.out.println("Student is not enrolled in this course!");
            return false;
        }
    }
}

class Student {
    private String studentId;
    private String studentName;
    private Set<String> enrolledCourses;

    public Student(String studentId, String studentName) {
        this.studentId = studentId;
        this.studentName = studentName;
        this.enrolledCourses = new HashSet<>();
    }

    public String getStudentId() {
        return studentId;
    }

    public String getStudentName() {
        return studentName;
    }

    public Set<String> getEnrolledCourses() {
        return enrolledCourses;
    }

    public boolean enrollCourse(Course course) {
        if (enrolledCourses.size() >= 5) {
            System.out.println("Maximum course limit reached!");
            return false;
        } else if (enrolledCourses.contains(course.getCourseId())) {
            System.out.println("Already enrolled in this course!");
            return false;
        } else {
            enrolledCourses.add(course.getCourseId());
            return course.enrollStudent(studentId);
        }
    }

    public boolean dropCourse(Course course) {
        if (enrolledCourses.remove(course.getCourseId())) {
            return course.dropStudent(studentId);
        } else {
            System.out.println("Not enrolled in this course!");
            return false;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Course javaCourse = new Course("CS101", "Java Programming", 20);
        Course pythonCourse = new Course("CS102", "Python Programming", 15);

        Student student1 = new Student("S001", "Alice");
        Student student2 = new Student("S002", "Bob");

        student1.enrollCourse(javaCourse);
        student1.enrollCourse(pythonCourse);
        student2.enrollCourse(javaCourse);

        System.out.println("Courses enrolled by Alice: " + student1.getEnrolledCourses());
        System.out.println("Courses enrolled by Bob: " + student2.getEnrolledCourses());

        student1.dropCourse(javaCourse);

        System.out.println("After dropping Java course, courses enrolled by Alice: " + student1.getEnrolledCourses());
        System.out.println("Current capacity of Java course: " + javaCourse.getCurrentCapacity());
    }
}

