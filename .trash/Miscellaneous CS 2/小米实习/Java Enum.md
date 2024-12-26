[java枚举enum和Enum类的使用_java_脚本之家 (jb51.net)](https://www.jb51.net/article/276866.htm)

例子

```Java
@Getter
@AllArgsConstructor
public enum AccountingStatusEnum {
    PENDING(0, "待入账"),
    DETAIL_PREPARING(1, "入账明细准备中"),
    DETAIL_WRITE_COMPLETED(2, "入账明细写入完成"),
    DETAIL_WRITE_FAILED(3, "入账明细写入失败"),
    IN_PROGRESS(4, "入账中"),
    COMPLETED(5, "已入账"),
    FAILED(6, "入账失败");

    private final int code;
    private final String description;

    public static AccountingStatusEnum fromCode(int code) {
        for (AccountingStatusEnum status : AccountingStatusEnum.values()) {
            if (status.getCode() == code) {
                return status;
            }
        }
        throw new IllegalArgumentException("Unknown accounting status code: " + code);
    }
}
```

- 将第一行的所有常量看成 public static final AccountingStatusEnum 对象。后面括号内的是声明调用的是哪个构造器，这里也就是AllArgsConstructor自动生成的 
    
    ```Java
    public AccountingStatusEnum(int code, String description) {
    		this.code = code;
    		this.description = description;
    }
    ```
    
- AccountingStatusEnum.values() 返回的是所有常量对象的数组，顺序和对象的声明顺序一致。