### Chrome 调试小技巧
---
1. $0 是当前我们选中的html节点的引用。$1 是我们上一次选择的节点的引用，$2 是在那之前选择的节点的引用，等等。一直到 $4。
2. 在你还没有在app中定义 $变量的情况下(例如 jQuery)， $在console中是冗长的函数document.querySelector的一个别名。$$$，因为它不仅仅执行document.QuerySelectorAll并且返回的是一个节点的数组，而不是一个Node list。`Array.from(document.querySelectorAll('div')) === $$('div')`
3. $_变量是上次执行的结果的引用。
4. **在console中引入和把玩一些npm库。直接运行例如 $i('lodash') 或者 $i('moment') 然后在几秒钟之后，你就可以获取到lodash / momentjs了。**
5.  **copy()** 可以拿到任何需要的资源

        copy($0)
        粘贴输出> XXX...得到你要的当前选中节点html

6.  Store as global  右击输出的值保存为全局变量
7.  Copy HTML (最快的方式)，选中节点右击选中copy
8.  值如果是假的会打印自定义提示信息`console.assert(value,"value is empty!!!") `
9.  `console.table($$('li') ,['textContent','className']);`**将数据以表格的形式展示**
10. **打印节点关联的js对象 使用 console.dir()**
11. **选中节点 按住 h 便可以隐藏节点**
12. **对节点进行拖动 & 放置 元素 ,或者使用control!(按钮),在DOM结构中往上一点或者往下一点，而不是拖动和放置，你同样可以使用**[ctrl] + [⬆] / [ctrl] + [⬇] *
13. **调试面板功能Command 菜单选择** `Ctrl + Shift + P`,截全面屏使用 Capture full size screenshot 功能，快速切换面板位置输入layout(使用快捷键 Ctrl + Shift +D)，也可以切换主题theme
14. **异步**
    
    在控制面板的输出使用promise要比在源码中简单，不需要写async关键字，直接写await就行。

            response=await fetch('http://....')
            json= await response.json()
15. 监控执行时间,传入不同的标签

    - console.time() — 开启一个计时器
    - console.timeEnd() — 结束计时并且将结果在 console 中打印出来

16. **当你把鼠标放在样式选择器的选择区域的最后时，你会看到几个按钮，他们可以让你快速的使用 Color 和 Shadow 编辑器添加 CSS 属性：**

    - text-shadow

    - box-shadow

    - color

    - background-color

17. **调试** 参考 [https://juejin.im/post/5c16d943518825566d2365f3](https://juejin.im/post/5c16d943518825566d2365f3 "参考")

-  **条件断点**

    有时候你设置的断点被执行太多次了：想想看，有一个对 200 个元素的循环，但是你只对第110 次循环的结果感兴趣，或者只对一些满足其他的特殊情况的结果感兴趣。

    - 右击行号并且选择 Add conditional breakpoint...(添加条件断) 点的选项

    - 或者右击一个已经设置的断点并且选择 Edit breakpoint(编辑断点)

    - 然后输入一个执行结果为 true 或者 false 的表达式（它的值并不需要明确的为 true 或者 false 尽管那个弹出框的描述是这样说的）。

- **调试循环时特别有用**

        let sth=0 
        //这里插入console.time()
        const objects=Array.from(Array(15),(v,i)=>({value:v}))
        //这里插入console.table(objects)
        objects.forEach(({value},i)=>{
            sth=sth+value*i   //这里插入 console.log(value,i)
        })
        //这里插入 console.timeEnd()
    设置断点后右击编辑断点——调试代码段前后使用console.time() && console.timeEnd()包裹，然后中间用console.table()打印信息

18. queryObjects function 对象查询方法

        class Person{
            constructor(name,role){
                this.name=name;
                this.role=role;
                }
            }

        const john = new Person('John','dad')

    控制台queryObjects(Person)得到类的实例

19. monitor functions 镜像函数

        class Person {
            constructor(name, role) {
                this.name = name;
                this.role = role;
        }

        greet() {
            return this.getMessage('greeting');
        }
        getMessage(type) {
            if (type === 'greeting') {
                    return `Hello, I'm ${this.name}!`;
                }
            }
        }
    控制台执行`const john=new Person('John');
        monitor(john.getMessage);
        john.greet();`

    monitor 是 DevTools 的一个方法, 它能够让你 spy(潜入) 到任何 function calls(方法的调用) 中：每当一个 spied 被潜入 的方法运行的时候，console 控制台 会把它的实例打印出来，包含函数名以及调用它的参数。

20. monitorEvents($0,click)  监控当前节点点击事件后触发
21. **展开所有子节点的方法，选中节点右击选中expand recursively**
22. **DOM节点监听**
    
    右击选中节点，选中Break on,之后如下：

    - 选择 subtree modifications :监听任何它内部的节点被 移除 或者 添加的事件

    - 选择 attribute modifications :监听任何当前选中的节点被 添加，移除 或者 被修改值的事件

    - 选择 node removal :监听被选中的元素被 移除 的事件

23. **将打印的变量包含在一个大括号内增加输出的易读性**

        console.log({xxxx,xxxx,xxxx})// {变量名:值，...}
        console.table({xxxx,xxx}) //同样
24. esc 点击调出drawer,更多功能可进行探索,参考 [https://juejin.im/post/5c1b4df45188255e9b61fde5](https://juejin.im/post/5c1b4df45188255e9b61fde5 "参考")

    - 如果你正在你的应用中使用一些获取位置信息的 API 而且想要测试一下它,位于 Drawer 的 Sensors(传感器) 面板可以让你模拟特定的位置。可以从预定义的位置中进行选择，添加自己的位置，或者只需手动键入纬度/经度。选定的值将被navigator.geolocation.watchPosition（或 .getCurrentPosition ）报告。如果你的应用使用加速计，传感器面板也可以模拟你设备在3D空间中的位置！

    - 你可以使用 Drawer 的 Network conditions 面板模拟特定的网络行为：模拟互联网为典型的3G网络甚至离线！ 这有助于了解页面资源的大小。
       
    - 当我们主要专注于 Elements 面板时，有时我也想看到源代码。就像 drawer console 一样，你可以在 drawer 中显示 Source。


25. **控制台保存代码段 Sources -> Snippets -> 编辑代码 -> 选中右击运行** **,快捷打开方式 Ctrl + P  ->!加上你代码段的名字**
26. Drawer 中的Changes 可以在你编辑Sources中的文件时显示修改前后的对比
27. Drawer 中的Coverage 可以显示你当前文件用绿色的线条标记运行和用红色的线条标记未运行，可以跟踪当前加载的 JS 和 CSS 文件的哪些行正在被执行，并显示未使用字节的百分比。
28. **直接在回调中使用console.log**

        getMyLocation(v=>console.log(v))

        //改成如下写法
        getMyLocation(console.log)

29. Console 面板中引入了一个非常漂亮的附加功能，这是一个名为 **Live expression** 的工具只需按下 "眼睛" 符号，你就可以在那里定义任何 JavaScript 表达式。 它会不断更新，所以表达的结果将永远，存在 




    











   

