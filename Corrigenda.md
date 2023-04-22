# 勘误表

## 序言

## 第一章，“导论”

## 第二章，“框架设计基础”

## 第三章，“命名准则”，

## 第四章，“类型设计准则”

### 4.7 结构体设计

1. P82 第一段代码
    
    私有字段 `value` 应为 `_value`。

```
// 错误设计
public struct PositiveInteger
{
    private int _value; // 译者注：英文原本中此处为字段名为 value，应为 _value

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

## 第五章，“成员设计准则”

## 第六章，“可扩展性设计”

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

## 附录 A，“C# 代码风格约定”

## 附录 B，“过时的指南”

## 附录 C，“API 规范示例”

## 附录 D，“不兼容变更”