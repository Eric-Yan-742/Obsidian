> In JSTL, variables do not have explicit data types like in Java or other strongly typed languages. Instead, variables are treated as objects, and their data types are determined by the objects they reference.

```HTML
<c:set var="myString" value="Hello, World!" />
<c:set var="myNumber" value="42" />
<c:set var="myList" value="${someList}" />
<c:set var="myObject" value="${someJavaObject}" />
<c:set var="myNull" value="${null}" />
```

- `**myString**` can hold a string.
- `**myNumber**` can hold a number (as a string in this case, but you can perform numeric operations on it if needed).
- `**myList**` can hold a collection (e.g., a List or Set).
- `**myObject**` can hold any Java object, such as a custom Java class or a JavaBean.
- `**myNull**` can hold a null value.