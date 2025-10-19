# Debug 2025 Freshman - C Programing

## Chapter03 - Array, String and Function

### struct

在上一节中，我们学习了 **数组** 和 **字符串**，它们让我们能够方便地存储和处理一组**相同类型**的数据。
 但是在生活中，我们的数据往往并不都是同一种类型。例如：

| 姓名 | 年龄 | 成绩 |
| ---- | ---- | ---- |
| 张三 | 18   | 95.5 |
| 李四 | 19   | 88.0 |
| 王五 | 17   | 90.0 |

这里的“姓名”是字符串，“年龄”是整数，“成绩”是小数。
这三种数据属于**同一个人**，显然应该被看作一个整体。

如果只用数组，我们可能会写出这样的代码：

```c++
char name[3][20] = {"ZhangSan", "LiSi", "WangWu"};
int age[3] = {18, 19, 17};
float score[3] = {95.5, 88.0, 90.0};
```

虽然能表示这些信息，但我们必须维护多个数组来表示同一类对象的不同属性，不仅麻烦，也容易出错。

此时，我们就需要一种能把“不同类型但属于同一个整体的数据”组织起来的结构：这就是 **结构体（struct）**。

**结构体（Structure）** 是一种用户自定义的数据类型，它可以将多个不同类型的变量组合成一个整体。
简单理解，它就像一个“容器”，把不同属性的数据“打包”在一起。

在 C 语言中，结构体的定义语法如下：

```c++
struct Student {
    char name[20];
    int age;
    float score;
};
```

这段代码定义了一个名为 `Student` 的结构体类型，包含三个成员：

- `name` —— 字符串，表示姓名；
- `age` —— 整数，表示年龄；
- `score` —— 浮点数，表示成绩。

结构体定义好后，我们就可以用它来声明变量：

```c++
struct Student s1;
```

此时，`s1` 就是一个结构体变量，包含 `name`、`age` 和 `score` 三个成员。
我们可以通过 **点号 `.`** 来访问结构体的每个成员：

```c++
strcpy(s1.name, "ZhangSan");
s1.age = 18;
s1.score = 95.5;

printf("Name: %s, Age: %d, Score: %.1f\n", s1.name, s1.age, s1.score);
```

输出结果为：

```bash
Name: ZhangSan, Age: 18, Score: 95.5
```

像数组一样，结构体也可以在定义时直接初始化：

```c++
struct Student s2 = {"LiSi", 19, 88.0};
```

也可以按成员名初始化：

```c++
struct Student s3 = {.name = "WangWu", .age = 17, .score = 90.0};
```

如果我们要保存多名学生的信息，可以定义一个**结构体数组**：

```c++
struct Student students[3] = {
    {"ZhangSan", 18, 95.5},
    {"LiSi", 19, 88.0},
    {"WangWu", 17, 90.0}
};
```

就像数组一样，我们可以用循环来遍历：

```c++
for (int i = 0; i < 3; i++) {
    printf("%s %d %.1f\n", students[i].name, students[i].age, students[i].score);
}
```

结构体不仅可以在主函数中使用，也可以作为**函数参数**或**返回值**。

1. 结构体作为参数传递

    ```c++
    void printStudent(struct Student s) {
        printf("Name: %s, Age: %d, Score: %.1f\n", s.name, s.age, s.score);
    }
    ```

    调用时：

    ```c++
    struct Student s1 = {"ZhangSan", 18, 95.5};
    printStudent(s1);
    ```

2. 结构体作为返回值

    ```c++
    struct Student createStudent(int age, float score) {
        struct Student s;
        s.age = age;
        s.score = score;
        return s;
    }
    ```
    
    使用：
	
    ```c++
    struct Student s2 = createStudent("LiSi", 19, 88.0);
    printStudent(s2);
    ```

每次都写 `struct Student` 比较麻烦，我们可以用 `typedef` 给它取个“别名”：

```c++
typedef struct {
    char name[20];
    int age;
    float score;
} Student;
```

这样我们以后就可以直接写：

```c++
Student s = {"LiSi", 19, 88.0};
```

而不需要再写 `struct Student`。

结构体的成员不仅可以是基本类型，也可以是另一个结构体：

```c++
struct Date {
    int year, month, day;
};

struct Student {
    char name[20];
    int age;
    float score;
    struct Date birthday;
};
```

使用时：

```c++
struct Student s = {"ZhangSan", 18, 95.5, {2006, 5, 20}};
printf("%s was born in %d-%d-%d\n", s.name, s.birthday.year, s.birthday.month, s.birthday.day);
```

定义结构体并使用结构体编程的时候，实际上我们正在将问题拆解一个个不同的“对象”，将操作基本数据的问题转换为操作对象的问题，从而减少代码的复杂度。这种将大问题简化为一个个对象，再进行处理的思维，我们叫做**抽象（abstract）**也就是**抽离对象**

