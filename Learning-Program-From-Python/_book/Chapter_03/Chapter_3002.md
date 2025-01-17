# 3.2 参数传递

### 1．基本传参

把数据用参数的形式输入到函数，被称为参数传递。如果只有一个参数，那么参数传递会变得很简单，只需把函数调用时输入的唯一一个数据对应为这个参数就可以了。如果有多个参数，那么在调用函数时，Python会根据位置来确认数据对应哪个参数，例如：

------------------------------------------------------------------------

    def print_arguments(a, b, c):
        """print arguments according to their sequence"""
        print(a, b, c)
        
    print_arguments(1, 3, 5)  # 打印1、3、5
    print_arguments(5, 3, 1)  # 打印5、3、1
    print_arguments(3, 5, 1)  # 打印3、5、1

------------------------------------------------------------------------

在程序的三次调用中，Python都是通过位置来确定实参与形参的对应关系的。

如果觉得位置传参比较死板，那么可以用关键字（Keyword）的方式来传递参数。在定义函数时，我们给了形参一个符号标记，即参数名。关键字传递是根据参数名来让数据与符号对应上。因此，如果在调用时使用关键字传递，那么不用遵守位置的对应关系。沿用上面的函数定义，改用参数传递的方式：

------------------------------------------------------------------------

    print_arguments(c=5,b=3,a=1)   # 打印1、3、5

------------------------------------------------------------------------

从结果可以看出，Python不再使用位置来对应参数，而是利用了参数的名字来对应参数和数据。

位置传递与关键字传递可以混合使用，即一部分的参数传递根据位置，另一部分根据参数名。在调用函数时，所有的位置参数都要出现在关键字参数之前。因此，你可以用如下方式来调用：

------------------------------------------------------------------------

    print_arguments(1, c=5,b=3) # 打印1、3、5

------------------------------------------------------------------------

但如果把位置参数1放在关键字参数c=5的后面，则Python将报错：

------------------------------------------------------------------------

    print_arguemnts(c=5, 1, b=3) # 程序报错

------------------------------------------------------------------------

位置传递和关键字传递让数据与形参对应起来，因此数据的个数与形参的个数应该相同。但在函数定义时，我们可以设置某些形参的默认值。如果我们在调用时不提供这些形参的具体数据，那么它们将采用定义时的默认值，比如：

------------------------------------------------------------------------

    def f(a,b,c=10):
        return a+b+c
        
    print(f(3,2,1))  # 参数c取传入的1。结果打印6
    print(f(3,2))    # 参数c取默认值10。结果打印15

------------------------------------------------------------------------

第一次调用函数时输入了3个数据，正好对应三个形参，因此形参c对应的数据是1。第二次调用函数时，我们只提供了3和2两个数据。函数根据位置，把3和2对应成形参a和b。到了形参c时，已经没有多余的数据，所以c将采用其默认值10。

### 2．包裹传参

以上传递参数的方式，都要求在定义函数时说明参数的个数。但有时在定义函数时，我们并不知道参数的个数。其原因有很多，有时是确实不知道参数的个数，需要在程序运行时才能知道。有时是希望函数定义的更加松散，以便于函数能运用于不同形式的调用。这时候，用包裹（packing）传参的方式来进行参数传递会非常有用。

和之前一样，包裹传参也有位置和关键字两种形式。下面是包裹位置传参的例子：

------------------------------------------------------------------------

    def package_position(*all_arguments):
        print(type(all_arguments))
        print(all_arguments)
        
    package_position(1,4,6)
    package_position(5,6,7,1,2,3)

------------------------------------------------------------------------

两次调用，尽管参数个数不同，但都基于同一个package\_position()定义。在调用package\_position()时，所有的数据都根据先后顺序，收集到一个元组。在函数内部，我们可以通过元组来读取传入的数据。这就是包裹位置传参。为了提醒Python参数all\_arguments是包裹位置传递所用的元组名，我们在定义package\_position()时要在元组名all\_arguments前加 \* 号。

我们再来看看包裹关键字传递的例子。这一参数传递方法把传入的数据收集为一个词典：

------------------------------------------------------------------------

    def package_keyword(**all_arguments):
        print(type(all_arguments))
        print(all_arguments)
        
    package_keyword(a=1,b=9)
    package_keyword(m=2,n=1,c=11)

------------------------------------------------------------------------

与上面一个例子类似，当函数调用时，所有参数会收集到一个数据容器里。只不过，在包裹关键字传递的时候，数据容器不再是一个元组，而是一个字典。每个关键字形式的参数调用，都会成为字典的一个元素。参数名成为元素的键，而数据成为元素的值。字典all\_arguments收集了所有的参数，把数据传递给函数使用。为了提醒，参数all\_arguments是包裹关键字传递所用的字典，因此在all\_arguments前加 \*\* 。

包裹位置传参和包裹关键字传参还可以混合使用，比如：

------------------------------------------------------------------------

    def package_mix(*positions, **keywords):
        print(positions)
        print(keywords)
        
    package_mix(1, 2, 3, a=7, b=8, c=9)

------------------------------------------------------------------------

还可以更进一步，把包裹传参和基本传参混合使用。它们出现的先后顺序是：位置→关键字→包裹位置→包裹关键字。有了包裹传递，我们在定义函数时可以更灵活地表示数据。

### 3．解包裹

除了用于函数定义， \* 和 \*\* 还可用于函数调用。这时候，两者是为了实现一种叫作解包裹（unpacking）的语法。解包裹允许我们把一个数据容器传递给函数，再自动地分解为各个参数。需要注意的是，包裹传参和解包裹并不是相反操作，而是两个相对独立的功能。下面是解包裹的一个例子：

------------------------------------------------------------------------

    def unpackage(a,b,c):
        print(a,b,c)
        
    args = (1,3,4)
    unpackage(*args)    # 结果为1 3 4

------------------------------------------------------------------------

在这个例子中，unpackage()使用了基本的传参方法。函数有三个参数，按照位置传递。但在调用该函数时，我们用了解包裹的方式。可以看到，我们调用函数时传递的是一个元组。按照基本传参的方式，一个元组是无法和三个参数对应上的。但我们通过在args前加上 \* 符号，来提醒Python，我想把元组拆成三个元素，每一个元素对应函数的一个位置参数。于是，元组的三个元素分别赋予了三个参数。

相应的，词典也可用于解包裹，使用相同的unpackage()定义：

------------------------------------------------------------------------

    args = {"a":1,"b":2,"c":3}
    unpackage(**args)      # 打印1、2、3

------------------------------------------------------------------------

然后在传递词典args时，让词典的每个键值对作为一个关键字传递给函数unpackage()。

解包裹用于函数调用。在调用函数时，几种参数的传递方式也可以混合。依然是相同的基本原则：位置→关键字→位置解包裹→关键字解包裹。
