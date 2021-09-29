# for-each循环应用于String中每个字符

for循环遍历String，使用charAt( i )，index从 0 ~ length-1。

```java
String a = "abcdefg";
for (int i=0 ; i<a.length() ; i++){
    System.out.println(a.charAt(i));
}
```

for-each循环遍历String，使用toCharArray( )方法，转换为char数组，实际上增加了占用空间。

```java
String a = "abcdefg";
for (char k:a.toCharArray()) {
    System.out.println(k);
}
```

