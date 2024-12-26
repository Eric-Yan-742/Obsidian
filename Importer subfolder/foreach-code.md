- Student.java
    
    ```Java
    package com.luv2code.jsp.tagdemo;
    
    public class Student {
        private String firstName;
        private String lastName;
        private boolean goldCustomer;
    
        public Student(String firstName, String lastName, boolean goldCustomer) {
            this.firstName = firstName;
            this.lastName = lastName;
            this.goldCustomer = goldCustomer;
        }
    
        public String getFirstName() {
            return firstName;
        }
    
        public String getLastName() {
            return lastName;
        }
    
        public void setLastName(String lastName) {
            this.lastName = lastName;
        }
    
        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }
    
        public boolean isGoldCustomer() {
            return goldCustomer;
        }
    
        public void setGoldCustomer(boolean goldCustomer) {
            this.goldCustomer = goldCustomer;
        }
    }
    ```
    
- foreach-student-test.jsp
    
    ```HTML
    <%--
      Created by IntelliJ IDEA.
      User: yyq20
      Date: 2023/9/6
      Time: 10:58
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
    <%@ page import="java.util.*, com.luv2code.jsp.tagdemo.Student" %>
    <%
        //sample data
        List<Student> data = new ArrayList<>();
    
        data.add(new Student("John", "Doe", false));
        data.add(new Student("Maxwell", "Johnson", false));
        data.add(new Student("Mary", "Public", false));
    
        pageContext.setAttribute("myStudents", data);
    %>
    <html>
    <body>
    <table border="1">
        <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Gold Customer</th>
        </tr>
        <c:forEach var="tempStudent" items="${myStudents}">
            <tr>
            <!-- JSP Expression Language, actually call the getter method in the background -->
                <td>${tempStudent.firstName}</td>
                <td>${tempStudent.lastName}</td>
                <td>${tempStudent.goldCustomer}</td>
            </tr>
        </c:forEach>
    </table>
    </body>
    </html>
    ```