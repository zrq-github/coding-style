# C# Coding Style

author: zrq

- [C# Coding Style](#c-coding-style)
  - [Guides](#guides)
  - [Formatting guidelines](#formatting-guidelines)
    - [Naming rules](#naming-rules)
  - [Example](#example)
  - [C# coding guidelines](#c-coding-guidelines)
    - [Constants(常数)](#constants常数)
    - [IEnumerable vs IList vs IReadOnlyList](#ienumerable-vs-ilist-vs-ireadonlylist)
    - [Generators vs containers (类和容器)](#generators-vs-containers-类和容器)

C# Code Style

google公司中,C#风格的规范: [C# at Google Style Guide](https://google.github.io/styleguide/csharp-style.html)

Microsoft-dotnet-github: [C# Coding Style](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md)

Microsoft-dotnet-csharp-learn:[C# Coding Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)

以下的规范主要来自于

## Guides

(Google Style Guides) Every major open-source project has its own style guide: a set of conventions (sometimes arbitrary) about how to write code for that project. It is much easier to understand a large codebase when all the code in it is in a consistent style.

每个主要的开源项目都有自己的风格指南: 一套关于如何为该项目编写代码的约定(有时候是任意的)。当所有代码都采用一致的风格时，理解大型代码库就容易得多。

(zrq): C# at Google Style Guide, 很多都是依据的微软的规范.

## Formatting guidelines

### Naming rules

Rule summary:

**Code**:  

- Names of classes, methods, enumerations, public fields, public properties, namespaces: PascalCase.
- Names of local variables, parameters: camelCase.
- Names of private, protected, internal and protected internal fields and properties: _camelCase.
- Naming convention is unaffected by modifiers such as const, static, readonly, etc.
- For casing, a “word” is anything written without internal spaces, including acronyms. For example, MyRpc instead of MyRPC.
- Names of interfaces start with I, e.g. IInterface.

<br>

- 类名, 方法, 枚举, 公有字段, 公有属性, 命名空间: PascalCase.
- 本地变量(zrq: 临时变量), 参数: camelCase  
- 私有、受保护、内部和受保护的内部字段和属性: _camelCase
- 命名约定不受诸如const、static、readonly等修饰符的影响
  (微软规范跟这有一点不一样)

**Files**:

- 文件和目录的名字使用 PascalCase, e.g. MyFile.cs
- 无论何时尽可能确保一个 .cs 文件中对应相同名字的一个主类. e.g. MyClass.cs
- 总体上, 一个文件一个主类.

Files zrq e.g.:

这个地方不好定义, 什么样的程度为一个主类?

个人理解, 除主类外, 其他类都是为主类服务, 它们执行的功能都与主类一样.

什么样的类, 可以放在一块? 提供一种思路.

```C#
    internal class Data
    {
    }
    
    internal class DataList : ObservableCollection<Data>
    {
    }
```

Data: 存放数据结构
DataList: 存放一组数据

DataList对Data强关联, 脱离Data无法生效.

就看其他类, 脱离了这个主类后, 是否还能存在.

## Example

<details>
<summary>Example</summary>

```C#
using System;                                       // `using` goes at the top, outside the
                                                    // namespace.

namespace MyNamespace {                             // Namespaces are PascalCase.
                                                    // Indent after namespace.
  public interface IMyInterface {                   // Interfaces start with 'I'
    public int Calculate(float value, float exp);   // Methods are PascalCase
                                                    // ...and space after comma.
  }

  public enum MyEnum {                              // Enumerations are PascalCase.
    Yes,                                            // Enumerators are PascalCase.
    No,
  }

  public class MyClass {                            // Classes are PascalCase.
    public int Foo = 0;                             // Public member variables are
                                                    // PascalCase.
    public bool NoCounting = false;                 // Field initializers are encouraged.
    private class Results {
      public int NumNegativeResults = 0;
      public int NumPositiveResults = 0;
    }
    private Results _results;                       // Private member variables are
                                                    // _camelCase.
    public static int NumTimesCalled = 0;
    private const int _bar = 100;                   // const does not affect naming
                                                    // convention.
    private int[] _someTable = {                    // Container initializers use a 2
      2, 3, 4,                                      // space indent.
    }

    public MyClass() {
      _results = new Results {
        NumNegativeResults = 1,                     // Object initializers use a 2 space
        NumPositiveResults = 1,                     // indent.
      };
    }

    public int CalculateValue(int mulNumber) {      // No line break before opening brace.
      var resultValue = Foo * mulNumber;            // Local variables are camelCase.
      NumTimesCalled++;
      Foo += _bar;

      if (!NoCounting) {                            // No space after unary operator and
                                                    // space after 'if'.
        if (resultValue < 0) {                      // Braces used even when optional and
                                                    // spaces around comparison operator.
          _results.NumNegativeResults++;
        } else if (resultValue > 0) {               // No newline between brace and else.
          _results.NumPositiveResults++;
        }
      }

      return resultValue;
    }

    public void ExpressionBodies() {
      // For simple lambdas, fit on one line if possible, no brackets or braces required.
      Func<int, int> increment = x => x + 1;

      // Closing brace aligns with first character on line that includes the opening brace.
      Func<int, int, long> difference1 = (x, y) => {
        long diff = (long)x - y;
        return diff >= 0 ? diff : -diff;
      };

      // If defining after a continuation line break, indent the whole body.
      Func<int, int, long> difference2 =
          (x, y) => {
            long diff = (long)x - y;
            return diff >= 0 ? diff : -diff;
          };

      // Inline lambda arguments also follow these rules. Prefer a leading newline before
      // groups of arguments if they include lambdas.
      CallWithDelegate(
          (x, y) => {
            long diff = (long)x - y;
            return diff >= 0 ? diff : -diff;
          });
    }

    void DoNothing() {}                             // Empty blocks may be concise.

    // If possible, wrap arguments by aligning newlines with the first argument.
    void AVeryLongFunctionNameThatCausesLineWrappingProblems(int longArgumentName,
                                                             int p1, int p2) {}

    // If aligning argument lines with the first argument doesn't fit, or is difficult to
    // read, wrap all arguments on new lines with a 4 space indent.
    void AnotherLongFunctionNameThatCausesLineWrappingProblems(
        int longArgumentName, int longArgumentName2, int longArgumentName3) {}

    void CallingLongFunctionName() {
      int veryLongArgumentName = 1234;
      int shortArg = 1;
      // If possible, wrap arguments by aligning newlines with the first argument.
      AnotherLongFunctionNameThatCausesLineWrappingProblems(shortArg, shortArg,
                                                            veryLongArgumentName);
      // If aligning argument lines with the first argument doesn't fit, or is difficult to
      // read, wrap all arguments on new lines with a 4 space indent.
      AnotherLongFunctionNameThatCausesLineWrappingProblems(
          veryLongArgumentName, veryLongArgumentName, veryLongArgumentName);
    }
  }
}
```

</details>

## C# coding guidelines

### Constants(常数)

- 变量和字段, 尽可能使用 const
- 如果 const 不行, 使用 readonly

zrq: 因为常数一样来说是不能被修改的.

### IEnumerable vs IList vs IReadOnlyList

- 对于输入，使用最严格的集合类型. 当输入的集合不可变, 使用 IReadOnlyCollection / IReadOnlyList / IEnumerable.
- 对于输出，如果将返回容器的所有权传递给所有者，首选IList而不是IEnumerable。如果不转让所有权，最好选择最具限制性的选项.

zrq-带来的好处思考:

- 对于 T 是引用对象来说, 对象内的元素仍然可以更改, 但是那些需要通过 new 来进行赋值的元素是没办法被修改的.
- 好的类设计规范, 结合只读接口能极大的减少因为误操作, 从而导致数据源被更改
- 更加能减少 new class() 后 GetHashCode() 改变, 从而导致 Dictionary 出错, 加强面对对象的概念.

### Generators vs containers (类和容器)

- Use your best judgement, bearing in mind:
  - Generator code is often less readable than filling in a container.
  - Generator code can be more performant if the results are going to be processed lazily, e.g. when not all the results are needed.
  - Generator code that is directly turned into a container via ToList() will be less performant than filling in a container directly.
  - Generator code that is called multiple times will be considerably slower than iterating over a container multiple times.

[What is generator code?](https://softwareengineering.stackexchange.com/questions/411877/what-is-generator-code)
