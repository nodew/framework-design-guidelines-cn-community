# 勘误表

## 序言

## 第一章，“导论”

## 第二章，“框架设计基础”

## 第三章，“命名准则”，

## 第四章，“类型设计准则”

### 4.7 结构体设计

1. P82 示例代码

原文：

```csharp
// 错误设计
public struct PositiveInteger
{
    private int value;

    public PositiveInteger(int value)
    {
        if (value <= 0) throw new ArgumentException(...);
        _value = value
    }

    public override string ToString()
    {
        return _value.ToString();
    }
}
```

更正为：

```csharp
// 错误设计
public struct PositiveInteger
{
    private int _value; // 译者注：英文原本中此处为字段名为 value，此处更正为 _value

    public PositiveInteger(int value)
    {
        if (value <= 0) throw new ArgumentException(...);
        _value = value
    }

    public override string ToString()
    {
        return _value.ToString();
    }
}
```

### 4.8.1 设计标记枚举

1. P89 示例代码

原文：

```csharp
[Flags]
public enum WatcherChangeTypes
{
    None = 0,
    Created = 0x0002,
    Deleted = 0x0004,
    Changed = 0x0008,
    Renamed = 0x0016,
}
```

更正为：

```csharp
[Flags]
public enum WatcherChangeTypes
{
    None = 0,
    Created = 0x0002,
    Deleted = 0x0004,
    Changed = 0x0008,
    Renamed = 0x0010,
}
```

2. P91 示例代码

原文：

```csharp
[Flags]
public enum MemberScopes
{
    None = 0,
    nstance = 0x4,
    Static = 0x8,
}
```

更正为：

```csharp
[Flags]
public enum MemberScopes
{
    None = 0,
    Instance = 0x4,
    Static = 0x8,
}
```

## 第五章，“成员设计准则”

## 第六章，“可扩展性设计”

### 6.2 基类

1. P184 最后一段示例代码

原文：

```csharp
public Directory {
    public Collection<string> GetFilenames(){
        return new FilenameCollection(this);
        }

    private class FilenameCollection : Collection<string> {
        ...
    }
}
```

更正为：

```csharp
public class Directory {
    public Collection<string> GetFilenames(){
        return new FilenameCollection(this);
    }

    private class FilenameCollection : Collection<string> {
        ...
    }
}
```

## 第七章，“异常”

### 7.2 选择抛出正确的异常类型

1. P198 第三段

> **KRZYSZTOF CWALINA** 请记住，将代码中所抛出的异常变更为该异常的子类型，并不是一种破坏性的变更。例如，如果你的框架的第一个版本抛出<del>`FileNotFoundExcption`</del><ins>`FooException`</ins>，
> 在该库的任何后续版本中，你都可以抛出 <del>`FileNotFoundExcption`</del><ins>`FooException`</ins> 的子类型，而且不会破坏针对库的前一个版本所编译的代码。
> 这意味着，当有疑虑时，我会考虑先不去创建新的异常类型，直到你确定你需要它们。在这一点上，你可以通过继承当前抛出的类型来创建一个新的异常类型。
> 有时，这可能会导致稍显奇怪的异常继承结构（例如，继承自 <del>`FileNotFoundExcption`</del><ins>`InvalidOperationException`</ins> 的自定义异常），
> 但与使你的库中拥有不必要的异常类型相比，这并不是什么大问题，这些不必要的异常类型使库更加复杂，增加了开发成本，增加了工作集，等等。

2. P198 第四段

> **× AVOID** 避免创建调用时可能导致系统故障的 API。如果的确会出现这样的故障，应该调用 <del>`FileNotFoundExcption`</del><ins>`Environment.FailFast`</ins>，而不是在系统故障发生时抛出异常。
>
>    系统故障是指开发者或程序无法处理的错误。好一点的是，在可复用库中，系统故障十分罕见。它们大多是由执行引擎引起的。在这种情况下，
> 停止进程的最好方法是调用 <del>`FileNotFoundExcption`</del><ins>`Environment.FailFast`</ins>，它记录了系统的状态——这对问题诊断非常有用。

3. P198 第六段

> **√ DO** 要合理地抛出最具体（派生最末端）的异常。
>
>    例如，如果在参数传入了空值，应该抛出 <del>`FileNotFoundExcption`</del><ins>`ArgumentNullException`</ins>，而不是它的基类 <del>`FileNotFoundExcption`</del><ins>`ArgumentException`</ins>。

4. P199 第一段

> **JEFFREY RICHTER** 直接抛出 <del>`FileNotFoundExcption`</del><ins>`System.Exception`</ins>——它是所有符合 CLS 的异常的共同基类——始终都是不正确的。

5. P199 第二段

> **BRENT RECTOR** 正如后面详细描述的那样，直接捕获 <del>`FileNotFoundExcption`</del><ins>`System.Exception`</ins> 也几乎都是错误的行为。

## 第八章，“使用准则”

## 第九章，“通用设计模式”

### 9.2.2 基于任务的异步模式

1. P258 第一段
...基于任务的异步模式可以直接使用这些类型手动实现，或者结合语言特性支持（如 C\# 中的 async/await，Visual Basic .NET 中的 <del>async</del><ins>Async</ins>/Await，以及 F\# 中的 async/let!/use!/do!）。该模式由以下元素组成：

2. P259 第二段示例代码的注释

原文：

```csharp
// 正确：支持 Cancellation 作为可选参数
public Task WriteAsync(
    string text,
    CancellationToken cancellationToken = default) { ... }
```

更正为：

```csharp
// 正确：支持 CancellationToken 作为可选参数
public Task WriteAsync(
    string text,
    CancellationToken cancellationToken = default) { ... }
```
### 9.2.4 为现有的同步方法制作一个异步变体

1. P266 第二段示例代码

原文：

```csharp
public readonly struct SomeResult {
    public int CalculatedResult { get; }
    public string UpdatedValue { get; }

    public DivisionResult(int result, string updatedValue) { ... }
}
```

更正为：

```csharp
public readonly struct SomeResult {
    public int CalculatedResult { get; }
    public string UpdatedValue { get; }

    public SomeResult(int result, string updatedValue) { ... }
}
```

### 9.2.5 异步模式一致性的实现准则

1. P270 倒数第二条准则

> **× DON'T** 不要在异<del>常</del><ins>步</ins>方法中调用 `Task.Wait()` 或读取 `Task.Result` 属性；相反，使用 await。

### 9.2.10 await foreach 的使用准则

1. P275 - P276 示例代码

原文：

```csharp
// 错误
public Task<int> MaxAsync(
    IAsyncEnumerable<int> source,
    CancellationToken cancellationToken = default) {

    int max = int.MinValue;
    bool hasValue = false;

    // cancellationToken 没有被使用
    async foreach (int value in source.ConfigureAwait(false)) {
        hasValue = true;
        if (value > max) {
            max = value;
        }
    }

    return hasValue ? max : throw new InvalidOperationException();
}

// 正确
public Task<int> MaxAsync(
    IAsyncEnumerable<int> source,
    CancellationToken cancellationToken = default) {

    int max = int.MinValue;
    bool hasValue = false;

    // cancellationToken 被传入了迭代器中
    async foreach (
        int value in source.WithCancellation(cancellationToken).ConfigureAwait(false)
    ) {
        hasValue = true;
        if (value > max) {
            max = value;
        }
    }

    return hasValue ? max : throw new InvalidOperationException();
}
```

更正为：

```csharp
// 错误
public async Task<int> MaxAsync(
    IAsyncEnumerable<int> source,
    CancellationToken cancellationToken = default) {

    int max = int.MinValue;
    bool hasValue = false;

    // cancellationToken 没有被使用
    await foreach (int value in source.ConfigureAwait(false)) {
        hasValue = true;
        if (value > max) {
            max = value;
        }
    }

    return hasValue ? max : throw new InvalidOperationException();
}

// 正确
public async Task<int> MaxAsync(
    IAsyncEnumerable<int> source,
    CancellationToken cancellationToken = default) {

    int max = int.MinValue;
    bool hasValue = false;

    // cancellationToken 被传入了迭代器中
    await foreach (
        int value in source.WithCancellation(cancellationToken).ConfigureAwait(false)
    ) {
        hasValue = true;
        if (value > max) {
            max = value;
        }
    }

    return hasValue ? max : throw new InvalidOperationException();
}
```

原文：

```csharp
// 不需要 WithCancellation(cancellationToken)，
// 因为它已经被传入了 ValueGenerator。
async foreach (int value in
    ValueGenerator(10, 5, cancellationToken).ConfigureAwait(false)) {
    ...
}
```

更正为：

```csharp
// 不需要 WithCancellation(cancellationToken)，
// 因为它已经被传入了 ValueGenerator。
await foreach (int value in
    ValueGenerator(10, 5, cancellationToken).ConfigureAwait(false)) {
    ...
}
```
### 9.6.2 实现 LINQ 支持的方法

1. P308 最后一段

> **√ DO** 当需要对查询进行检查时，要使用 `Expression<Func<...>>` 作为参数，而不是 `Func<...>`。
> 更多细节见 <del>9.6.5</del> <ins>9.6.2.3</ins> 节。

2. P309 第二段

> 另一个使用表达式的原因是执行时优化。例如，一个<del>排序的</del><ins>有序的</ins>列表可以用<del>二进制搜索</del><ins>二分查找</ins>实现查找（Where 语句），
> 这比标准的 `IEnumerable<T>` 或 `IQueryable<T>` 实现要高效得多。

## 附录 A，“C# 代码风格约定”

## 附录 B，“过时的指南”

## 附录 C，“API 规范示例”

## 附录 D，“不兼容变更”
