# JDBC Programs

For JDBC Programs I used [XAMPP](https://www.apachefriends.org/index.html)'s MySQL. [MySQL's jar](https://dev.mysql.com/downloads/connector/j) file.

> There is a database called `mydb` has table (with data)

- [Table's Data In Reverse Order](#tables-data-in-reverse-order)
- [CRUD Operation](#crud-operation)
- [Stored Procedure Call](#stored-procedure-call)

## Table's Data In Reverse Order

Write a program which prints the student_name and id_no from the database in reverse order. And also insert the two new row

```Java
public class Main {
    public static void main(String[] args) throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "root", "");
        Statement statement = con.createStatement();
        ResultSet result = statement.executeQuery("SELECT * FROM students ORDER BY id_no DESC");
        System.out.println("Table content in reverse order");
        while (result.next()) {
            System.out.println(result.getInt("id_no") + "\t" + result.getString("student_name"));
        }
        con.close();
    }
}
```

## CRUD Operation

Develop an event driven program that perform the following SQL operations: (i) Select (ii) Insert (iii) Update (iv) Delete

```Java
import java.sql.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        boolean exit = false;
        Scanner sc = new Scanner(System.in);

        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "root", "");
        Statement statement = con.createStatement();

        while (!exit) {
            System.out.println("(1) Show records\n(2) Insert a new record");
            System.out.println("(3) Update a record\n(4) Delete a record\n(5) Exit");
            System.out.print("Enter your choice: ");
            int choice = Integer.parseInt(sc.nextLine());
            System.out.println();
            int id = 0;
            String name = null;
            switch (choice) {
                case 1:
                    System.out.println("Table contents");
                    ResultSet result = statement.executeQuery("SELECT * FROM students");
                    while (result.next()) {
                        System.out.println(result.getInt("id_no") + "\t" + result.getString("student_name"));
                    }
                    break;
                case 2:
                    System.out.print("Enter student id: ");
                    id = Integer.parseInt(sc.nextLine());
                    System.out.print("Enter student name: ");
                    name = sc.nextLine();
                    statement.execute("INSERT INTO students(id_no, student_name) VALUES(" + id + ", '" + name + "')");
                    System.out.printf("\nStudent %s added\n", name);
                    break;
                case 3:
                    System.out.print("Enter student id: ");
                    id = Integer.parseInt(sc.nextLine());
                    System.out.print("Enter new name value: ");
                    name = sc.nextLine();
                    statement.execute(String.format("UPDATE students SET student_name='%s' WHERE id_no=%d", name, id));
                    System.out.println("\nRecord updated");
                    break;
                case 4:
                    System.out.print("Enter student id: ");
                    id = Integer.parseInt(sc.nextLine());
                    statement.execute(String.format("DELETE FROM students WHERE id_no=%d", id));
                    System.out.println("\nRecord deleted");
                    break;
                case 5:
                    exit = true;
                    break;
                default:
                    System.out.println("Invalid option!");
                    break;
            }
            System.out.println();
        }
        con.close();
        sc.close();
    }
}
```

## Stored Procedure Call

> There is a stored procedure called `getStudentCount` which returns counts of students in student table

```Java
public class Main {
    public static void main(String[] args) throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "root", "");
        CallableStatement call = con.prepareCall("{CALL getStudentCount(?)}");
        call.registerOutParameter(1, Types.INTEGER);
        call.execute();
        System.out.println("Total students: " + call.getInt(1));
    }
}
```

