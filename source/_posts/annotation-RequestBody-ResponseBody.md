---
title: Annotation RequestBody & ResponseBody
date: 2016-12-18 19:01:29
tags:
category: Spring
---

使用 `@RequestBody`和`@ResponseBody` 可以自动实现对象和xml/json之间的转换，当然，在这之前需要以下步骤：
<!--more-->

* 在配置文件添加：
```
    <context:component-scan base-package="com.henry.spring" />
    <mvc:annotation-driven/>
```
`mvc:annotation-driven`会告知Spring，我们要启用注解驱动。然后Spring会自动为我们注册相应的Bean到工厂中，来处理我们的请求。

* 默认采用的是`Jackson`，在`pom`文件中添加依赖
```
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.8.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.8.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.8.5</version>
        </dependency>
```

Spring的消息转换机制如下图：

![](/img/httpmessageconverter.JPG)  


### 定义Bean
```
public class Cat {
    private String color;
    private String name;
    ....... getter and setter .....

}
```
```
@XmlRootElement
public class Dog {

    @XmlElement
    private String color;
    @XmlElement
    private String name;
....... getter and setter .....
}
```
  
### 定义Controller  

```
@Controller
@RequestMapping("/")
public class AnnotationController {

    private static  Logger logger = Logger.getLogger(AnnotationController.class);

    @RequestMapping(value = "/cat", method = RequestMethod.GET)
    @ResponseBody
    public Cat getCat(){
        Cat cat = new Cat();
        cat.setName("miao");
        cat.setColor("white");

        return cat;
    }

    @RequestMapping(value = "/cat", method = RequestMethod.POST)
    @ResponseBody
    public void saveCat(@RequestBody Cat cat) {
        logger.info(cat.getColor() + ";;;;" + cat.getName());
    }

    @RequestMapping(value = "/dog", method = RequestMethod.GET)
    @ResponseBody
    public Dog getDog(){
        Dog dog = new Dog();
        dog.setName("wangwang");
        dog.setColor("black");

        return dog;
    }

}
```

* 对于请求1 GET /cat 会返回 {"color":"white","name":"miao"}
* 对于请求2 
``` 
POST /cat {"color":"white","name":"miao"}  
```

request入参会直接转换为 object `cat`  
* 对于请求3 GET /dog 会返回 
```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<dog>
    <color>black</color>
    <name>wangwang</name>
</dog>
```
