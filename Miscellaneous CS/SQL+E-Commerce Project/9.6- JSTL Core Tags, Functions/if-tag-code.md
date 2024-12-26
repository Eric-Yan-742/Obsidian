- Student.java
    - Same as the last example
- foreach-student-test.jsp
    
    ```HTML
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
    <%@ page import="java.util.*, com.luv2code.jsp.tagdemo.Student" %>
    <%
        //sample data
        List<Student> data = new ArrayList<>();
    
        data.add(new Student("John", "Doe", false));
        data.add(new Student("Maxwell", "Johnson", false));
        data.add(new Student("Mary", "Public", true));
    
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
                <td>
    								<!-- Added Code!!!! -->
                    <c:if test="${tempStudent.goldCustomer}">
                        Special Discount
                    </c:if>
                    <c:if test="${not tempStudent.goldCustomer}">
                        -
                    </c:if>
                </td>
            </tr>
        </c:forEach>
    </table>
    </body>
    </html>
    ```