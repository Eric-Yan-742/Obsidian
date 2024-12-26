```HTML
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="java.util.*" %>
<html>
<head>
    <title>Haha</title>
</head>
<body>
<form action="todo-demo.jsp">
    Add new item: <input type="text" name="item"/>
    <input type="submit" value="Submit"/>
</form>
<br>
Item entered: <%= request.getParameter("item")%>
<!-- Add new items to the list -->
<%
    List<String> items = (List<String>) session.getAttribute("myTodoList");
    if(items == null) {
        items = new ArrayList<>();
        session.setAttribute("myTodoList", items);
    }
    String theItem = request.getParameter("item");
    if(theItem != null) {
        items.add(theItem);
    }
%>
<!-- Display all "To Do" item from session -->
<hr>
<b>Todo List Items: </b><br>

<ol>
    <%
        for(String temp : items) {
            out.println("<li>" + temp + "</li>");
        }
    %>
</ol>
</body>
</html>
```