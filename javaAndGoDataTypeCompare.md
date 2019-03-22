### java 和go 的基本数据类型的比较



java是我的老本行语言，所以对比着java来学习Go语言，先从最基本的数据类型来入手。

| java数据类型 | java数据类型备注                      | Go数据类型 | Go数据类型备注                        |
| ------------ | ------------------------------------- | ---------- | ------------------------------------- |
| byte         | 8位 有符号 整型 [-128   127]          | int8       | 8位 有符号 整型 [-128   127]          |
|              |                                       | uint8      | 8位 无符号 整型 [0   255]             |
| short        | 16位 有符号 整型 [-32768  32767]      | int16      | 16位 有符号 整型 [-32768  32767]      |
|              |                                       | unit16     | 16位 无符号 整型 [0  65535]           |
| int          | 32位 有符号 [(-2^31)      (2^31) - 1] | int32      | 32位 有符号 [(-2^31)      (2^31) - 1] |
|              |                                       | unit32     | 32位 无符号 [      (2^33) - 1]        |
| long         | 64位 有符号 [(-2^63)    (2^63) - 1]   | int64      | 64位 有符号 [(-2^63)    (2^63) - 1]   |
|              |                                       | unit64     | 64位 无符号 [0    (2^64) - 1]         |
| float        | 32位  单精度浮点数                    | float32    | 32位                                  |
| double       | 64位  双精度浮点数                    | float64    | 64位                                  |
| boolean      |                                       | bool       |                                       |
|              |                                       | int        | 有符号     所用位数等于cpu位数        |
|              |                                       | unit       | 无符号    所用位数等于cpu位数         |
| char         |                                       | rune       | 有符号 Unicode 字符类型 等价于int32   |
| char         |                                       | byte       | 无符号   等价于unit8                  |
|              |                                       | unitptr    | 无符号整型，用于存放一个指针          |
| String       | 不可变类                              | string     | 不可变                                |
| char         | 单一的 16 位 Unicode 字符             |            |                                       |
|              |                                       | complex64  |                                       |
|              |                                       | complex128 |                                       |

由此可见Go语言的数据类型还是比较丰富的，并且还有指针这个java没有的数据类型。

#### 引用数据类型

| 类型 | java                                      | go                                                 | 备注                 |
| ---- | ----------------------------------------- | -------------------------------------------------- | -------------------- |
| 数组 | String[] s = {"str","str"}                | var s = []string{"str","str"}                      |                      |
| list | List<String> list = new ArrayList<>();    | var list = list.New()                              | go的list可以左插右插 |
| map  | Map<String,Object> map = new HashMap<>(); | var map_variable map[key_data_type]value_data_type | go的map需要make一下  |
| set  | HashSet、TreeSet                          |                                                    |                      |
|      |                                           |                                                    |                      |

