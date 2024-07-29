
参考:
* Effective Java
* Java 编程的逻辑
* [Good to read] Generic's Wildcards, https://jenkov.com/tutorials/java-generics/wildcards.html

## Item26: Don't use raw types
* `List` is raw type to `List<E>`.
* raw types是为了兼容泛型出现之前的代码, 不应该使用raw type
* `List<String>` 是`List`的子类型, 而不是`List<object>`的子类型

#### `List<Object>, List<?>, List`的区别
* `List<?>`是通配符类型, 可以匹配`List<String>/List<Integer>等等`

```java
private void func(List<Object> ls, Object o) {
    ls.add(o);
}

@Test
public void testGenerics() {
    // Don't use raw types
    List ls = new ArrayList<String>();
    ls = new ArrayList<Integer>();
    ls = new ArrayList<String>();

    List<Object> ls2 = new ArrayList<Object>();
    func(ls2, "hello");
    // incompatible types: java.util.ArrayList<java.lang.Integer> cannot be converted to java.util.List<java.lang.Object>
    //ls2 = new ArrayList<Integer>();
    // incompatible types: java.util.ArrayList<java.lang.String> cannot be converted to java.util.List<java.lang.Object>
    //ls2 = new ArrayList<String>();

    List<String> ls3 = new ArrayList<String>();
    // incompatible types: java.util.List<java.lang.String> cannot be converted to java.util.List<java.lang.Object>
    // func(ls3);
}

private void unsafeAdd(List ls, Object o) {
    ls.add(o);
}

//可以写成: 
//private <T> void func2(List<T> ls)
private void func2(List<?> ls) {

}

@Test
public void testGenerics2() {
    List<String> ls = new ArrayList<String>();
    unsafeAdd(ls, "Hello");
    unsafeAdd(ls, 10);
    assertEquals(2, ls.size());

    //runtime exception: java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String
    // String s = ls.get(1);

    // incompatible types: java.util.List<java.lang.String> cannot be converted to java.util.List<java.lang.Object>
//         add(ls, "world");

    func2(ls);
    List<Integer> lsi = new ArrayList<>();
    func2(lsi);
}
```