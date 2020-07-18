## JVM运行时数据区



> 众所周知`java`引以为豪的就是其`跨平台性`和`内存管理`，要了解 这个特性就需要了解`java`程序运行时的内存区域划分   

正确理解`jvm`运行时数据区是理解`jvm`底层设计的关键步骤，下图可以看出`jvm内存结构`处于`jvm`知识体系的`C`位。

![C位](../img/jvm/c_location.png)

#### 正文

Java 虚拟机在执行java程序过程中会把`它管理`的内存划分为若干个数据区域，这些数据区域有各自的用途，以及创建和销毁时间。根据虚拟机规范，Java虚拟机所管理的内存会有一下几个数据区域  

官方文档如是说：

##### `The pc Register(program counter register)`

```
The Java Virtual Machine can support many threads of execution at once (JLS §17). Each Java Virtual Machine thread has its own pc (program counter) register. At any point, each Java Virtual Machine thread is executing the code of a single method, namely the current method (§2.6) for that thread. If that method is not native, the pc register contains the address of the Java Virtual Machine instruction currently being executed. If the method currently being executed by the thread is native, the value of the Java Virtual Machine's pc register is undefined. The Java Virtual Machine's pc register is wide enough to hold a returnAddress or a native pointer on the specific platform.
```

释：pc 计数器，Java虚拟机支持同时执行多个线程，每个线程有自己的p c计数器，在任何时候每个虚拟机线程只能正在执行一个方法的代码，如果线程执行的不是`nativev method`，**pc 计数器存放的是正在执行的虚拟机指令的地址**，如果线程执行的是`native method`那么pc计数器就是`undifine`

##### `Java Virtual Machine Stacks`

```
Each Java Virtual Machine thread has a private Java Virtual Machine stack, created at the same time as the thread. A Java Virtual Machine stack stores frames (§2.6). A Java Virtual Machine stack is analogous to the stack of a conventional language such as C: it holds local variables and partial results, and plays a part in method invocation and return. Because the Java Virtual Machine stack is never manipulated directly except to push and pop frames, frames may be heap allocated. The memory for a Java Virtual Machine stack does not need to be contiguous.

In the First Edition of The Java® Virtual Machine Specification, the Java Virtual Machine stack was known as the Java stack.

This specification permits Java Virtual Machine stacks either to be of a fixed size or to dynamically expand and contract as required by the computation. If the Java Virtual Machine stacks are of a fixed size, the size of each Java Virtual Machine stack may be chosen independently when that stack is created.

A Java Virtual Machine implementation may provide the programmer or the user control over the initial size of Java Virtual Machine stacks, as well as, in the case of dynamically expanding or contracting Java Virtual Machine stacks, control over the maximum and minimum sizes.

The following exceptional conditions are associated with Java Virtual Machine stacks:

If the computation in a thread requires a larger Java Virtual Machine stack than is permitted, the Java Virtual Machine throws a StackOverflowError.

If Java Virtual Machine stacks can be dynamically expanded, and expansion is attempted but insufficient memory can be made available to effect the expansion, or if insufficient memory can be made available to create the initial Java Virtual Machine stack for a new thread, the Java Virtual Machine throws an OutOfMemoryError.
```

释：虚拟机栈，每个虚拟机线程一创建就会创建一个私有的虚拟机栈，虚拟机栈保存`frame（栈帧）`，虚拟机栈除了压入帧和弹出帧不会被直接操作，帧可以被分配在堆上，虚拟机栈不一定是连续的。栈的大小可以被设置最大值和最小值，如果线程需要的虚拟机栈大于允许的值虚拟机将抛出`StackOverflowError`，如果可以动态扩展虚拟机栈，并且尝试扩展，但无法提供足够的内存来实现扩展，或者如果内存不足，无法为新线程创建初始Java虚拟机堆栈，则Java虚拟机将抛出`OutOfMemoryError`。

#### `Native Method Stacks`

```
An implementation of the Java Virtual Machine may use conventional stacks, colloquially called "C stacks," to support native methods (methods written in a language other than the Java programming language). Native method stacks may also be used by the implementation of an interpreter for the Java Virtual Machine's instruction set in a language such as C. Java Virtual Machine implementations that cannot load native methods and that do not themselves rely on conventional stacks need not supply native method stacks. If supplied, native method stacks are typically allocated per thread when each thread is created.

This specification permits native method stacks either to be of a fixed size or to dynamically expand and contract as required by the computation. If the native method stacks are of a fixed size, the size of each native method stack may be chosen independently when that stack is created.

A Java Virtual Machine implementation may provide the programmer or the user control over the initial size of the native method stacks, as well as, in the case of varying-size native method stacks, control over the maximum and minimum method stack sizes.

The following exceptional conditions are associated with native method stacks:

If the computation in a thread requires a larger native method stack than is permitted, the Java Virtual Machine throws a StackOverflowError.

If native method stacks can be dynamically expanded and native method stack expansion is attempted but insufficient memory can be made available, or if insufficient memory can be made available to create the initial native method stack for a new thread, the Java Virtual Machine throws an OutOfMemoryError.
```

释：本地方法栈，调用非Java 语言实现的方法时候使用，同样是线程私有，会抛出`StackOverflowError`,`OutOfMemoryError`

#### `Heap`

```
The Java Virtual Machine has a heap that is shared among all Java Virtual Machine threads. The heap is the run-time data area from which memory for all class instances and arrays is allocated.

The heap is created on virtual machine start-up. Heap storage for objects is reclaimed by an automatic storage management system (known as a garbage collector); objects are never explicitly deallocated. The Java Virtual Machine assumes no particular type of automatic storage management system, and the storage management technique may be chosen according to the implementor's system requirements. The heap may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger heap becomes unnecessary. The memory for the heap does not need to be contiguous.

A Java Virtual Machine implementation may provide the programmer or the user control over the initial size of the heap, as well as, if the heap can be dynamically expanded or contracted, control over the maximum and minimum heap size.

The following exceptional condition is associated with the heap:

If a computation requires more heap than can be made available by the automatic storage management system, the Java Virtual Machine throws an OutOfMemoryError.
```

释：堆，虚拟机堆是线程共享的，堆是运行时数据区，在这里所有的类实例和数组将被分配内存。堆是在虚拟机启动时候创建，堆存储的对象被由自动存储管理系统（称作 垃圾回收器）回收，对象永远不会显式回收，堆的大小可以固定也可以是可扩展的。堆内存不需要是连续的。虚拟机实现者需要让编程人员控制堆的初始大小，或者最大和最小。

#### `Method Area`

```
The Java Virtual Machine has a method area that is shared among all Java Virtual Machine threads. The method area is analogous to the storage area for compiled code of a conventional language or analogous to the "text" segment in an operating system process. It stores per-class structures such as the run-time constant pool, field and method data, and the code for methods and constructors, including the special methods used in class and interface initialization and in instance initialization (§2.9).

The method area is created on virtual machine start-up. Although the method area is logically part of the heap, simple implementations may choose not to either garbage collect or compact it. This specification does not mandate the location of the method area or the policies used to manage compiled code. The method area may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger method area becomes unnecessary. The memory for the method area does not need to be contiguous.

A Java Virtual Machine implementation may provide the programmer or the user control over the initial size of the method area, as well as, in the case of a varying-size method area, control over the maximum and minimum method area size.

The following exceptional condition is associated with the method area:

If memory in the method area cannot be made available to satisfy an allocation request, the Java Virtual Machine throws an OutOfMemoryError.
```

释：方法区，方法区线程之间共享，方法区类似于常规语言编译代码的存储区，或类似于操作系统进程中的“文本”段。它存储每个类的结构，如运行时常量池、字段和方法数据，以及方法和构造函数的代码，包括类和接口初始化以及实例初始化中使用的特殊方法，方法区域是在虚拟机启动时创建的。方法区在逻辑上是堆的一部分

##### `Run-Time Constant Pool`

```
A run-time constant pool is a per-class or per-interface run-time representation of the constant_pool table in a class file (§4.4). It contains several kinds of constants, ranging from numeric literals known at compile-time to method and field references that must be resolved at run-time. The run-time constant pool serves a function similar to that of a symbol table for a conventional programming language, although it contains a wider range of data than a typical symbol table.

Each run-time constant pool is allocated from the Java Virtual Machine's method area (§2.5.4). The run-time constant pool for a class or interface is constructed when the class or interface is created (§5.3) by the Java Virtual Machine.

The following exceptional condition is associated with the construction of the run-time constant pool for a class or interface:

When creating a class or interface, if the construction of the run-time constant pool requires more memory than can be made available in the method area of the Java Virtual Machine, the Java Virtual Machine throws an OutOfMemoryError.

See §5 (Loading, Linking, and Initializing) for information about the construction of the run-time constant pool.
```

释：运行时常量池，运行时常量池是类文件（§4.4）中常量池表的每个类或每个接口的运行时表示。它包含几种常量，从编译时已知的数字字面值到必须在运行时解析的方法和字段引用。运行时常量池的功能类似于传统编程语言的符号表，尽管它包含的数据范围比典型的符号表更广。每个运行时常量池都是在虚拟机方法区分配的，类或接口的运行时常量池是在Java虚拟机创建类或接口（§5.3）时构造的



#### 先总结一波

按照线程私有和线程共享划分

**线程私有：** 虚拟机栈、本地方法栈、pc计数器。

**线程共享：** 堆、方法区、运行时常量池。

直接内存，jvm并不直接管理直接内存但是可以使用直接内存，

![C位](./img/jvm/c_location.png)











引用：

[官方文档地址](https://docs.oracle.com/javase/specs/index.html)

[jvm参数官方文档](https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html)



