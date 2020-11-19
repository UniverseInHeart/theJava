> 针对类做代理使用的是`Cglib`,即使针对接口做代理，也可以将代理方式配置成走`Cglib`的

## 使用Cglib代码对类做代理

首先定义一个Dao类，里面有一个select()方法和一个update()方法：


```java
public class Student {
    public void toClass() {
        System.out.println("上课了");
    }
}
```

创建一个`DaoProxy`，实现`MethodInterceptor`接口，目标是在update()方法与select()方法调用前后输出两句话：

```java
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;
import java.lang.reflect.Method;

public class ProxyHandler implements MethodInterceptor {

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("开始了");
         // invokesuper执行的是原始类的方法，invoke执行的是子类的方法，
        // 如果invoke后面传进去参数是子类的话，就会引起循环调用。
        methodProxy.invokeSuper(o, objects);
        System.out.println("结束了");
        return o;
    }
}
```
    intercept方法的几个参数的含义为：
    + Object表示要进行增强的对象
    + Method表示拦截的方法
    + Object[]数组表示参数列表，基本数据类型需要传入其包装类型，如int-->Integer、long-Long、double-->Double
    + MethodProxy表示对方法的代理，invokeSuper方法表示对被代理对象方法的调用


写一个测试类：

```java
public class Test {

    public static void main(String[] args) {
        ProxyHandler proxyHandler = new ProxyHandler();

        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(Student.class);
        enhancer.setCallback(proxyHandler);

        Student student = (Student)enhancer.create();
        student.toClass();
    }
}
```

这是使用 `Cglib` 的通用写法，`setSuperclass` 表示设置要代理的类，`setCallback` 表示设置回调即 `MethodInterceptor` 的实现类，使用 `create()` 方法生成一个代理对象，注意要强转一下，因为返回的是 `Object`。

最后看一下运行结果：

开始了
上课了
结束了

符合我们的期望。

 ----------