### csv用excel打开中文乱码问题

csv是一个文本形式的文件，用Excel打开需要在头部添加一个BOM

```java
byte[] uft8bom = {(byte)0xef,(byte)0xbb,(byte)0xbf};
```
在输出csv之前先输出这个BOM就可以防止乱码的出先。