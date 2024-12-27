- cookies-personalize-form.html
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Personalize the site</title>
    </head>
    <body>
    <form action="cookies-personalize-response.jsp">
        Select your Favorite Programming Language
        <select name="favoriteLanguage">
            <option>Java</option>
            <option>C#</option>
            <option>PHP</option>
            <option>Ruby</option>
        </select>
        <br>
        <input type="submit" value="Submit">
    </form>
    </body>
    </html>
    ```
    
- cookies-personalize-response.jsp
    
    ```HTML
    <%--
      Created by IntelliJ IDEA.
      User: yyq20
      Date: 2023/9/5
      Time: 11:55
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Personalize Response</title>
    </head>
    <%
        String favLang = request.getParameter("favoriteLanguage");
    
        //create the cookies
        Cookie theCookie = new Cookie("myApp.favoriteLanguage", favLang);
    
        //set life span (1 year)
        theCookie.setMaxAge(60*60*24*365);
    
        //send cookie to browser
        response.addCookie(theCookie);
    %>
    <body>
    Thanks! We set your favorite language to ${param.favoriteLanguage}
    <br><br/>
    <a href="cookies-homepage.jsp">Return to homepage.</a>
    </body>
    </html>
    ```
    
- cookies-homepage.jsp
    
    ```HTML
    <%--
      Created by IntelliJ IDEA.
      User: yyq20
      Date: 2023/9/5
      Time: 12:26
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Home page</title>
    </head>
    <body>
    <!-- read the cookie -->
    <%
        //the default
        String favLang = "Java";
        //get the cookie
        Cookie[] theCookies = request.getCookies();
        //find our favorite language cookie
        if(theCookies != null) {
            for(Cookie tempCookie : theCookies) {
                if("myApp.favoriteLanguage".equals(tempCookie.getName())) {
                    favLang = tempCookie.getValue();
                    break;
                }
            }
        }
    %>
    
    <!-- show the page based on that information -->
    <h4>New books for <%= favLang %></h4>
    <ul>
        <li>blah blah</li>
    </ul>
    <h4>Latest News for <%= favLang %></h4>
    <ul>
        <li>blah blah</li>
    </ul>
    <h4>Hot Jobs for <%= favLang %></h4>
    <ul>
        <li>blah blah</li>
    </ul>
    <a href="cookies-personalize-form.html">Personalize this page</a>
    </body>
    </html>
    ```