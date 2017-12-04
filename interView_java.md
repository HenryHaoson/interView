### HashMap,HashTable实现原理，线程安全性，Hash冲突处理算法，ConCurrenthashMap


### 类卸载

类卸载是内存优化的一种手段。

类加载器有虚拟机自带的类加载器:跟类加载器，扩展类及载器，系统类加载器；还有自定义的类加载器。

因为虚拟机一直保持对自带类加载器的引用，这类类加载器有引用着加载的类，所以虚拟机自带的类加载器是不支持卸载的。

而自定义的类加载器不会被虚拟机一直引用，所以自定义类加载器加载的类支持类卸载。


### 如何控制一个方法允许并发访问的线程的个数

类 Semaphore（一个同步辅助类，它允许一组线程互相等待，直到到达某个公共屏障点 ）

### ArrayList的动态扩容机制

当在 ArrayList 中增加一个对象时 Java 会去检查 Arraylist 以确保已存在的数组中有足够的容量来存储这个新对象，如果没有足够容量就新建一个长度更长的数组（原来的1.5倍），旧的数组就会使用 Arrays.copyOf 方法被复制到新的数组中去，现有的数组引用指向了新的数组。下面代码展示为 Java 1.8 中通过 ArrayList.add 方法添加元素时，内部会自动扩容，扩容流程如下：

```java

//确保容量够用，内部会尝试扩容，如果需要
ensureCapacityInternal(size + 1)
//在未指定容量的情况下，容量为DEFAULT_CAPACITY = 10
//并且在第一次使用时创建容器数组，在存储过一次数据后，数组的真实容量至少DEFAULT_CAPACITY
private void ensureCapacityInternal(int minCapacity) {
//判断当前的元素容器是否是初始的空数组
if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
//如果是默认的空数组，则 minCapacity 至少为DEFAULT_CAPACITY
minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
}
ensureExplicitCapacity(minCapacity);
}
//通过该方法进行真实准确扩容尝试的操作
private void ensureExplicitCapacity(int minCapacity) {
modCount++;//记录List的结构修改的次数
    //需要扩容
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}
//扩容操作
private void grow(int minCapacity) {
//原来的容量
int oldCapacity = elementData.length;
//新的容量 = 原来的容量 + （原来的容量的一半）
int newCapacity = oldCapacity + (oldCapacity >> 1);

//如果计算的新的容量比指定的扩容容量小，那么就使用指定的容量
if (newCapacity - minCapacity < 0)
    newCapacity = minCapacity;

//如果新的容量大于MAX_ARRAY_SIZE(Integer.MAX_VALUE - 8)
//那么就使用hugeCapacity进行容量分配
if (newCapacity - MAX_ARRAY_SIZE > 0)
    newCapacity = (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;

//创建长度为newCapacity的数组，并复制原来的元素到新的容器，完成ArrayList的内部扩容
elementData = Arrays.copyOf(elementData, newCapacity);
}

```

### 顺带说一下remove操作

看代码就行和注释就行。

```java

 public boolean remove(Object o) {
	//如果传进来要remove的对象是空的。就遍历一下，将里面的空对象都remove。
        if (o == null) {
            for (int index = 0; index < size; index++)
                if (elementData[index] == null) {
                    fastRemove(index);
                    return true;
                }
        } else {
	//如果传进来的对象不是空的，就看里面有没有这个对象，有就删除。
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }

``` 


