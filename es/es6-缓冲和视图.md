# 缓冲和视图

time: 2019.01.22

## 1 es6-类数组

在 javascript 中类数组对象有多个，比如: ArrayBuffer, arguments, domList ...  
类数组和正常的数组 Array 不同，通过调用 Array.isArray() 来判断类数组，会返回 false。

对于原始二进制数据的存储于访问，不能通过普通 Array 对象及其方法来实现。  
javascript 采用了缓冲和视图来实现，用以保证最大的灵活性和效率。

## 2 缓冲和视图

缓冲：由 ArrayBuffer 对象实现，它表示的是一个二进制数据数据缓冲区，没有具体的格式，由宿主环境自己实现，用户不能直接操作访问。

视图：为了访问到缓冲区中的数据，javascript 提供了一些视图数值类型，视图提供了上下文，用户可以直接操作视图。

****

类数组视图: Int8Array, Unit8Array, Unit8ClampedArray, Int16Array, Unit16Array, Int32Array, Unit32Array, Float32Array, Float64Array

数据视图: DataView 是一种底层接口，它提供了操作缓冲区中任意数据的读写接口。

****

构建 ArrayBuffer 对象，以及创建它的视图例子

```javascript
var buffer = new ArrayBuffer(16);
var int32View = new Int32Array(buffer);
for (var i = 0; i < int32View.length; i++) {
  int32View[i] = i * 2;
}
```

## 3 应用场景

webGL, fileReader ...

## 参考文章

[mdn javascript 类数组对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Typed_arrays)
