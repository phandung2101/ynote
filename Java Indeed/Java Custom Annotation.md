## Custom Annotation Spring

- Sử dụng `@interface` để định nghĩa 1 annotation

```jsx
public @interface JsonSerializable {
}

```

- Sử dụng `RotentionPolicy` dùng để chú thích mức độ tồn tại của 1 annotation nào đó. Có 3 cách để nhận thức:

```java
public enum RetentionPolicy {
    // chỉ tồn tại trên mã nguồn và không được trình biên dịch nhận ra
    SOURCE,

    // Mức tồn tại được trình biên dịch nhận ra, nhưng không được nhận biết bởi
		// máy ảo tại thời điểm chạy (Runtime).
    CLASS,

    // Mức tồn tại lớn nhất, được trình biên dịch nhận biết,
		// và máy ảo (JVM) cũng nhận ra khi chạy chương trình.
    RUNTIME
}

```

- Sử dụng @Target Dùng để chú thích **phạm vi sử dụng** của một Annotation:

```java
public enum ElementType {
    /** Chú thích trên Class, interface, enum, annotation */
    TYPE,

    /** Chú thích trường (field), bao gồm cả các hằng số enum. */
    FIELD,

    /** Chú thích trên method. */
    METHOD,

    /** Chú thích trên parameter. */
    PARAMETER,

    /** Chú thích trên constructor. */
    CONSTRUCTOR,

    /** Chú thích trên biến địa phương. */
    LOCAL_VARIABLE,

    /** Chú thích trên Annotation khác. */
    ANNOTATION_TYPE,

    /** Chú thích trên package. */
    PACKAGE,

    /**
     * Type parameter declaration
     *
     * @since 1.8
     */
    TYPE_PARAMETER,

    /**
     * Use of a type
     *
     * @since 1.8
     */
    TYPE_USE
}

```