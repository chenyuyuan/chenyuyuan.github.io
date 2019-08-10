---
title: Java-ArrayList
date: 2019-08-10 17:19:43
tags: Java
---

##### void clear(): 移除此列表中的所有元素

```java
public void clear() {
	modCount++;
	
    // clear to let GC do its work
	for(int = 0; i < size; i++)
		elementData[i] = null;
	
    size = 0;
}
```

使用null: 直接将变量list指向null，通常当我们不需要再使用ArrayList对象时，可以将变量值设为null，以便GC可以运作并回收这部分内存空间。需要注意的是当仍有其他变量指向该对象时，即使将变量list置为null垃圾回收器也无法回收该内存空间。

使用 = new ArrayList()，有点类似于使用clear()，都是得到一个空的ArrayList对象。不过new ArrayList()会得到一个初始化内部数组elementData容量为10的ArrayList对象，而方法1得到的对象的容量与原对象一致。值得注意的是使用方法3需要进行如在内存中重新开辟内存空间等操作，开销较大，如果只是单纯的想要使用空的ArrayList对象，建议使用方法1，相对来说可尽量避免堆内存溢出问题。