# 1、JAVA一年经验实面记录
## 1.1、中科软
1、自我介绍
2、为何从上一家离职？
3、业务很复杂的场景，如何解决？
4、讲解项目？
流程化
测算、预测算、反查、统计
测算结果如何验证？
5、SpringMVC请求流程？
不建议使用觉得、感觉之类的词。可以用认为代替
6、Sprin控制事务的机制？
7、事务的隔离级别？
8、单例模式？
9、JAVA集合？流程化
ArrayList和LinkedList的区别？切忌好像
10、获取异常和抛出异常？
11、final和finaly区别？
12、前端请求后端有哪几种方式？
13、where和having区别？
14、unio和unioALL区别？
## 1.2、北京宏远兴邦
1、自我介绍
2、离职原因？
3、项目框架？
4、持久层框架？
mybatis、sql文件
5、一级缓存和二级缓存？
6、项目中的应用服务器？
aparch、Tomcat、Netty
7、线程池的执行原理？
8、还有什么要问的吗？
项目的模块的开发流程？ 
## 1.3、天津云客科技（北京）
1、简单的自我介绍
2、String和StringBuilder的区别？
3 、Collection和Collections区别？
4、一段代码在遍历ArrayList时移除一个元素
5、说出以下代码的运行结果?
```
public class TestLong {
    public static void main(String[] args){

        Long a=151L;
        Long b=151L;
        System.out.println(a==b);

        long e=151;
        long f=151;
        System.out.println(e==f);

        String s1="abc";
        String s2="abc";
        System.out.println("------------------------------");
        String s3 = new String("abc");
        String s4 = new String("abc");
        System.out.println(s1==s2); 
        System.out.println(s1.equals(s2));

        System.out.println(s3==s4);
        System.out.println(s3.equals(s4));

        System.out.println(s1.equals(s3));
        System.out.println(s1==s3);

    }
}
```
6、JAVA构造器是否可以被重写
7、JVM内存模型
## 1.4、博彦科技
内网和外网？
## 1.5、中科软
1. JDK、JRE、JVM
2. ==和equals?
3. final关键字
4. redis持久化机制？
5. Redis实现分布式锁？
6. MySql事务隔离级别？
7. MySql默认的事务隔离级别？

redis缓存击穿和雪崩？
session 和Cokie区别？
## 1.5、书生科技
Nginx?
