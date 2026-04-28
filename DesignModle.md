设计模式实际上是一种经过历史时间检验的多种设计样板

为的是多人协作的时候能看懂别人代码和让别人看懂你的代码

# 创建型

## 工厂方法模式Factory

将复杂的创建流程封装在一个创建类里面(Factory)

类似于new一个新的类一样,但是屏蔽了对象的相关创建细节,让使用者可以更加关心别的事情

```jade
public class FruitFactory {
    /**
     * 这里就直接来一个静态方法根据指定类型进行创建
     * @param type 水果类型
     * @return 对应的水果对象
     */
    public static Fruit getFruit(String type) {
        switch (type) {
            case "苹果":
                return new Apple();
           	case "橘子":
                return new Orange();
            default:
                return null;
        }
    }
}
```



## 建造者模式Builder

假如某个类需要较多参数数据时

通过在类外面套一层(类似多元缓存)构建的过程依次填入参数,最后再完整的调用build()方法创建对象

与其建造者我觉得叫乐高模式更贴切,主要是为了让别人能清晰的配置参数把

```java
public class Student {
    int id;
    int age;
    int grade;
    String name;
    String college;
    String profession;
    List<String> awards;

    public Student(int id, int age, int grade, String name, String college, String profession, List<String> awards) {
        this.id = id;
        this.age = age;
        this.grade = grade;
        this.name = name;
        this.college = college;
        this.profession = profession;
        this.awards = awards;
    }
}
```



## 单例模式INSTANCE

假如流程中只会同时有一个对象操作,为什么不用同一个复用?

又有分懒汉和饿汉式(初始化时会不会先创建对象)

已经一些涉及到的并发问题,尝试加sync锁或静态内部类的内部再进行静态初始化

```java
public class Singleton {
    private static Singleton INSTANCE;   //在一开始先不进行对象创建

    private Singleton() {}   //不用多说了吧

    public static Singleton getInstance(){   //将对象的创建延后到需要时再进行
        if(INSTANCE == null) {    //如果实例为空，那么就进行创建，不为空说明已经创建过了，那么就直接返回
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
// 如果在多线程并发情况下,还要考虑同步问题
public static Singleton getInstance(){
    if(INSTANCE == null) {
        synchronized (Singleton.class) {    //实际上只需要对赋值这一步进行加锁即可
            INSTANCE = new Singleton();   
        }
    }
    return INSTANCE;
}
```



## 原型模式

像预制了某种默认的参数一样

在java里通过Cloneable接口实现浅拷贝

当然深拷贝也可以自己实现

```java
public class Student implements Cloneable{
    
    String name;

    public Student(String name){
        this.name = name;
    }
    
    public String getName() {
        return name;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
//它默认是浅拷贝,意思是内部的属性对象是不会拷贝的,仅拷贝本类型对象
//如果要改为深拷贝的话
@Override
public Object clone() throws CloneNotSupportedException {   //这里我们改进一下，针对成员变量也进行拷贝
    Student student = (Student) super.clone();
    student.name = new String(name);
    return student;   //成员拷贝完成后，再返回
}
```



# 结构型

## 适配器模式

## 桥接模式

## 组合模式

## 装饰模式

## 代理模式

## 外观模式

## 享元模式

# 行为型
