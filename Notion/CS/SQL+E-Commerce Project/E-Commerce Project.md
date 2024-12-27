# 问题总结

- Servlet中 `PrintWriter` 无法输出中文。解决方法：在 `getWriter` 前将 `response` 的编码格式设为 utf-8。
    
    ```Java
    response.setContentType("text/plain");
    response.setCharacterEncoding("utf-8");
    PrintWriter out = response.getWriter();
    ```
    
- SQL中，嵌套语句也可以用在 `update` 中
    
    ```SQL
    update customer set balance = balance - (select price from items where id=?) where id=1;
    ```
    
- `c:url` 完全可以用 `form` 替代
- JDBC中， `PreparedStatement` 不可以执行多条不一样的SQL，但 `Statement` 可以
- CSS中， `nth-child` 指在HTML中的第n个child，而不是class中的第几个
    - <table>中的<tr>