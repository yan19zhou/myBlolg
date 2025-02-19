### DOM:

​	1.DOM(文档对象模型)是针对html和xml的一个API，通过DOM去改变文档；DOM可以理解为一个访问或操作HTML各种标签的实现标准
​	2.每一段HTML标记都可以用相应的节点来表示，一共有12种节点类型，都继承自node类型；(html元素有html元素节点，注释有注释节点，文档有文档节点)

​	node类型(基类型，所有节点都继承自他，所以所有节点都有一个共同的属性和方法)：
​	//常用节点名：nodeName，nodeValue
​		对于元素：nodeName为标签名，nodeValue为null；
​		对于文本：nodeName为“#Text”，nodeValue为文本的值；
​	//childNodes：
​		1.保存了这个节点所有直接子元素；
​		2.比较有用的方法和属性：
​			hasChildNodes()，如果包含子节点则返回true；
​			ownerDocument，返回文档的节点引用，(html里面也就是document对象)；
​	//常用的方法：
​		1.appendChild()方法可以在节点的childNodes的末尾增加一个节点，值得注意的是如果这个节点是已经存在在文档中的，那么便会删除原节点，
​		感觉上就像是移动节点一样。
​		2.insertBefore()方法接受两个参数，一个是插入的节点，另外一个是参照的节点。如果第二个参数为null，则insertBefore和appendChild效果一样。
​		否则便会把节点插入到参照节点之前。这里要注意的是，如果第二个参数不为null，那么插入的节点不能是已经存在的节点。
​		3.replaceChild()方法可以替换节点，接受两个参数，需要插入的节点和需要替换的节点。返回被替换掉的节点。
​		4.removeChild()移除节点。这里有个常见需求，比如我有一个节点 #waste-node ，那么如何移除它呢？
​			var wasteNode =  document.getElementById("waste-node");
​			wasteNode.parentNode.removeClhid(wasteNode);    // 先拿到父节点，再调用removeClild删除自己
​		以上的几个方法都是操作某个节点的子节点，也就是说，操作前必须找到父节点（通过parentNode来找）
​		5.cloneNode();复制节点，接受一个参数 true或者false。如果true就是复制那个节点和它的子节点。
​			如果是false，就是复制节点本身（复制出来的节点就会没有任何子元素）。这个方法返回复制的节点，
​			如果需要操作它，那么需要借助前面讲的4个方法来把这个节点放入到html中去。
​	//document类型：
​	//document对象上的一些属性
​		1.document.childNodes 继承自上面讲的Node类型，可以返回文档的直接子节点（通常包括文档声明和html节点）
​		2.document.documentElement 可以直接拿到html节点的引用（等价于document.getElementsByTagName("html")[0]）。
​		3.document.body body节点的引用
​		4.document.title  页面的title，可以修改，会改变浏览器标签上的名字
​		5.document.URL 页面的url
​		6.document.referrer 取得referrer，也就是打开这个页面的那个页面的地址，做来源统计时候比较有用
​		7.document.domain 取得域名，可以设置，但是通常只能设置为不包含子域名的情况，在一些子域名跨域情况下有效。
​	//document的一些方法：
​		1.getElementById 和 getElementsByTagName
​		2.getElementsByName(根据name返回元素，为htmldocument类型的方法即document对象)；
​		3.write() writeln() open() close()
​			open和close分别是打开和关闭网页的输出流，在页面加载过程中，就相当于open状态。这两个方法一般不会去用它。
​		4.write()和writeln()，它们都是向页面写入东西，区别就是后者会多加入一个换行符。

		//Element类型：日常所操作的都是Element类型，日常所说的“DOM对象”，通常也就是指Element类型的对象。
		//操作属性的方法：getAttribute 、setAttribute 、removeAttribute；


​		

### DOM事件绑定：

//事件监听函数：
	element.addEventListener(event-name,callback,use-capture)//use-capture表示事件实在捕获中监听还是在冒泡中监听
//移除事件事件：
	element.removeEventLister(event-name,callback,use-capture)
//绑定事件的回调函数必须为命名函数，因为接触事件绑定时需要传递这个回调函数的引用才可以断开绑定；
//模拟触发事件
内置的事件也可以被javascript触发，使用dispatchEvent方法
//自定义事件：
	Event.CustomEvent,dispatchEvent;
//直接自定义事件，使用event构造函数：
var event=new event("build");
//listen for event
	elem.addEventListener("bulid",function(e){},false);//当传递参数值时，使用"匿名函数"调用带参数的函数：
									element.addEventListener("click", function(){ myFunction(p1, p2); });
//dispatch the event
	elem.dispatchEvent(event);
//CustomEvent可以创建一个更高度自定义的事件，还可以附带一些数据：
	var myEvent= new CustomEvent(eventName,options);
	options={
		detail:{...},//detail可以放一些初始化信息，可以在触发的时候调用，
		bubbles:true,//定义该事件是否冒泡
		cancelable:false//定义该事件是否可以撤销
	}
//dispatchEvent用于触发自定义事件
//事件顺序：首先进入事件捕获阶段(event captuing)>>达到元素后>>进入事件冒泡阶段(event bubbling)
/*事件代理：
	事件绑定之后，如果监听了一个dom节点，那也就等于监听了其所有的后代节点，代理就是只监听父节点的事件触发以来代理对其后代的监听，
	只需要使用currentTarget属性获取触发元素并作出回应；
	1.使用事件代理可以节省大量重复的事件监听，2.可以使HTML独立起来，之后要对子元素进行监听时，也不需要在对其添加监听事件；
	*/
/*事件的Event对象：
	1.当一个事件被触发的时候，会创建一个事件对象(event object),这个对象包含了一些有用的属性和方法，事件会作为第一个参数传递给我们的回调函数，
		我们可以使用代码在浏览器中打印出这个事件：
		var btn=document.getElementsByTagName("button");
		btn[0].addEventListener('click',function(evnet){
			console.log(evnet)
		},false);
	2.比较常用的几个属性和方法：
		

    type(string): 事件的名称，比如 “click”。
    target(node): 事件要触发的目标节点。
    currentTarget(node): 它就指向正在处理事件的元素：这恰是我们需要的。很不幸的是微软模型中并没有相似的属性, 你也可以使用”this”关键字。
    事件属性也提供了一个值可供访问:event.currentTarget。
    bubbles (boolean): 表明该事件是否是在冒泡阶段触发的。
    preventDefault (function): 这个方法可以禁止一切默认的行为，例如点击 a 标签时，会打开一个新页面，
    如果为 a 标签监听事件 click 同时调用该方法，则不会打开新页面。
    stopPropagation (function): 很多时候，我们触发某个元素，会顺带触发出它父级身上的事件，这有时候是我们不想要的，
    大多数我们想要的还是事件相互独立。所以我们可以选择阻止事件冒泡，使用event.stopPropagation().
    stopImmediatePropagation (function): 与 stopPropagation 类似，就是阻止触发其他监听函数。但是与 stopPropagation 
    不同的是，它更加 “强力”，阻止除了目标之外的事件触发，甚至阻止针对同一个目标节点的相同事件
    cancelable (boolean): 这个属性表明该事件是否可以通过调用 event.preventDefault 方法来禁用默认行为。
    eventPhase (number): 这个属性的数字表示当前事件触发在什么阶段。
        0: none
        1: 捕获
        2: 目标
        3: 冒泡
    pageX 和 pageY (number): 这两个属性表示触发事件时，鼠标相对于页面的坐标。
    isTrusted (boolean): 表明该事件是浏览器触发（用户真实操作触发），还是 JavaScript 代码触发的。
    */
/*事件的回调函数：
	事件绑定函数时，函数会以当前元素为作用域执行，所以回调函数中的this就是当前DOM元素，如果要使用指定作用域，可以对回调函数使用匿名函数包裹，
	或使用bind方法
	*/
/*常用事件

    load 资源加载完成时触发。这个资源可以是图片、CSS 文件、JS 文件、视频、document 和 window 等等。
    DOMContentLoaded DOM构建完毕的时候触发, jQuery的ready方法包裹的就是这个事件。
    beforeunload 当浏览者在页面上的输入框输入一些内容时，未保存、误操作关掉网页可能会导致输入信息丢失。当浏览者输入信息但未保存时关掉网页，我们就可以开始监听这个事件,这时候试图关闭网页的时候，会弹窗阻止操作，点击确认之后才会关闭。当然，如果没有必要，就不要监听，不要以为使用它可以为你留住浏览者。
    resize 当节点尺寸发生变化时，触发这个事件。通常用在 window 上，这样可以监听浏览器窗口的变化。通常用在复杂布局和响应式上。出于对性能的考虑，你可以使用函数 throttle 或者 debounce 技巧来进行优化，throttle 方法大体思路就是在某一段时间内无论多次调用，只执行一次函数，到达时间就执行；debounce 方法大体思路就是在某一段时间内等待是否还会重复调用，如果不会再调用，就执行函数，如果还有重复调用，则不执行继续等待。
    error 当我们加载资源失败或者加载成功但是只加载一部分而无法使用时，就会触发 error 事件，我们可以通过监听该事件来提示一个友好的报错或者进行其他处理。比如 JS 资源加载失败，则提示尝试刷新；图片资源加载失败，在图片下面提示图片加载失败等。该事件不会冒泡。因为子节点加载失败，并不意味着父节点加载失败，所以你的处理函数必须精确绑定到目标节点。
    */

//IE下绑定事件

	在 IE 下面绑定一个事件监听，在 IE9之前的版本中无法使用标准的 addEventListener 函数，而是使用自家的 attachEvent，
	具体用法：element.attachEvent(<event-name>, <callback>);
	
	它只支持监听在冒泡阶段触发的事件，所以为了统一，在使用标准事件监听函数的时候，第三参数传递 false。
	IE下的Event事件

//IE 中往回调函数中传递的事件对象与标准也有一些差异，你需要使用 window.event 来获取事件对象。所以你通常会写出下面代码来获取事件对象：

	event = event || window.event
	
	此外还有一些事件属性有差别，比如比较常用的 event.target 属性，IE 中没有，而是使用 event.srcElement 来代替。
	如果你的回调函数需要处理触发事件的节点，那么需要写:
	
	node = event.srcElement || event.target;
