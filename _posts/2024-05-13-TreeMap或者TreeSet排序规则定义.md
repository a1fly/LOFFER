---
layout: post
title: TreeMap或者TreeSet排序规则定义
date: 2024-05-13 
Author: a1fly
tags: [sample, document]
comments: true
---


## 如果是自定义的类（如Student）

实现`Comparable<T>`接口，并且如果要比较的对象是Student，泛型即为`Student`



```java
package TreeMap_Test1;

public class Student implements Comparable<Student>{
    private String name;
    private int age;


    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return age
     */
    public int getAge() {
        return age;
    }

    /**
     * 设置
     * @param age
     */
    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Student{name = " + name + ", age = " + age + "}";
    }

    @Override
    public int compareTo(Student o) {
        if (this.getAge()-o.getAge()!=0){
            return this.getAge()-o.getAge();
        }
        else if (! this.getName().equals(o.getName())) {
            return this.getName().compareTo(o.getName());
        }
        else {
            return 0;
        }

    }

}

```



重写接口的`compareTo`方法

```java
@Override
public int compareTo(Student o) {
    if (this.getAge()-o.getAge()!=0){
        return this.getAge()-o.getAge();
    }
    else if (! this.getName().equals(o.getName())) {
        return this.getName().compareTo(o.getName());
    }
    else {
        return 0;
    }
}

// o表示已经存在树中的对象
//返回 > 0就说明大
// 0 表示不写入


```



>为什么自定义类的时候需要写这个比较规则？
>
>因为`Integer`，`String`，`Character`等类中已经重写过该方法了



## 如果不是自定义类

如`Integer`源码里面重写的`Comparable<T>`接口里的规则是从小到大排序的。



如果想要从大到小排序，就需要使用匿名内部类的方式了



```java
package TreeMap_Test1;

import java.util.Comparator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class StudentTest {
    public static void main(String[] args) {
        Student s1=new Student("Tom",21);
        Student s2=new Student("MiKi",22);
        Student s3=new Student("MiKi",22);
        Student s4=new Student("Amy",19);
        Student s5=new Student("Zohn",21);

        Map<Student,String> m=new TreeMap<>(new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                int i=o2.getAge()-o1.getAge()!=0?o2.getAge()-o1.getAge():o1.getName().compareTo(o2.getName());

                return i;
            }
        });


        m.put(s1,"浙江");
        m.put(s2,"宁夏");
        m.put(s3,"广州");
        m.put(s4,"北京");
        m.put(s5,"西藏");

        Set<Map.Entry<Student,String>> sm=m.entrySet();
        for (Map.Entry<Student, String> item : sm) {
            System.out.println(item.getKey()+"---"+item.getValue());
        }

    }
}

```



我们需要在创建TreeSet或者TreeMap的对象时就创建一个匿名内部类

```java
Map<Student,String> m=new TreeMap<>(new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
        
        int i=o2.getAge()-o1.getAge()!=0?o2.getAge()-o1.getAge():o1.getName().compareTo(o2.getName());
        
        return i;
    }
});



// 这里的o1.getName().compareTo(o2.getName());表示使用String原本自己的compareTo方法
```



去定义规则



## 如果两个方式都定义了规则

如果两个方式都定义了规则就听第二种规则

