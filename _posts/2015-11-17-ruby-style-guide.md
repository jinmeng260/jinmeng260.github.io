---
layout: post
title:  Ruby Style Guide
tags: ruby guide
category: guide
---

# 序幕

https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md

> 榜样很重要。<br>
> ——墨菲警官《机器战警》

身为 Ruby 开发者，有件总是令我烦心的事——Python 开发者有一份好的编程风格参考指南（[PEP-8](http://www.python.org/dev/peps/pep-0008/)）而我们永远没有一份官方指南，一份记录 Ruby 编程风格及最佳实践的指南。我确信风格很重要。我也相信像 Ruby 这样的黑客社区，应该可以自己写一份这个梦寐以求的文档。

这份指南开始是作为我们公司内部的 Ruby 编程指南 (在下所写的)。后来，我决定要把成果贡献给广大的 Ruby 社区，况且这个世界再多一份公司司内部文件又有何意义。然而由社区制定及策动的一系列 Ruby 编程惯例、实践及风格，确能让世界收益。

从编写这份指南开始，我收到了优秀 Ruby 社区的很多用户反馈。感谢所有的建议及帮助！同心协力，我们能创造出让每一个 Ruby 开发者受益的资源。

顺道一提，如果你对 Rails 感兴趣，你可以看看这份 [Ruby on Rails 风格指南](https://github.com/bbatsov/rails-style-guide) 作为补充。

# Ruby 风格指南

这份 Ruby 风格指南向你推荐实际使用中的最佳实践，Ruby 程序员如何写出可被别的 Ruby 程序员维护的代码。我们只说实际使用中的用法。指南再好，但里面说的过于理想化结果大家拒绝使用或者可能根本没人用，又有何意义。

本指南依照相关规则分成数个小节。我尽力在规则后面说明理由（如果省略了说明，那是因为其理由显而易见）。

规则不是我凭空想出来的——绝大部分来自我作为从业多年的职业软件工程师的经验，从 Ruby 社区成员得到的反馈及建议，和几个评价甚高的 Ruby 编程资源，像 [《Programming Ruby 1.9》](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0) 以及 [《The Ruby Programming Language》](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177)。

本指南仍在完善中——某些规则缺乏实例，某些例子也不够清楚。到时候都会解决的——放心吧。

你可以使用 [Transmuter](https://github.com/TechnoGate/transmuter) 生成本指南的 PDF 或 HTML 版本。

[RuboCop](https://github.com/bbatsov/rubocop) 项目会自动检查你的 Ruby 代码是否符合这份 Ruby 风格指南。

本指南有以下翻译版本：

* [简体中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [繁體中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [法文](https://github.com/porecreat/ruby-style-guide/blob/master/README-frFR.md)
* [德文](https://github.com/arbox/de-ruby-style-guide/blob/master/README-deDE.md)
* [日文](https://github.com/fortissimo1997/ruby-style-guide/blob/japanese/README.ja.md)
* [韩文](https://github.com/dalzony/ruby-style-guide/blob/master/README-koKR.md)
* [葡萄牙文](https://github.com/rubensmabueno/ruby-style-guide/blob/master/README-PT-BR.md)
* [俄文](https://github.com/arbox/ruby-style-guide/blob/master/README-ruRU.md)
* [西班牙文](https://github.com/alemohamad/ruby-style-guide/blob/master/README-esLA.md)
* [越南文](https://github.com/scrum2b/ruby-style-guide/blob/master/README-viVN.md)


## 目录

* [源代码排版](#源代码排版)
* [语法](#语法)
* [命名](#命名)
* [注释](#注释)
    * [注解](#注解)
* [类别](#类与模块)
* [异常](#异常)
* [集合](#集合)
* [字符串](#字符串)
* [正则表达式](#正则表达式)
* [百分比字面](#百分比字面)
* [元编程](#元编程)
* [其它](#其它)
* [工具](#工具)

## 源代码排版

> 所有风格都又丑又难读，自己的除外。几乎人人都这样想。把“自己的除外”拿掉，他们或许是对的...<br>
> ——Jerry Coffin（论缩排）

* 使用 `UTF-8` 作为源文件的编码。
* 每个缩排层级使用两个**空格**。不要使用硬 tab。

    ```Ruby
    # 差 - 四个空格
    def some_method
        do_something
    end

    # 好
    def some_method
      do_something
    end
    ```

* 使用 Unix 风格的换行符。（BSD/Solaris/Linux/OSX 的用户不用担心，Windows 用户要格外小心。）
    * 如果你使用 Git ，可用下面这个配置，来保护你的项目不被 Windows 的换行符干扰：

      ```bash
      $ git config --global core.autocrlf true
      ```

* 不使用 `;` 隔开语句和表达式。推论——一行一条语句。

    ```Ruby
    # 差
    puts 'foobar'; # 不必要的分号

    puts 'foo'; puts 'bar' # 一行里有两个表达式

    # 好
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # 仅对 puts 适用
    ```

* 对于没有成员的类，尽可能使用单行类定义。

    ```Ruby
    # 差
    class FooError < StandardError
    end

    # 勉强可以
    class FooError < StandardError; end

    # 好
    FooError = Class.new(StandardError)
    ```

* 定义方法时避免单行写法。尽管还是有些人喜欢这么用的。但是单行定义很容易出错，因为它在语法上有些古怪。无论如何——一个单行方法里的表达式不应该多于 1 个。

    ```Ruby
    # 差
    def too_much; something; something_else; end

    # 勉强可以——注意第一个 ; 是必需的
    def no_braces_method; body end

    # 勉强可以——注意第二个 ; 是可选的
    def no_braces_method; body; end

    # 勉强可以——语法上正确，但是没有 ; 让它有些难读
    def some_method() body end

    # 好
    def some_method
      body
    end
    ```

    这个规则的一个例外是空方法。

    ```Ruby
    # 好
    def no_op; end
    ```

* 操作符前后的空格。在逗号 `,` 、冒号 `:` 及分号 `;` 之后，在 `{` 前后，在 `}` 之前。
  Ruby 解释器（大部分情况下）忽略空格。但要写出可读性高的代码，正确使用空格是关键。

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    [1, 2, 3].each { |e| puts e }
    class FooError < StandardError; end
    ```

    （针对操作符）唯一的例外是当使用指数操作符时：

    ```Ruby
    # 差
    e = M * c ** 2

    # 好
    e = M * c**2
    ```

    `{` 和 `}` 需要额外说明，因为他们是用在块（block）、
    哈希字面量（hash literals），以及字符串插值中。
    对于哈希字面量来说，两种风格都是可接受的。

    ```Ruby
    # 好——`{` 之后和 `}` 之前有空格
    { one: 1, two: 2 }

    # 好——`{` 之后和 `}` 之前没有空格
    {one: 1, two: 2}
    ```

    第一个种风格稍微更具可读性（而且有争议的是，一般在 Ruby 社区里更受欢迎）。
    第二种风格具有可为块和哈希字面量添加可视化的差别的优点。
    无论你选哪一种都行——但是最好保持一致。


* `(` 、 `[` 之后， `]` 、 `)` 之前，不要有空格。

    ```Ruby
    some(arg).other
    [1, 2, 3].size
    ```


* `!` 后不要有空格。

    ```Ruby
    # 差
    ! something

    # 好
    !something
    ```

* 范围字面量中间不要有空格。

    ```Ruby
    # 差
    1 .. 3
    'a' ... 'z'

    # 好
    1..3
    'a'...'z'
    ```

* 把 `when` 跟 `case` 缩排在同一层。我知道很多人不同意这一点，但这是《The Ruby Programming Language》及《Programming Ruby》所使用的风格。

    ```Ruby
    # 差
    case
      when song.name == 'Misty'
        puts 'Not again!'
      when song.duration > 120
        puts 'Too long!'
      when Time.now.hour > 21
        puts "It's too late"
      else
        song.play
    end

    # 好
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end
    ```

* 当赋值一个条件表达式的结果给一个变量时，保持分支的缩排在同一层。

    ```Ruby
    # 差 - 非常复杂
    kind = case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

    result = if some_cond
      calc_something
    else
      calc_something_else
    end

    # 好 - 结构很清晰
    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end

    result = if some_cond
               calc_something
             else
               calc_something_else
             end

    # 好 ( 避免代码让行宽过长 )
    kind =
      case year
      when 1850..1889 then 'Blues'
      when 1890..1909 then 'Ragtime'
      when 1910..1929 then 'New Orleans Jazz'
      when 1930..1939 then 'Swing'
      when 1940..1950 then 'Bebop'
      else 'Jazz'
      end

    result =
      if some_cond
        calc_something
      else
        calc_something_else
      end
    ```

* 在 `def` 之间使用空行，并且用空行把方法分成合乎逻辑的段落。

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

*  函数最后一个参数后面不要加逗号，特别是每个参数单独一样的时候

    ```Ruby
    # 差 - 虽然移动和增删参数的时候会很简单，但仍不推荐
    some_method(
                 size,
                 count,
                 color,
               )

    # 差
    some_method(size, count, color, )

    # 好
    some_method(size, count, color)
    ```


* 当给方法的参数赋默认值时，在 `=` 两边使用空格：

    ```Ruby
    # 差
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # 做一些任务...
    end

    # 好
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # 做一些任务...
    end
    ```

    虽然几本 Ruby 书建议用第一个风格，不过第二个风格在实践中更为常见（并可争议地可读性更高一点）。

* 避免在不需要的时候使用续行符 `\` 。实际编码时，除非用于连接字符串, 否则避免在任何情况下使用续行符。

    ```Ruby
    # 差
    result = 1 - \
             2

    # 好 (但是仍然丑到爆)
    result = 1 \
             - 2

    long_string = 'First part of the long string' \
                  ' and second part of the long string'
    ```

* 使用链式方法时风格统一。社区认为前引点号和末端点号都是好的风格。

   * （可选 A）和当一个链式方法调用需要在另一行继续时，将 `.` 放在第二行。

      ```Ruby
      # 差 - 为了理解第二行需要去查阅第一行
      one.two.three.
        four

      # 好 - 第二行在做什么立刻变得很清晰
      one.two.three
        .four
      ```

    * （可选 B）末尾用点号表示表达式没有结束

      ```Ruby
      # 差 - 需要读到第二行才能确定表达式没有结束
      one.two.three
        .four

      # 好 - 从第一行就可以立即明白表达式没有结束
      one.two.three.
        four
      ```

  两种方法各自优点参阅[这里](https://github.com/bbatsov/ruby-style-guide/pull/176)。

* 方法参数过长时，将它对齐排列在多行。当对齐的参数由于线宽不适合对齐时, 简单的在第一行之后缩进也是可以接受的。

    ```Ruby
    # 初始（行太长了）
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # 差（两倍缩排）
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # 好
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end

    # 好（普通缩排）
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end
  ```

* 构建数组时，如果跨行，应对齐。

    ```Ruby
    # 差 - 未对齐
    menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
      'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']

    # 好
    menu_item = [
      'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
      'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
    ]

    # 好
    menu_item =
      ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
       'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
    ```

* 大数字添加下划线来改善可读性。

    ```Ruby
    # 差 - 有几个零？
    num = 1000000

    # 好 - 更容易被人脑解析。
    num = 1_000_000
    ```

* 使用 RDoc 以及它的惯例来撰写 API 文档。注解区块及 `def` 不要用空行隔开。

* 每一行限制在 80 个字符内。

* 避免行尾空格。

* 文件以空白行结尾。

* 不要使用区块注释。它们不能由空白引导（`=begin` 必须顶头开始），并且不如普通注释容易辨认。

    ```Ruby
    # 差
    =begin
    一行注释
    另一行注释
    =end

    # 好
    # 一行注释
    # 另一行注释
    ```

## 语法

* 使用 `::` 引用常量（包括类和模块）和构造器 (比如 `Array()` 或者 `Nokogiri::HTML()`)。永远不要使用 `::` 来调用方法。

    ```Ruby
    # 差
    SomeClass::some_method
    some_object::some_method

    # 好
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    SomeModule::SomeClass()
    ```

* 使用 `def` 时，有参数时使用括号。方法不接受参数时，省略括号。

    ```Ruby
    # 差
    def some_method()
      # 此处省略方法体
    end

    # 好
    def some_method
      # 此处省略方法体
    end

    # 差
    def some_method_with_parameters param1, param2
      # 此处省略方法体
    end

    # 好
    def some_method_with_parameters(param1, param2)
      # 此处省略方法体
    end
    ```

* 定义参数默认值时候，有默认值的参数应该在参数列表的后面。如果有默认值的参数应该在参数列表的前面，Ruby调用时会发生不可预料的结果。

    ```Ruby
    # bad
    def some_method(a = 1, b = 2, c, d)
    puts "#{a}, #{b}, #{c}, #{d}"
    end

    some_method('w', 'x') # => '1, 2, w, x'
    some_method('w', 'x', 'y') # => 'w, 2, x, y'
    some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'

    # good
    def some_method(a, b, c = 1, d = 2)
      puts "#{a}, #{b}, #{c}, #{d}"
    end

    some_method('w', 'x') # => 'w, x, 1, 2'
    some_method('w', 'x', 'y') # => 'w, x, y, 2'
    some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'
    ```

* 避免使用并行赋值，当使用方法返回值，变量带星号，交换赋值时可以使用并行赋值。单独给变量赋值相比并行赋值更有可读性。

    ```Ruby
    # bad
    a, b, c, d = 'foo', 'bar', 'baz', 'foobar'

    # good
    a = 'foo'
    b = 'bar'
    c = 'baz'
    d = 'foobar'

    # good - swapping variable assignment
    # Swapping variable assignment is a special case because it will allow you to
    # swap the values that are assigned to each variable.
    a = 'foo'
    b = 'bar'

    a, b = b, a
    puts a # => 'bar'
    puts b # => 'foo'

    # good - method return
    def multi_return
      [1, 2]
    end

    first, second = multi_return

    # good - 带`*`其实就是把变量当成数组
    first, *list = [1,2,3,4]

    hello_array = *"Hello"

    a = *(1..3)
    ```

* 并行赋值时避免在最右边使用不必要的下划线变量。当左边的变量带`*`时才使用下划线变量。避免`*_`这个用法。

    ```Ruby
    # bad
    a, b, _ = *foo
    a, _, _ = *foo
    a, *_ = *foo

    # good
    *a, _ = *foo
    *a, b, _ = *foo
    a, = *foo
    a, b, = *foo
    a, _b = *foo
    a, _b, = *foo
    ```

* 永远不要使用 `for` ，除非你很清楚为什么。大部分情况应该使用迭代器。`for` 是由 `each` 实现的，所以你绕弯了。另外，`for` 没有包含一个新的作用域 (`each` 有），因此在它区块中定义的变量在外部是可见的。

    ```Ruby
    arr = [1, 2, 3]

    # 差
    for elem in arr do
      puts elem
    end

    # 注意 elem 可在 for 循环外部被访问
    elem #=> 3

    # 好
    arr.each { |elem| puts elem }

    # elem 不能够在 each 块外部被访问
    elem #=> NameError: undefined local variable or method `elem'
    ```

* 永远不要在多行的 `if/unless` 中使用 `then`。

    ```Ruby
    # 差
    if some_condition then
      # 此处省略语句体
    end

    # 好
    if some_condition
      # 此处省略语句体
    end
    ```

* 总是在多行的 `if/unless` 中把条件语句放在同一行。

    ```Ruby
    # 差
    if
      some_condition
      do_something
      do_something_else
    end

    # 好
    if some_condition
      do_something
      do_something_else
    end
    ```

* 三元操作符 `? : ` 比 `if/then/else/end` 结构更为常见也更精准。

    ```Ruby
    # 差
    result = if some_condition then something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```

* 三元操作符的每个分支只写一个表达式。即不要嵌套三元操作符。嵌套情况使用 `if/else` 结构。

    ```Ruby
    # 差
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # 好
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* 永远不要使用 `if x: ...`——它已经在 Ruby 1.9 被移除了。使用三元操作符。

    ```Ruby
    # 差
    result = if some_condition: something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```

* 利用 if 和 case 是表达式的特性。

    ```Ruby
    # 差
    if condition
      result = x
    else
      result = y
    end

    # 好
    result =
      if condition
        x
      else
        y
      end
      ```
* 单行情况使用 `when x then ...`。另一种语法 `when x: ...` 已经在 Ruby 1.9 被移除了。

* 永远不要使用 `when x; ...`。参考前一个规则。

* 使用 `!` 替代 `not`。

    ```Ruby
    # 差 - 因为操作符有优先级，需要用括号。
    x = (not something)

    # 好
    x = !something
    ```

* 避免使用 `!!`。

    ```Ruby
    # 差
    x = 'test'
    # 令人疑惑的空值检查
    if !!x
      # 此处省略语句体
    end

    x = false
    # 对布尔值进行双重否定是不必要的
    !!x # => false

    # 好
    x = 'test'
    unless x.nil?
      # 此处省略语句体
    end
    ```

* `and` 和 `or` 这两个关键字被禁止使用了。 总是使用 `&&` 和 `||` 来取代。

    ```Ruby
    # 差
    # 布尔表达式
    if some_condition and some_other_condition
      do_something
    end

    # 控制流程
    document.saved? or document.save!

    # 好
    # 布尔表达式
    if some_condition && some_other_condition
      do_something
    end

    # 控制流程
    document.saved? || document.save!
    ```


* 避免多行的 `? : `（三元操作符）；使用 `if/unless` 来取代。

* 单行主体用 `if/unless` 修饰符。另一个好的方法是使用 `&&/||` 控制流程。

    ```Ruby
    # 差
    if some_condition
      do_something
    end

    # 好
    do_something if some_condition

    # 另一个好方法
    some_condition && do_something
    ```

* 避免在多行区块后使用 `if` 或 `unless`。

    ```Ruby
    # 差
    10.times do
      # 此处省略多行语句体
    end if some_condition

    # 好
    if some_condition
      10.times do
        # 此处省略多行语句体
      end
    end
    ```

*  否定判断时，`unless`（或控制流程的 `||`）优于 `if`（或使用 `||` 控制流程）。

    ```Ruby
    # 差
    do_something if !some_condition

    # 差
    do_something if not some_condition

    # 好
    do_something unless some_condition

    # 另一个好方法
    some_condition || do_something
    ```

* 永远不要使用 `unless` 和 `else` 组合。改写成肯定条件。

    ```Ruby
    # 差
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # 好
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* 不要使用括号围绕 `if/unless/while` 的条件式。

    ```Ruby
    # 差
    if (x > 10)
      # 此处省略语句体
    end

    # 好
    if x > 10
      # 此处省略语句体
    end
    ```

* 在多行 `while/until` 中不要使用 `while/until condition do` 。

    ```Ruby
    # 差
    while x > 5 do
      # 此处省略语句体
    end

    until x > 5 do
      # 此处省略语句体
    end

    # 好
    while x > 5
      # 此处省略语句体
    end

    until x > 5
      # 此处省略语句体
    end
    ```

* 单行主体时尽量使用 `while/until` 修饰符。

    ```Ruby
    # 差
    while some_condition
      do_something
    end

    # 好
    do_something while some_condition
    ```

* 否定条件判断尽量使用 `until` 而不是 `while` 。

    ```Ruby
    # 差
    do_something while !some_condition

    # 好
    do_something until some_condition
    ```

* 无限循环用 `Kernel#loop`，不用 `while/until` 。

    ```Ruby
    # 差
    while true
      do_something
    end

    until false
      do_something
    end

    # 好
    loop do
      do_something
    end
    ```

* 循环后条件判断使用 `Kernel#loop` 和 `break`，而不是 `begin/end/until` 或者 `begin/end/while`。

   ```Ruby
   # 差
   begin
     puts val
     val += 1
   end while val < 0

   # 好
   loop do
     puts val
     val += 1
     break unless val < 0
   end
   ```

* 忽略围绕方法参数的括号，如内部 DSL (如：Rake, Rails, RSpec)，Ruby 中带有“关键字”状态的方法（如：`attr_reader`，`puts`）以及属性存取方法。所有其他的方法呼叫使用括号围绕参数。

    ```Ruby
    class Person
      attr_reader :name, :age

      # 忽略
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* 省略可选哈希参数的外部花括号。

    ```Ruby
    # 差
    user.set({ name: 'John', age: 45, permissions: { read: true } })

    # 好
    User.set(name: 'John', age: 45, permissions: { read: true })
    ```

* 如果方法是内部 DSL 的一部分，那么省略外层的花括号和圆括号。

    ```Ruby
    class Person < ActiveRecord::Base
      # 差
      validates(:name, { presence: true, length: { within: 1..10 } })

      # 好
      validates :name, presence: true, length: { within: 1..10 }
    end
    ```

* 如果方法调用不需要参数，那么省略圆括号。

    ```Ruby
    # 差
    Kernel.exit!()
    2.even?()
    fork()
    'test'.upcase()

    # 好
    Kernel.exit!
    2.even?
    fork
    'test'.upcase
    ```

* 当被调用的方法是只有一个操作的区块时，使用`Proc`。

    ```Ruby
    # 差
    names.map { |name| name.upcase }

    # 好
    names.map(&:upcase)
    ```

* 单行区块倾向使用 `{...}` 而不是 `do...end`。多行区块避免使用 `{...}`（多行串连总是​​丑陋）。在 `do...end` 、 “控制流程”及“方法定义”，永远使用 `do...end` （如 Rakefile 及某些 DSL）。串连时避免使用 `do...end`。

    ```Ruby
    names = %w(Bozhidar Steve Sarah)

    # 差
    names.each do |name|
      puts name
    end

    # 好
    names.each { |name| puts name }

    # 差
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # 好
    names.select { |name| name.start_with?('S') }.map(&:upcase)
    ```

    某些人会争论多行串连时，使用 `{...}` 看起来还可以，但他们应该扪心自问——这样代码真的可读吗？难道不能把区块内容取出来放到小巧的方法里吗？

* 显性使用区块参数而不是用创建区块字面量的方式传递参数给区块。此规则对性能有所影响，因为区块先被转化为 `Proc`。

    ```Ruby
    require 'tempfile'

    # 差
    def with_tmp_dir
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir) { |dir| yield dir }  # block just passes arguments
      end
    end

    # 好
    def with_tmp_dir(&block)
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir, &block)
      end
    end

    with_tmp_dir do |dir| # 使用上面的方法
      puts "dir is accessible as a parameter and pwd is set: #{dir}"
    end
    ```

* 避免在不需要控制流程的场合时使用 `return` 。

    ```Ruby
    # 差
    def some_method(some_arr)
      return some_arr.size
    end

    # 好
    def some_method(some_arr)
      some_arr.size
    end
    ```

* 避免在不需要的情况使用 `self` 。（只有在调用一个 self write 访问器时会需要用到。）

    ```Ruby
    # 差
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # 好
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* 避免局部变量 shadowing 外部方法，除非它们彼此相等。

    ```Ruby
    class Foo
      attr_accessor :options

      # 勉强可以
      def initialize(options)
        self.options = options
        # 此处 options 和 self.options 都是等价的
      end

      # 差
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # 好
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* 不要在条件表达式里使用 `=` （赋值）的返回值，除非条件表达式在圆括号内被赋值。这是一个相当流行的 Ruby 方言，有时被称为*safe assignment in condition*。

    ```Ruby
    # 差 (会触发一个警告提示)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # 好 (MRI 仍会抱怨, 但 RuboCop 不会)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # 好
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* 变量自赋值用简写方式。

    ```Ruby
    # 差
    x = x + y
    x = x * y
    x = x**y
    x = x / y
    x = x || y
    x = x && y

    # 好
    x += y
    x *= y
    x **= y
    x /= y
    x ||= y
    x &&= y
    ```

* 如果变量未被初始化过，用 `||=` 来初始化变量并赋值。

    ```Ruby
    # 差
    name = name ? name : 'Bozhidar'

    # 差
    name = 'Bozhidar' unless name

    # 好 仅在 name 为 nil 或 false 时，把名字设为 Bozhidar。
    name ||= 'Bozhidar'
    ```

* 不要使用 `||=` 来初始化布尔变量。 （想看看如果现在的值刚好是 `false` 时会发生什么。）

    ```Ruby
    # 差——会把 `enabled` 设成真，即便它本来是假。
    enabled ||= true

    # 好
    enabled = true if enabled.nil?
    ```

* 使用 &&= 可先检查是否存在变量，如果存在则做相应动作。这样就无需用 `if` 检查变量是否存在了。

    ```Ruby
    # 差
    if something
      something = something.downcase
    end

    # 差
    something = something ? something.downcase : nil

    # 可以
    something = something.downcase if something

    # 好
    something = something && something.downcase

    # 更好
    something &&= something.downcase
    ```

* 避免使用 case 语句等价操作符 `===` 。从名称可知，这是 `case` 台面下所用的操作符，在 `case` 语句外的场合使用，会产生难以理解的代码。

    ```Ruby
    # 差
    Array === something
    (1..100) === 7
    /something/ === some_string

    # 好
    something.is_a?(Array)
    (1..100).include?(7)
    some_string =~ /something/
    ```

* 能使用 `==` 时，就不要使用 `eql?`。提供更加严格比较的 `eql?` 在实践中极少使用。

  ```Ruby
  # 差，对于字符串，eql? 和 == 作用相同
  "ruby".eql? some_str

  # 好
  "ruby" == some_str
  1.0.eql? x # 当需要区别 Fixnum 1 与 Float 1.0 时，eql? 是具有意义的
  ```

* 避免使用 Perl 风格的特殊变量（像是 `$:`、`$;` 等）。它们看起来非常神秘，除非用于单行脚本，否则不鼓励使用。使用 `English` 库提供的友好别名。

    ```Ruby
    # 差
    $:.unshift File.dirname(__FILE__)

    # 好
    require 'English'
    $LOAD_PATH.unshift File.dirname(__FILE__)
    ```

* 永远不要在方法名与左括号之间放一个空格。

    ```Ruby
    # 差
    f (3 + 2) + 1

    # 好
    f(3 + 2) + 1
    ```

* 如果方法的第一个参数由左括号开始的，则此方法调用应该使用括号。举个例子，如 `f((3 + 2) + 1)`。

* 总是使用 `-w` 来执行 Ruby 解释器，如果你忘了某个上述的规则，它就会警告你！

* 不要在方法中嵌套定义方法，使用 lambda 代替。
  嵌套定义产生的方法，其实际上和外层方法处于同一作用域（比如类
  作用域）。此外，“嵌套方法”会在定义它的外层方法每次调用时被重新定义。

  ```Ruby
  # 差
  def foo(x)
    def bar(y)
      # 此处省略方法体
    end

    bar(x)
  end

  # 好，作用同前，但 bar 不会在 foo 每次调用时被重新定义
  def bar(y)
    # 此处省略方法体
  end

  def foo(x)
    bar(x)
  end

  # 好
  def foo(x)
    bar = ->(y) { ... }
    bar.call(x)
  end
  ```

* 用新的 lambda 字面语法定义单行区块，用 `lambda` 方法定义多行区块。

    ```Ruby
    # 差
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # 正确，但看着怪怪的
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # 好
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

* 当定义一个简短且没有参数的 lambda 时，省略参数的括号。

    ```Ruby
    # 差
    l = ->() { something }

    # 好
    l = -> { something }
    ```

* 用 `proc` 而不是 `Proc.new`。

    ```Ruby
    # 差
    p = Proc.new { |n| puts n }

    # 好
    p = proc { |n| puts n }
    ```

* 用 `proc.call()` 而不是 `proc[]` 或 `proc.()`。

    ```Ruby
    # 差 - 看上去像枚举访问
    l = ->(v) { puts v }
    l[1]

    # 也不好 - 不常用的语法
    l = ->(v) { puts v }
    l.(1)

    # 好
    l = ->(v) { puts v }
    l.call(1)
    ```

* 未使用的区块参数和局部变量使用 `_` 前缀或直接使用 `_`（虽然表意性差些） 。Ruby解释器和RuboCop都能辨认此规则，并会抑制相关地有变量未使用的警告。

    ```Ruby
    # 差
    result = hash.map { |k, v| v + 1 }

    def something(x)
      unused_var, used_var = something_else(x)
      # ...
    end

    # 好
    result = hash.map { |_k, v| v + 1 }

    def something(x)
      _unused_var, used_var = something_else(x)
      # ...
    end

    # 好
    result = hash.map { |_, v| v + 1 }

    def something(x)
      _, used_var = something_else(x)
      # ...
    end
    ```

* 使用 `$stdout/$stderr/$stdin` 而不是 `STDOUT/STDERR/STDIN`。`STDOUT/STDERR/STDIN` 是常量，虽然在 Ruby 中是可以给常量重新赋值的（可能是重定向到某个流），但解释器会警告。

* 使用 `warn` 而不是 `$stderr.puts`。除了更加清晰简洁，如果你需要的话，
  `warn` 还允许你压制（suppress）警告（通过 `-W0` 将警告级别设为 `0`）。

* 倾向使用 `sprintf` 和它的别名 `format` 而不是相当隐晦的 `String#%` 方法.

    ```Ruby
    # 差
    '%d %d' % [20, 10]
    # => '20 10'

    # 好
    sprintf('%d %d', 20, 10)
    # => '20 10'

    # 好
    sprintf('%{first} %{second}', first: 20, second: 10)
    # => '20 10'

    format('%d %d', 20, 10)
    # => '20 10'

    # 好
    format('%{first} %{second}', first: 20, second: 10)
    # => '20 10'
    ```

* 倾向使用 `Array#join` 而不是相当隐晦的使用字符串作参数的 `Array#*`。

    ```Ruby
    # 差
    %w(one two three) * ', '
    # => 'one, two, three'

    # 好
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* 当处理你希望将变量作为数组使用，但不确定它是不是数组时，
  使用 `[*var]` 或 `Array()` 而不是显式的 `Array` 检查。

    ```Ruby
    # 差
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # 好
    [*paths].each { |path| do_something(path) }

    # 好（而且更具易读性一点）
    Array(paths).each { |path| do_something(path) }
    ```

* 尽量使用范围或 `Comparable#between?` 来替换复杂的逻辑比较。

    ```Ruby
    # 差
    do_something if x >= 1000 && x < 2000

    # 好
    do_something if (1000...2000).include?(x)

    # 好
    do_something if x.between?(1000, 2000)

    ```

* 尽量用判断方法而不是使用 `==` 。比较数字除外。

    ```Ruby
    # 差
    if x % 2 == 0
    end

    if x % 2 == 1
    end

    if x == nil
    end

    # 好
    if x.even?
    end

    if x.odd?
    end

    if x.nil?
    end

    if x.zero?
    end

    if x == 0
    end
    ```

* 除非是布尔值，不用显示检查它是否不是 `nil` 。

    ```Ruby
    # 差
    do_something if !something.nil?
    do_something if something != nil

    # 好
    do_something if something

    # 好——检查的是布尔值
    def value_set?
      !@some_boolean.nil?
    end
    ```

* 避免使用 `BEGIN` 区块。

* 使用 `Kernel#at_exit` 。永远不要用 `END` 区块。

    ```Ruby
    # 差

    END { puts 'Goodbye!' }

    # 好

    at_exit { puts 'Goodbye!' }
    ```

* 避免使用 flip-flops 。

* 避免使用嵌套的条件来控制流程。

  当你可能断言不合法的数据，使用一个防御从句。一个防御从句是一个在函数顶部的条件声明，这样如果数据不合法就能尽快的跳出函数。


    ```Ruby
    # 差
    def compute_thing(thing)
      if thing[:foo]
        update_with_bar(thing)
        if thing[:foo][:bar]
          partial_compute(thing)
        else
          re_compute(thing)
        end
      end
    end

    # 好
    def compute_thing(thing)
      return unless thing[:foo]
      update_with_bar(thing[:foo])
      return re_compute(thing) unless thing[:foo][:bar]
      partial_compute(thing)
    end
    ```

  使用 `next` 而不是条件区块。

    ```Ruby
    # 差
    [0, 1, 2, 3].each do |item|
      if item > 1
        puts item
      end
    end

    # 好
    [0, 1, 2, 3].each do |item|
      next unless item > 1
      puts item
    end
    ```

* 倾向使用 `map` 而不是 `collect` ， `find` 而不是 `detect` ， `select` 而不是 `find_all` ， `reduce` 而不是 `inject` 以及 `size` 而不是 `length` 。这不是一个硬性要求；如果使用别名增加了可读性，使用它没关系。这些有押韵的方法名是从 Smalltalk 继承而来，在别的语言不通用。鼓励使用 `select` 而不是 `find_all` 的理由是它跟 `reject` 搭配起来是一目了然的。

* 不要用 `count` 代替 `size`。除了`Array`其它`Enumerable`对象都需要遍历整个集合才能得到大小。

    ```Ruby
    # 差
    some_hash.count

    # 好
    some_hash.size
    ```

* 倾向使用 `flat_map` 而不是 `map` + `flatten` 的组合。
  这并不适用于深度大于 2 的数组，举个例子，如果 `users.first.songs == ['a', ['b', 'c']]` ，则使用 `map + flatten` 的组合，而不是使用 `flat_map`。
  `flat_map` 将数组变平坦一个层级，而 `flatten` 会将整个数组变平坦。

    ```Ruby
    # 差
    all_songs = users.map(&:songs).flatten.uniq

    # 好
    all_songs = users.flat_map(&:songs).uniq
    ```

* 使用 `reverse_each` ，不用 `reverse.each` 。 `reverse_each` 不会重新分配新数组。

    ```Ruby
    # 差
    array.reverse.each { ... }

    # 好
    array.reverse_each { ... }
    ```

## 命名

> 程式设计的真正难题是替事物命名及使缓存失效。<br>
> ——Phil Karlton

* 标识符用英语命名。

    ```Ruby
    # 差 - 变量名用非ascii字符
    заплата = 1_000

    # 差 - 变量名用带有拉丁文的保加利亚语写成。
    zaplata = 1_000

    # 好
    salary = 1_000
    ```

* 符号、方法与变量使用蛇底式小写（snake_case）。

    ```Ruby
    # 差
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # 好
    :some_symbol

    def some_method
      ...
    end

    ```

* 类别与模组使用驼峰式大小写（CamelCase）。（保留类似 HTTP、RFC、XML 这种缩写为大写。）

    ```Ruby
    # 差
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # 好
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* 文件名用蛇底式小写，如 `hello_world.rb`。

* 目录名用蛇底式小写，如 `lib/hello_world/hello_world.rb`。

* 每个类/模块都在单独的文件，文件名用蛇底式小写而不是驼峰式大小写。

* 其他常数使用尖叫蛇底式大写（SCREAMING_SNAKE_CASE）。

    ```Ruby
    # 差
    SomeConst = 5

    # 好
    SOME_CONST = 5
    ```

* 判断式方法的名字（返回布尔值的方法）应以问号结尾。 (例如： `Array#empty?` )。不返回布尔值的方法不应用问号结尾。

* 有潜在**危险性**的方法，若此**危险**方法有安全版本存在时，应以安全版本名加上惊叹号结尾（例如：改动 `self` 或参数、 `exit!` （不会向 `exit` 那样运行 finalizers）, 等等方法）。

* 如果存在潜在的**危险**方法（即修改 `self` 或者参数的方法，不像 `exit` 那样运行 finalizers 的 `exit!`，等等）的安全版本，那么 *危险* 方法的名字应该以惊叹号结尾。

    ```Ruby
    # 不好 - 没有对应的安全方法
    class Person
      def update!
      end
    end

    # 好
    class Person
      def update
      end
    end

    # 好
    class Person
      def update!
      end

      def update
      end
    end
    ```

* 如果可能的话，根据危险方法（bang）来定义对应的安全方法（non-bang）。

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* 在简短区块中使用 `reduce` 时，把参数命名为 `|a, e|` (累加器（`accumulator`），元素（`element`）)

* 在定义二元操作符时，把参数命名为 `other` （`<<` 与 `[]` 是这条规则的例外，因为它们的语义不同）。

    ```Ruby
    def +(other)
      # 此处省略方法体
    end
    ```

## 注释

> 良好的代码是最佳的文档。当你要加一个注释时，扪心自问，“如何改善代码让它不需要注释？” 改善代码，再写相应文档使之更清楚。<br>
> ——Steve McConnell

* 编写让人一目了然的代码然后忽略这一节的其它部分。我是认真的！

* 用英语写注释。

* `#` 与注释文字之间使用一个空格。

* 注释超过一个单词时句首字母大写，并且在句子停顿或结尾处使用标点符号。句号后使用[一个空格](http://en.wikipedia.org/wiki/Sentence_spacing)。

* 避免肤浅的注释。

    ```Ruby
    # 差
    counter += 1 # 计数器加一
    ```

* 及时更新注释。过时的注解比没有注解还差。

> 好代码就像是好的笑话 - 它不需要解释。<br>
> ——Russ Olsen

* 避免替烂代码写注释。重构代码让它们看起来一目了然。（要嘛就做，要嘛不做——不要只是试试看。——Yoda）

### 注解

* 注解应该直接写在相关代码那行之前。

* 注解关键字后面，跟着一个冒号及空格，接着是描述问题的文字。

* 如果需要用多行来描述问题，后续行要放在 `#` 号后面并缩排两个空格。

    ```Ruby
    def bar
      # FIXME: 这在 v3.2.1 版本之后会异常崩溃，或许与
      #   BarBazUtil 的版本更新有关
      baz(:quux)
    end
    ```

* 在问题是显而易见的情况下，任何的文档会是多余的，注解应放在有问题的那行的最后，并且不需更多说明。这个用法应该是例外而不是规则。

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* 使用 `TODO` 标记以后应加入的特征与功能。

* 使用 `FIXME` 标记需要修复的代码。

* 使用 `OPTIMIZE` 标记可能影响性能的缓慢或效率低下的代码。

* 使用 `HACK` 标记代码异味，即那些应该被重构的可疑编码习惯。

* 使用 `REVIEW` 标记需要确认其编码意图是否正确的代码。举例来说：`REVIEW: 我们确定用户现在是这么做的吗？`

* 如果你觉得恰当的话，可以使用其他定制的注解关键字，但别忘记录在项目的 `README` 或类似文档中。

## 类与模块

* 在类别定义里使用一致的结构。

    ```Ruby
    class Person
      # 首先是 extend 与 include
      extend SomeModule
      include AnotherModule

      # 接着是常量
      SOME_CONSTANT = 20

      # 接下来是属性宏
      attr_reader :name

      # 跟着是其它的宏（如果有的话）
      validates :name

      # 公开的类别方法接在下一行
      def self.some_method
      end

      # 初始化方法在类方法和实例方法之间
      def initialize
      end

      # 跟着是公开的实例方法
      def some_method
      end

      # 受保护及私有的方法，一起放在接近结尾的地方
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* 如果某个类需要多行代码，则不要嵌套在其它类中。应将其独立写在文件中，存放以包含它的类的的名字命名的文件夹中。

    ```Ruby
    # 差

    # foo.rb
    class Foo
      class Bar
        # 30个方法
      end

      class Car
        # 20个方法
      end

      # 30个方法
    end

    # 好

    # foo.rb
    class Foo
      # 30个方法
    end

    # foo/bar.rb
    class Foo
      class Bar
        # 30个方法
      end
    end

    # foo/car.rb
    class Foo
      class Car
        # 20个方法
      end
    end
    ```

* 倾向使用模块，而不是只有类别方法的类。类别应该只在产生实例是合理的时候使用。

    ```Ruby
    # 差
    class SomeClass
      def self.some_method
        # 省略函数体
      end

      def self.some_other_method
      end
    end

    # 好
    module SomeClass
      module_function

      def some_method
        # 省略函数体
      end

      def some_other_method
      end
    end
    ```

* 当你想将模块的实例方法变成类别方法时，偏爱使用 `module_function` 胜过 `extend self`。

    ```Ruby
    # 差
    module Utilities
      extend self

      def parse_something(string)
        # 做一些事
      end

      def other_utility_method(number, string)
        # 做另一些事
      end
    end

    # 好
    module Utilities
      module_function

      def parse_something(string)
        # 做一些事
      end

      def other_utility_method(number, string)
        # 做另一些事
      end
    end
    ```

* 当设计类型层级时，确认它们符合 [里式替换原则](http://en.wikipedia.org/wiki/Liskov_substitution_principle)。

* 尽可能让你的类型越 [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) 越好。

* 永远替那些用以表示领域模型的类型提供一个适当的 `to_s` 方法。

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

* 使用 `attr` 系列函数来定义琐碎的访问器或修改器。

    ```Ruby
    # 差
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # 好
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* 不要使用 `attr`。使用 `attr_reader` 和 `attr_accessor`。

    ```Ruby
    # 差 - ruby 1.9 中就不推荐了
    attr :something, true
    attr :one, :two, :three # behaves as attr_reader

    # 好
    attr_accessor :something
    attr_reader :one, :two, :three
    ```

* 考虑使用 `Struct.new`，它替你定义了那些琐碎的访问器（accessors），构造器（constructor）以及比较操作符（comparison operators）。

    ```Ruby
    # 好
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # 更好
    Person = Struct.new(:first_name, :last_name) do
    end
    ```

* 不要扩展 `Struct.new`。它已经是个类了。对它扩展不但引入了无意义的类的层次也会在该文件多次被 require 时出现奇怪的错误。

  ```Ruby
  # 差
  class Person < Struct.new(:first_name, :last_name)
  end

  # 好
  Person = Struct.new(:first_name, :last_name)
  ```

* 考虑加入工厂方法以提供附加的有意义的方式来生成一个特定的类实例。

    ```Ruby
    class Person
      def self.create(options_hash)
        # 此处省略方法体
      end
    end
    ```

* 倾向使用[鸭子类型](http://en.wikipedia.org/wiki/Duck_typing) 而不是继承。

    ```Ruby
    ## 差
    class Animal
      # 抽象方法
      def speak
      end
    end

    # 继承超类
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # 继承超类
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    ## 好
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* 由于类变量在继承中产生的“讨厌的”行为，避免使用类变量（`@@`）。

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    如同你所看到的，在类型层级中的所有类其实都共享单独一个类变量。通常情况下应该倾向使用实例变量而不是类变量。

* 依据方法的目的用途指定适当的可见层级（`private`，`protected`）。别把所有方法都设为 `public`（方法的缺省值）。我们现在是在写“Ruby”，不是“Python”。

* 将 `public`，`protected`，`private` 和被应用的方法定义保持一致的缩排。在上下各留一行来强调这个可见性应用于之后的所有方法。

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

* 使用 `def self.method` 来定义方法。在代码重构时如果修改类名也无需重复多次修改了。

    ```Ruby
    class TestClass
      # 差
      def TestClass.some_method
        # 省略方法体
      end

      # 好
      def self.some_other_method
        # 省略方法体
      end

      # 当你需要定义很多个类时，另一种便捷的方式
      class << self
        def first_method
          # 省略方法体
        end

        def second_method_etc
          # 省略方法体
        end
      end
    end
    ```

* 在类的语法作用域中定义别名时优先使用 `alias`，因为 `alias` 有词法作用域，`self` 对象是源代码被读取时候的值（不是运行时候的 `self`），她清楚地告诉使用程序员，除非明确说明，否则方法别名的引用不会在运行时被改变或者被任何子类改变。

    ```Ruby
    class Westerner
      def first_name
        @names.first
      end

      alias given_name first_name
    end
    ```
  因为 `alias` 和 `def` 一样，是关键词，相比字符串和符号，优先使用裸字（bareword）；也就是说，这样使用 `alias foo bar`， 不是 `alias :foo :bar`。
  另外要知道 Ruby 怎么处理别名和继承，方法别名定义后，即使对应的方法在后面的代码中重新定义（即修改内部实现）后，
  别名仍然可以调用到修改前的方法。

    ```Ruby
    class Fugitive < Westerner
      def first_name
        'Nobody'
      end
    end
    ```
  这个例子中，`Fugitive#given_name` 仍然调用原来的 `Westerner#first_name`方法，而不是 `Fugitive#first_name` 方法。
  要想覆写 `Fugitive#given_name` 行为，必须在类中重新定义一次。

    ```Ruby
    class Fugitive < Westerner
      def first_name
        'Nobody'
      end

      alias given_name first_name
    end
    ```

* 运行时定义模块方法，类方法和单件类方法别名，总是使用 `alias_method`。因为在上述情况下 `alias` 会导致不可预期的结果

    ```Ruby
    module Mononymous
      def self.included(other)
        other.class_eval { alias_method :full_name, :given_name }
      end
    end

    class Sting < Westerner
      include Mononymous
    end
    ```

## 异常

* 使用 `fail` 方法来抛出异常。仅在捕捉到异常时使用 `raise` 来重新抛出异常（因为没有失败，所以只是显式地有目的性地抛出一个异常）

    ```Ruby
    begin
     fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* 如果 `fail/raise` 只有两个参数，无需显性指定 `RuntimeError`。

    ```Ruby
    # 差
    fail RuntimeError, 'message'

    # 好——默认就是 RuntimeError
    fail 'message'
    ```

* 将异常类和消息作为参数给 `fail/raise` ，而不是异常类的的实例。

    ```Ruby
    # 差
    fail SomeException.new('message')
    # 无法使用 `fail SomeException.new('message'), backtrace`.

    # 好
    fail SomeException, 'message'
    # 可以使用 `fail SomeException, 'message', backtrace`.
    ```

* 永远不要从 `ensure` 区块返回。如果你显式地从 `ensure` 区块中的一个方法返回，那么这方法会如同没有异常般的返回。实际上，异常会被默默丢掉。

    ```Ruby
    def foo
      fail
    ensure
      return 'very bad idea'
    end
    ```

* 尽可能使用隐式的 `begin` 区块。

    ```Ruby
    # 差
    def foo
      begin
        # 此处放主要逻辑
      rescue
        # 错误处理放在此处
      end
    end

    # 好
    def foo
      # 此处放主要逻辑
    rescue
      # 错误处理放在此处
    end
    ```

* 通过 *contingency* 方法 (一个由 Avdi Grimm 创造的词) 来减少 `begin` 区块的使用。

    ```Ruby
    # 差
    begin
      something_that_might_fail
    rescue IOError
      # 处理 IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # 处理 IOError
    end

    # 好
    def with_io_error_handling
       yield
    rescue IOError
      # 处理 IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* 不要抑制异常。

    ```Ruby
    begin
      # 这里发生了一个异常
    rescue SomeError
      # 拯救子句完全没有做事
    end

    # 差
    do_something rescue nil
    ```

* 避免使用 `rescue` 的修饰符形式。

    ```Ruby
    # 差 - 这里将会捕捉 StandardError 及其所有子孙类的异常。
    read_file rescue handle_error($!)

    # 好 - 这里只会捕获 Errno::ENOENT 及其所有子孙类的异常
    def foo
      read_file
    rescue Errno::ENOENT => ex
      handle_error(ex)
    end
    ```

* 不要为了控制流程而使用异常。

    ```Ruby
    # 差
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # 好
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* 避免救援 `Exception` 类别。这会把信号困住，并呼叫 `exit`，导致你需要 `kill -9` 进程。

    ```Ruby
    # 差
    begin
      # 呼叫 exit 及杀掉信号会被捕捉（除了 kill -9）
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # 异常处理
    end

    # 好
    begin
      # 一个不明确的 rescue 子句捕捉的是 StandardError，
      # 而不是许多编程者所设想的 Exception。
    rescue => e
      # 异常处理
    end

    # 也好
    begin
      # 这里发生一个异常
    rescue StandardError => e
      # 异常处理
    end
    ```

* 把较具体的异常放在救援链的较上层，不然它们永远不会被拯救。

    ```Ruby
    # 差
    begin
      # 一些代码
    rescue Exception => e
      # 一些处理
    rescue StandardError => e
      # 一些处理
    end

    # 好
    begin
      # 一些代码
    rescue StandardError => e
      # 一些处理
    rescue Exception => e
      # 一些处理
    end
    ```

* 在 `ensure` 区块中释放程序的外部资源。

    ```Ruby
    f = File.open('testfile')
    begin
      # .. 处理
    rescue
      # .. 错误处理
    ensure
      f.close if f
    end
    ```

* 调用资源获取方法时，尽可能使用具备自动清理功能的版本。

  ```Ruby
  # 差，需要显式关闭文件描述符
  f = File.open('testfile')
    # ...
  f.close

  # 好，文件描述符会被自动关闭
  File.open('testfile') do |f|
    # ...
  end
  ```

* 倾向使用标准库的异常类而不是导入新的异常类。

## 集合

* 倾向数组及哈希的字面表示法（除非你需要给构造器传入参数）。

    ```Ruby
    # 差
    arr = Array.new
    hash = Hash.new

    # 好
    arr = []
    hash = {}
    ```

* 创建元素为单词（没有空格和特殊符号）的数组时，用 `%w` 而不是 [] 方法。仅当数组有两个及以上元素时才应用这个规则。

    ```Ruby
    # 差
    STATES = ['draft', 'open', 'closed']

    # 好
    STATES = %w(draft open closed)
    ```

* 当你需要一个符号的数组（并且不需要保持 Ruby 1.9 兼容性）时，使用 `%i`。仅当数组只有两个及以上元素时才应用这个规则。

    ```Ruby
    # 差
    STATES = [:draft, :open, :closed]

    # 好
    STATES = %i(draft open closed)
    ```

* 避免在 `Array` 和 `Hash` 字面量中的最后一个元素后面使用逗号。特别是元素同一行的情况下

    ```Ruby
    # 差 - 方面移动、增加和修改参数，但仍不建议使用
    VALUES = [
              1001,
              2020,
              3333,
            ]

    # 差
    VALUES = [1001, 2020, 3333, ]

    # 好
    VALUES = [1001, 2020, 3333]
    ```

* 避免在数组中创造巨大的间隔。

    ```Ruby
    arr = []
    arr[100] = 1 # 现在你有一个很多 nil 的数组
    ```

* 当访问数组的首元素或尾元素时，尽量使用 `first` 或 `last`， 而非 `[0]` 或 `[-1]`。

* 当处理的元素没有重复时，使用 `Set` 来替代 `Array` 。 `Set` 实现了无序、无重复值的集合。 `Set` 的方法同数组类一样直观，还可像哈希中那样快速查找元素。

* 尽量用符号来取代字符串作为哈希的键。

    ```Ruby
    # 差
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* 避免使用可变的对象作为键值。

* 当哈希的键为符号时，使用 Ruby 1.9 的哈希的字面语法。

    ```Ruby
    # 差
    hash = { :one => 1, :two => 2, :three => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* 但哈希的键有符号也有字符串时，不使用Ruby 1.9的字面量语法。

    ```Ruby
    # 差
    { a: 1, 'b' => 2 }

    # 好
    { :a => 1, 'b' => 2 }
    ```

* 用 `Hash#key?` 不用 `Hash#has_key?`；用 `Hash#value?` 不用 `Hash#has_value?`。Matz [在此](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/43765)提及，考虑废弃较长形式的方法。

    ```Ruby
    # 差
    hash.has_key?(:test)
    hash.has_value?(value)

    # 好
    hash.key?(:test)
    hash.value?(value)
    ```

* 在处理应该存在的哈希键时，使用 `Hash#fetch`。

    ```Ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # 差 - 如果我们打错字的话，我们就无法找到对的英雄了
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermen] # => nil

    # 好 - fetch 会抛出一个 KeyError 来使这个问题明显
    heroes.fetch(:supermen)
    ```

* 在使用 `Hash#fetch` 时，使用第二个参数设置默认值。

   ```Ruby
   batman = { name: 'Bruce Wayne', is_evil: false }

   # 差 - 如果我们仅仅使用 || 操作符，那么当值为假时，我们不会得到预期的结果
   batman[:is_evil] || true # => true

   # 好 - fetch 在遇到假值时依然正确
   batman.fetch(:is_evil, true) # => false
   ```

* 如果求值的代码有副作用或者开销大，尽量用 `Hash#fetch` 加区块而不是直接设定默认值。

   ```Ruby
   batman = { name: 'Bruce Wayne' }

   # 差 - 默认值是立即求值
   batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers 需要复杂的计算

   # 好 - 区块是惰性求值，只有当 KeyError 异常时才执行
   batman.fetch(:powers) { obtain_batman_powers }
   ```

* 当需要从哈希中同时获取多个键值时，使用 `Hash#values_at`。

  ```Ruby
  # 差
  email = data['email']
  username = data['nickname']

  # 好
  email, username = data.values_at('email', 'nickname')
  ```

* Ruby 1.9 的哈希是有序的，利用这个特性。

* 在遍历一个集合时，不要改动它。

* 当访问集合中的元素时，避免通过 `[n]` 直接访问，尽量使用提供的方法。这样可以防止你对 `nil` 调用 `[]`。

  ```Ruby
  # 差
  Regexp.last_match[1]

  # 好
  Regexp.last_match(1)
  ```

* 为集合提供存取器时，在访问元素之前采用一种替代的形式，从而防止用户访问的下标是 `nil`。

  ```Ruby
  # 差
  def awesome_things
    @awesome_things
  end

  # 好
  def awesome_things(index = nil)
    if index && @awesome_things
      @awesome_things[index]
    else
      @awesome_things
    end
  end
  ```

## 字符串

* 尽量使用字符串插值（interpolation），而不是字符串连接（concatenation）。

    ```Ruby
    # 差
    email_with_name = user.name + ' <' + user.email + '>'

    # 好
    email_with_name = "#{user.name} <#{user.email}>"

    # 好
    email_with_name = format('%s <%s>', user.name, user.email)
    ```

* 对于插值表达式, 括号内不应有留白（padded-spacing）。

    ```Ruby
    # 差
    "From: #{ user.first_name }, #{ user.last_name }"

    # 好
    "From: #{user.first_name}, #{user.last_name}"
    ```

* 选定一个字符串字面量创建的风格。Ruby 社区认可两种分割，默认用单引号（风格 A）和默认用双引号（风格 B）

  * **（风格 A）**当你不需要插入特殊符号如 `\t`, `\n`, `'`, 等等时，尽量使用单引号的字符串。

    ```Ruby
    # 差
    name = "Bozhidar"

    # 好
    name = 'Bozhidar'
    ```

  * **（风格 B）** 用双引号。除非字符串中含有双引号，或者含有你希望抑制的逃逸字符。

    ```Ruby
    # 差
    name = 'Bozhidar'

    # 好
    name = "Bozhidar"
    ```

  有争议的是，第二种风格在 Ruby 社区里更受欢迎一些。但是本指南中字符串采用第一种风格。

* 不要用 `?x`。从 Ruby 1.9 开始， `?x` 和 `'x'` 是等价的（只包括一个字符的字符串）。

    ```Ruby
    # 差
    char = ?c

    # 好
    char = 'c'
    ```

* 别忘了使用 `{}` 来围绕被插入字符串的实例与全局变量。

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # 差 - 有效，但难看
      def to_s
        "#@first_name #@last_name"
      end

      # 好
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # 差
    puts "$global = #$global"

    # 好
    puts "$global = #{$global}"
    ```

* 字符串插值不要用 `Object#to_s` 。Ruby 默认会调用该方法。

    ```Ruby
    # 差
    message = "This is the #{result.to_s}."

    # 好
    message = "This is the #{result}."
    ```

* 当你需要建构庞大的数据块（chunk）时，避免使用 `String#+` 。
  使用 `String#<<` 来替代。`<<` 就地改变字符串实例，因此比 `String#+` 来得快。`String#+` 创造了一堆新的字符串对象。

    ```Ruby
    # 好也比较快
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

* 当你可以选择更快速、更专门的替代方法时，不要使用 `String#gsub`。

    ```Ruby
    url = 'http://example.com'
    str = 'lisp-case-rules'

    # 差
    url.gsub("http://", "https://")
    str.gsub("-", "_")

    # 好
    url.sub("http://", "https://")
    str.tr("-", "_")
    ```

* heredocs 中的多行文字会保留前缀空白。因此做好如何缩进的规划。

    ```Ruby
    code = <<-END.gsub(/^\s+\|/, '')
      |def test
      |  some_method
      |  other_method
      |end
    END
    #=> "def\n  some_method\n  \nother_method\nend"
    ```

## 正则表达式

> 有些人在面对问题时，不经大脑便认为，「我知道，这里该用正则表达式」。现在他要面对两个问题了。<br>
> ——Jamie Zawinski

* 如果只需要在字符串中简单的搜索文字，不要使用正则表达式：`string['text']`。

* 针对简单的字符串查询，可以直接在字符串索引中直接使用正则表达式。

    ```Ruby
    match = string[/regexp/]             # 获得匹配正则表达式的内容
    first_group = string[/text(grp)/, 1] # 获得分组的内容
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```
* 当你不需要替分组结果时，使用非捕获组。

    ```Ruby
    /(first|second)/   # 差
    /(?:first|second)/ # 好
    ```

* 不要使用 Perl 遗风的变量来表示捕获的正则分组（如 `$1` 、 `$2` 等），它们看起来神神秘秘的。使用 `Regexp.last_match(n)`。

    ```Ruby
    /(regexp)/ =~ string
    ...

    # 差
    process $1

    # 好
    process Regexp.last_match(1)
    ```

* 避免使用数字来获取分组。因为很难明白它们代表的意思。应该使用命名分组来替代。

    ```Ruby
    # 差
    /(regexp)/ =~ string
    ...
    process Regexp.last_match(1)

    # 好
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* 字符类别只有几个你需要关心的特殊字符：`^`、`-`、`\`、`]`，所以你不用转义 `[]` 中的 `.` 或中括号。

* 小心使用 `^` 与 `$` ，它们匹配的是一行的开始与结束，不是字符串的开始与结束。如果你想要匹配整个字符串，使用 `\A` 与 `\z`。(译注：`\Z` 实为 `/\n?\z/`，使用 `\z` 才能匹配到有含新行的字符串的结束)

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # 匹配
    string[/\Ausername\z/] # 不匹配
    ```
* 针对复杂的正则表达式，使用 `x` 修饰符。可提高可读性并可以加入有用的注释。只是要注意空白字符会被忽略。

    ```Ruby
    regexp = /
      start         # 一些文本
      \s            # 空白字符
      (group)       # 第一组
      (?:alt1|alt2) # 一些替代方案
      end
    /x
    ```

* 针对复杂的替换，`sub` 或 `gsub` 可以与区块或哈希结合使用。

## 百分比字面

* 需要插值与嵌入双引号的单行字符串使用 `%()` （是 `%Q` 的简写）。多行字符串，最好用 heredocs 。

    ```Ruby
    # 差（不需要插值）
    %(<div class="text">Some text</div>)
    # 应该使用 '<div class="text">Some text</div>'

    # 差（没有双引号）
    %(This is #{quality} style)
    # 应该使用 "This is #{quality} style"

    # 差（多行）
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # 应该是一个 heredoc

    # 好（需要插值、有双引号以及单行）
    %(<tr><td class="name">#{name}</td>)
    ```

* 没有 `'` 和 `"` 的字符串不要使用 `%q`。除非需要插值，否则普通字符串可读性更好。

    ```Ruby
    # 差
    name = %q(Bruce Wayne)
    time = %q(8 o'clock)
    question = %q("What did you say?")

    # 好
    name = 'Bruce Wayne'
    time = "8 o'clock"
    question = '"What did you say?"'
    ```

* 只有正则表达式要匹配多于一个的 `/` 字元时，使用 `%r`。

    ```Ruby
    # 差
    %r{\s+}

    # 好
    %r{^/(.*)$}
    %r{^/blog/2011/(.*)$}
    ```

* 除非调用的命令中用到了反引号（这种情况不常见），否则不要用 `%x` 。

    ```Ruby
    # 差
    date = %x(date)

    # 好
    date = `date`
    echo = %x(echo `date`)
    ```

* 不要用 `%s` 。社区倾向使用 `:"some string"` 来创建含有空白的符号。

* 用 `%` 表示字面量时使用 `()`，`%r` 除外。因为 `(` 在正则中比较常用。

    ```Ruby
    # 差
    %w[one two three]
    %q{"Test's king!", John said.}

    # 好
    %w(one tho three)
    %q("Test's king!", John said.)
    ```

## 元编程

* 避免无谓的元编程。

* 写一个函数库时不要使核心类混乱（不要使用 monkey patch）。

* 倾向使用区块形式的 `class_eval` 而不是字符串插值（string-interpolated）的形式。
  - 当你使用字符串插值形式时，总是提供 `__FILE__` 及 `__LINE__`，使你的 backtrace 看起来有意义：

    ```ruby
    class_eval "def use_relative_model_naming?; true; end", __FILE__, __LINE__
    ```

  - 倾向使用 `define_method` 而不是 `class_eval{ def ... }`

* 当使用 `class_eval` （或其它的 `eval`）搭配字符串插值时，添加一个注解区块，来演示如果做了插值的样子（我从 Rails 代码学来的一个实践）：

    ```ruby
    # activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)      # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block) #   to_str.capitalize(*args, &block)
          end                                      # end

          def #{unsafe_method}!(*args) # def capitalize!(*args)
            @dirty = true              #   @dirty = true
            super                      #   super
          end                          # end
        EOT
      end
    end
    ```

* 元编程避免使用 `method_missing`。它会让 backtraces 变得很凌乱；行为不被罗列在 `#methods` 里；拼错的方法调用可能默默地工作（`nukes.launch_state = false`）。考虑使用委托、代理、或是 `define_method` 来取代。如果你必须使用 `method_missing`：
  - 确保 [也定义了 `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - 仅捕捉字首定义良好的方法，像是 `find_by_*`——让你的代码愈肯定（assertive） 愈好。
  - 在语句的最后调用 `super`
  - 委托到确定的、非魔法方法中:

    ```ruby
    # 差
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # 好
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # 最好的方式，可能是每个可找到的属性被声明后，使用 define_method。
    ```

* 相比较 `send`，倾向优先使用 `public_send`，因为后者会无视 `private` / `protected` 的可见性。

    ```Ruby
    # We have  an ActiveModel Organization that includes concern Activatable
    module Activatable
      extend ActiveSupport::Concern

      included do
        before_create :create_token
      end

      private

      def reset_token
        ...
      end

      def create_token
        ...
      end

      def activate!
        ...
      end
    end

    class Organization < ActiveRecord::Base
      include Activatable
    end

    linux_organization = Organization.find(...)
    # BAD - violates privacy
    linux_organization.send(:reset_token)
    # GOOD - should throw an exception
    linux_organization.public_send(:reset_token)
    ```

* 相比 `send`，优先使用 `__send__`，因为调用对象可能已经存在 `send` 方法。

    ```Ruby
    require 'socket'

    u1 = UDPSocket.new
    u1.bind('127.0.0.1', 4913)
    u2 = UDPSocket.new
    u2.connect('127.0.0.1', 4913)
    # 不会发送一个消息给接受者
    # 实际上通过 UDP socket 发送一个消息
    u2.send :sleep, 0

    # 发送一个消息给接受者，相当于 u2.sleep(0)
    u2.__send__ :sleep, 0
    ```

## 其它

* `ruby -w` 写安全的代码。

* 避免使用哈希作为可选参数。这个方法是不是做太多事了？（对象初始器是本规则的例外）。

* 避免方法长于 10 行代码（LOC）。理想上，大部分的方法会小于 5 行。空行不算进 LOC 里。

* 避免参数列表长于三或四个参数。

* 如果你真的需要“全局”方法，把它们加到 Kernel 并设为私有的。

* 使用模块变量代替全局变量。

    ```Ruby
    # 差
    $foo_bar = 1

    # 好
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* 使用 `OptionParser` 来解析复杂的命令行选项及 `ruby -s` 来处理琐碎的命令行选项。

* 使用 `Time.now` 而不是 `Time.new` 来获取系统时间。

* 用函数式的方法编程，在有意义的情况下避免赋值 (mutation)。

* 不要改变参数，除非那是方法的目的。

* 避免超过三层的区块嵌套。

* 保持一致性。在理想的世界里，遵循这些准则。

* 使用常识。

## 工具

以下是一些工具，让你自动检查 Ruby 代码是否符合本指南。

### RuboCop

[RuboCop](https://github.com/bbatsov/rubocop) 是一个基于本指南的 Ruby 代码风格检查工具。 RuboCop 涵盖了本指南相当大的部分，支持 MRI 1.9 和 MRI 2.0，而且与 Emacs 整合良好。

### RubyMine

[RubyMine](http://www.jetbrains.com/ruby/) 的代码检查[部分基于](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)本指南。

# 贡献

在本指南所写的每条规则都不是定案。这只是我渴望想与同样对 Ruby 编程风格有兴趣的大家一起工作，以致于最终我们可以替整个 Ruby 社区创造一个有益的资源。

欢迎 open tickets 或 push 一个带有改进的更新请求。在此提前感谢你的帮助！

## 如何贡献？
很简单，只需要参考 [贡献准则](https://github.com/bbatsov/ruby-style-guide/blob/master/CONTRIBUTING.md)。

# 授权

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.zh)

# 口耳相传

一份社区驱动的风格指南，如果没多少人知道，对一个社区来说就没有多少用处。微博转发这份指南，分享给你的朋友或同事。我们得到的每个评价、建议或意见都可以让这份指南变得更好一点。而我们想要拥有的是最好的指南，不是吗？

共勉之，<br>
[Bozhidar](https://twitter.com/bbatsov)
