---
layout: post
title:  "观察者模式详解"
date:   2019-09-27 17:14:54
categories: 设计模式
tags: 观察者
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}














![](https://user-gold-cdn.xitu.io/2019/10/10/16db4fe8f783b8e6?w=1011&h=771&f=png&s=2343102)

本篇文章总结一下观察者模式，主要从以上几点介绍。
![声明](https://user-gold-cdn.xitu.io/2019/5/22/16adf0c4dd3185d5?w=1080&h=237&f=png&s=1025845) 

### 概念介绍

观察模式是我们在开发过程中经常遇到的一种设计模式，这里先来介绍一下概念。

从字面意思上去理解，所谓的观察者模式，首先有观察者（一个或者多个），被观察者（一个）。当被观察者状态发生变化的时候，就会去通知它的所有的观察者，然后由观察者根据被观察者的情况作出反应。观察者模式属于行为型模式。

在 Android 中的接口回调属于一种特殊的观察者模式，观察者只有一个（监听器）。

在观察者模式中有以下四个重要的角色：

**Subject**  被观察者的抽象接口，在这个接口里面，提供添加观察者和删除观察者、通知观察者的方法。

**ConcreteSubject** 具体的被观察者，Subject 的实现类。在具体的内部发生改变时，给所有注册过的观察者发送通知。

**Observer**  观察者的抽象接口，在这个接口里面定义了一个更新方法，当收到被观察者的通知的时候就更新。

**ConcreteObserver** 具体的观察者，Observer 的实现类。实现 Observer 的更新方法。

### 图例解释

使用送奶工的图例来介绍：

![](https://user-gold-cdn.xitu.io/2019/10/10/16db5003189b654c?w=810&h=370&f=webp&s=7370)

送奶工就可以看做是一个被观察者，然后订奶的张三、赵四、王五就是观察者（观察送奶工有没有来），送奶工来送奶会通知各位观察者，通知他们来取奶。这个时候某六也想订奶，就可以告诉送奶工，以后他也要订奶了，以后来送奶的时候就也会通知某六了。

![](https://user-gold-cdn.xitu.io/2019/10/10/16db5007c4ad42e1?w=640&h=370&f=webp&s=6048)



![](https://user-gold-cdn.xitu.io/2019/10/10/16db500ab5919951?w=590&h=370&f=webp&s=6692)

某一天，张三不想喝奶了，就告诉送奶工，他不再订奶了，那么以后送奶工再来送奶就不会通知张三了。

**具体的代码实现**

根据上面的概念，需要有一个被观察者的抽象，也就是送奶工的抽象

```java
public interface Subject{
    // 注册观察者
    void registerObserver(Observer o);
    // 取消注册观察者
    void unregisterObserver(Observer o);
    // 通知观察者
    void notifyObserver();
}
```

具体的观察者对象

```java
public class ConcreteSubject implements Subject{
    // 存放观察者的集合
    private List<Observer> observers;
    public ConcreteSubject(){
        observers = new ArrayList<>();
    }
    // 注册观察者
    @Override
    public void registerObserver(Observer o){
        if（!observers.contains(o)）{
            observers.add(o);
        }
    }
    // 取消注册观察者
    @Override
    public void unregisterObserver(Observer o){
        if(observers.contains(o)){
            observers.remove(o);
        }
    }
    @Override
    public void notifyObserver(){
        for(int i=0;i<observers.size();i++){
            observers.get(i).update();
        }
    }
    
}
```

观察者抽象接口

```java
public interface Observer{
    void update();
}
```

观察者具体的实现

```java
public class ConcreteObserver implements Observer{
    private Subject subject;
    public ConcreteObserver(Subject subject){
        this.subject = subject;
        this.subject.registerObserver(this);
    }
    @Override
    public void update(){
        // 具体的实现
    }
}
```

到此关于观察者和被观察者都已经写完了，这仅仅是最简单的写法，当然 `update()` 方法里面可以传递参数，具体的实现让被观察者来实现。这就具体情况具体写了。

下面写 `main` 方法来测试

```java
public static void main(String[] args){
    // 创建被观察者
    Subject subject = new ConcreteSubject();
    // 创建观察者
    Observer one = new ConcreteObserver(subject);
    // 创建观察者
    Observer two = new ConcreteObserver(subject);
    // 创建观察者
    Observer three = new ConcreteObserver(subject);
    // 状态发生改变，通知观察者
    subject.notifyObserver();
}
```

### 实际运用实例

在 Java 中实现观察模式，需要借助系统 API 提供的类，在包 `java.util` 中的 `Observer` 和 `Observable` 。被观察者需要继承 Observable 类，观察者需要实现接口 `Observer` 并且实现其中的 `update(Observable o,Object arg)` 方法

其实虽然系统提供了 `Observer` 和 `Observable` ，通过看源码你会发现它们的实现和我们上面写的例子是很像的，没什么难点。`Observable` 类里面主要就是有存放 `Observer` 的集合和方法 `addObserver(Observer o)`  、`deleteObserver(Observer o)` 、`notifyObservers(Object arg)` 观察者接口 `Observer` 内部主要有方法`update(Observable o,Object arg)` 

**简单运用**

房子的价格和我们的生活息息相关，人们都很关注房价，假设房子是被观察者，人们是观察者，当房价发生变化的时候就会通知人们，这就就是一个观察者模式，用 Java 代码体现，下面列出核心代码，具体代码见链接

被观察者

```java
public class House extends Observable {
    private float house_price;

    public House(float house_price) {
        this.house_price = house_price;
    }

    public void updatePrice(float house_price) {
        this.house_price = house_price;
        // 设置变化点
        setChanged();
        // 通知观察者
        notifyObservers(house_price);
    }

    @NonNull
    @Override
    public String toString() {
        return "当前房子价格：" + house_price;
    }
}
```

观察者

```java
public class People implements Observer {
    private String name;

    public People(String name) {
        this.name = name;
    }

    @Override
    public void update(Observable o, Object arg) {
        Float price = (Float) arg;
        Log.e("update", "我是观察者" + name + "房子价格变了：" + price);
        if (price > 20000) {
            Log.e("update:", "太TM贵啦，不关注了！");
            o.deleteObserver(this);
        }
    }
}
```

验证：

```java
public class DesignModeActivity extends BaseActivity {

    @BindView(R.id.bt_change)
    Button btChange;
    @BindView(R.id.et_price)
    EditText etPrice;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_observer);
        ButterKnife.bind(this);
        init();
    }

    @Override
    public void initView() {
        super.initView();
        // 房子 属于被观察者
        House house = new House(10000);
        // 创建观察者
        People people = new People("张三");
        People people1 = new People("李四");
        People people2 = new People("王五");
        // 注册观察者
        house.addObserver(people);
        house.addObserver(people1);
        house.addObserver(people2);

        btChange.setOnClickListener(v -> {
            float price = Float.parseFloat(etPrice.getText().toString());
            house.updatePrice(price);
        });

    }
}

```

显示结果：

![](https://user-gold-cdn.xitu.io/2019/10/10/16db50163d755685?w=577&h=303&f=png&s=525757)

**重点：** 需要将观察者一一添加到被观察者中，当被观察者对象内容发生变化的时候调用 `setChanged()` 和 `notifyObservers()`  来通知观察者

### Android 中的观察者

在 Android 中经常用到观察者模式，最常用的就是设计点击监听事件（这是一种特殊的观察者模式，观察者只有一个）、ContentObserver、android.database.Observable 等；还有组件通讯库`RxJava`、`RxAndroid` 、`EventBus` 等。这里拿 Adapter 中的观察者模式来举例：

先来看一下 `BaseAdapter` 的部分源码

```java
public abstract class BaseAdapter implements ListAdapter,SpinnerAdapter{
    // 数据是否变化的被观察者
    private final DataSetObservable mDataSetObservable = new DataSetObservable();
    
    public boolean hasStablelds(){
        return false;
    }
    public void registerDataSetObserver(DataSetObserver observer){
        // 其实就是 mDataSetObservable 内部有一个 List 用来保存观察者
        mDataSetObservable.registerObserver(observer);
    }
    public void unregisterDataSetObserver(DataSetObserver observer){
        mDataSetObservable.unregisterObserver(observer);
    }
    // 当数据集合发生变化的时候，通知所有的观察者
    public void notifyDataSetChanged(){
        mDataSetObservable.notifyChanged();
    }
    
}

```

从源码中可以很容易的看到，这里有 `被观察者对象 mDataSetObservable`  当数据发生变化的时候，我们就调用 `notifyDataSetChanged()` 方法，这个时候 `被观察者对象` 就会通知观察者作出反应。就是这么一个过程！

整个过程我们都了解了，下面就是进一步看看代码是怎么实现的：看看 `mDataSetObservable.notifyChanged()` 做了什么

```java
public class DataSetObservable extends Observable<DataSetObserver>{
    // 这个方法就是挨个通知观察者，其中储存观察者的 List 在 Observable 中，包括 registerObserver 和 unregisterObserver 方法
    public void notifyChanged(){
        synchronized(mObservers){
            for(int i = mObservers.size-1;i>=0;i--){
                mObservers.get(i).onChanged();
            }
        }
    }
}

```

这里的 mObservers 就是观察者的集合，这些观察者是在 ListView 通过 setAdapter() 设置 Adapter 时产生的：

```java
@Override
public void setAdapter(ListAdapter adapter){
    if(mAdapter!= null && mDataSetObserver != null){
        mAdapter.unregisterDataSetObserver(mDataSetObserver);
    }
    // .... 省略代码
    super.setAdapter(adapter);
    
    if(mAdapter != null){
        mAreAllItemsSelectable = mAdapter.areAllItemsEnabled();
        mOldItemCount = mItemCount;
        mItemCount = mAdapter.getCount();
        checkFocus();
        //创建数据观察者
        mDataSetObserver = new AdapterDataSetObserver();
        //注册观察者
        mAdapter.registerDataSetObserver(mDataSetObserver);
        //... 省略代码
    }
       
}

```

再来看看观察者 AdapterDataSetObserver  的部分关键代码

```java
class AdapterDataSetObserver extends AdapterView<ListAdapter>.AdapterDataSetObserver{
    // 这个方法就是观察者在收到通知后，调用的方法
    @Override
    public void onChanged(){
        super.onChanged();
        if(mFastScroller != null){
            mFastScroller.onSectionsChanged();
        }
    }
    @Override
    public void onInvalidated(){
        super.onInvalidated();
        if(mFastScroller != null){
            mFastScroller.onSectionsChanged();
        }
    }
}

```

上面代码的主要实现部分都在它的父类，再来看看 AdapterDataSetObserver 的父类 AdapterView 的 AdapterDataSetObserver :

```java
class AdapterDataSetObserver extends DataSetObserver{
    private Parcelable mInstanceState = null;
    @Override
    public void onChanged(){
        mDataChanged = true;
        mOldItemCount = mItemCount;
        mItemCount = getAdapter().getCount();
        if(AdapterView.this.getAdapter().hasStableIds()&& mInstanceState != null && mOldItemCount==0 && mItemCount > 0){
            AdapterView.this.onRestoreInstanceState(mInstanceState);
            mInstanceState = null;
        }else{
            rememberSyncState();
        }
        checkFocus();
        // 重新布局
        requestLayout();
    }
    // .... 省略代码
    
    public void clearSavedState(){
        mInstanceState = null;
    }
}

```

在源码中可以看到在 `onChanged()` 方法中调用了 `requestLayout()` 方法来重新进行布局。到此 BaseAdapter 的观察模式就缕清楚了。

### 观察者模式的使用场景和优缺点

**使用场景**

- 事件多级触发场景
- 跨系统的消息交换场景，如消息队列、事件总线的处理机制

**优点**

解除耦合，让耦合的双方都依赖于抽象，从而使得各自的变换都不会影响到另一边的变换

**缺点**

在应用观察者模式时需要考虑一下开发效率和运行效率的问题，程序中包括一个被观察者，多个观察者，开发、调试等内容会比较复杂，而且在 Java 中消息的通知一般是顺序执行的，那么一个观察者卡顿，就会影响整体的执行效率，在这种情况下，一般会采用异步实现。

### 总结：

其实无论程序整体多复杂，观察者模式是永远不会变的，首先有一个被观察者，被观察者对象中持有观察者，当被观察者发生变化的时候，调用被观察者的方法通知观察者（这个方法内通常会是 观察者调用自己对应的方法）。

至于观察者什么时候被添加到被观察者中，观察者收到信号后如果处理，这就是具体情况具体处理了。

> 参考内容
> https://blog.csdn.net/itachi85/article/details/50773358>
> https://www.jianshu.com/p/9ee823cd94a9
> https://www.2cto.com/kf/201310/253013.html

![](https://user-gold-cdn.xitu.io/2019/5/22/16adf0cca1c726bb?w=2048&h=1365&f=jpeg&s=1855540)

