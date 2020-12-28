# ClassFile

```
ClassFile {
    u4             magic;
    u2             minor_version;
    u2             major_version;
    u2             constant_pool_count;
    cp_info        constant_pool[constant_pool_count-1];
    u2             access_flags;
    u2             this_class;
    u2             super_class;
    u2             interfaces_count;
    u2             interfaces[interfaces_count];
    u2             fields_count;
    field_info     fields[fields_count];
    u2             methods_count;
    method_info    methods[methods_count];
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}
```

### 类型表示

- BaseType: B C D F I J S Z
- ObjectType: L ClassName ;
- ArrayType: [ ComponentType
- ComponentType: BaseType | ObjectType | ArrayType

### Field Descriptor

用于描述字段的类型，区别于 **access flags**。

`int[] values` --> `[I`

`Object obj` --> `Ljava/lang/Object;`

### Method Descriptor

用于描述方法的类型，包含参数列表和返回值。

`Object m(int i, double d, Thread t){...}` --> `(IDLjava/lang/Thread;)Ljava/lang/Object;`

`int[] compute(double d, List<Boolean> l){...}` --> `(D)[I` #TODO 范型如何表示？

### The Constant Pool

**Kinds**

- CONSTANT_Integer
- CONSTANT_Float
- CONSTANT_Long
- CONSTANT_Double
- CONSTANT_String
- CONSTANT_Utf8

- CONSTANT_Class
- CONSTANT_Fieldref
- CONSTANT_Methodref
- CONSTANT_InterfaceMethodref
- CONSTANT_NameAndType
- CONSTANT_MethodHandle
- CONSTANT_MethodType
- CONSTANT_Dynamic
- CONSTANT_InvokeDynamic
- CONSTANT_Module
- CONSTANT_Package

> **TODO:** What's the difference between `CONSTANT_String` and `CONSTANT_Utf8` ?

```
CONSTANT_Class_info {
    u1 tag;
    u2 name_index;
}
```

```bash
# field of a class of an interface
CONSTANT_Fieldref_info {
    u1 tag;
    u2 class_index;
    u2 name_and_type_index;
}
# method of a class(not an interface)
CONSTANT_Methodref_info {
    u1 tag;
    u2 class_index;
    u2 name_and_type_index;
}
# method of an interface(not a class)
CONSTANT_InterfaceMethodref_info {
    u1 tag;
    u2 class_index;
    u2 name_and_type_index;
}
```

```bash
# maybe a field or a method
# - field name & field descriptor
# - method name & method descriptor
CONSTANT_NameAndType_info {
    u1 tag;
    u2 name_index;
    u2 descriptor_index;
}
```

```
CONSTANT_MethodHandle_info {
    u1 tag;
    u1 reference_kind;
    u2 reference_index;
}
```

[The CONSTANT_MethodHandle_info Structure](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-4.html#jvms-4.4.8)

