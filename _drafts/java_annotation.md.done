
1. 创建 `Annotation` 接口：

        public @interface AnnotataionName {}

2. 注意事项：

    创建接口之后，就可以在别的类上使用注解，但是该注解在程序运行时会被自动丢弃。
    如果不希望被自动丢弃，则需要在注解接口上添加 `Retention(RetentionPolicy.RUNTIME` 注解。
        
        @Retention(RetentionPolicy.RUNTIME)
        public @interface AnnotationName {}
        lasdk
