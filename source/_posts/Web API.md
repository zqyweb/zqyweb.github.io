---
title: WEB API
---


## DOM树

	核心DOM和HTML DOM
	核心DOM：操作一切结构化文档的统一API
	特点：繁琐！万能！

	HTML DOM:专门操作HTML文档的简化版DOM API
	特点：简洁，不是万能！
	
	节点对象：Node
	三大属性：nodeType、nodeName、nodeValue

### DOM:查找

	方式一：不要查找直接获得元素
	html document.documentElement
	head document.head
	body document.body
	
	方式二：按节点间关系查找
	1.父子关系
	elem.parentNode	获得elem的父节点
	elem.childNodes 获得elem的所有**直接**子节点
	elem.firstChild 获得elem下的第一个子节点
	elem.lastChild  获得elem下的最后一个子节点
	2.兄弟节点
	elem.previousSibling 获得elem的前一个兄弟节点
	elem.nextSibling 获得elem的后一个兄弟节点
	
**存在的问题：**

 
- 一切文本都是节点对象，包括看不见的空字符（tab,空格，换行）

**解决：元素树**
	
	方式三：元素树
	1.父子关系
	elem.parentElement 获得elem的父元素
	elem.children 获得elem的所有**直接**子元素
	elem.firstElementChild 获取elem下的第一个子元素
	elem.lastElementChild 获得elem下的最后一个子元素
	2.兄弟关系
	elem.previousElementSibling 获得elem的前一个兄弟元素
	elem.nextElementSibling 获得elem的后一个兄弟元素
	
	元素树不是一颗新树，只是节点树的一个子集
 	
**存在问题：**

-  只适合IE9+的浏览器
- children和childNodes只能查找直接子节点，无法查找更深层次

**解决：**

	1.递归遍历
	缺点：反复访问集合，导致反复查找DOM树，效率低
	解决2：节点迭代器
	
	方法四：按HTML查找
	1.按id查找
	var elem = document.getElementById("ID名");
	2.按标签名查找
	var elems = parent.getElementsByTagName("标签名");
	可查找所有后代元素
	3.按name属性查找
	var elems = document.getElementsByName("name");
	4.按class属性查找
	var elems = parent.getElementsByClassName("class");
	适用于：一个元素被多个class修饰。在所有后代中查找
	
**问题：**每次只能按一种条件查找，当查找条件复杂时，步骤就非常繁琐。

**解决：**用选择器查找

	1.仅查找一个元素
	var elem = parent.querySelector("选择器");
	2.查找多个元素
	var elems = parent.querySelectorAll("选择器");
	返回非动态集合。但是存在浏览器兼容性的问题。
	非动态集合：实际存储完整数据，即使反复访问集合，也不会反复查找DOM树
	

### DOM:修改
	
修改分为修改内容，属性，样式三种。

	1.内容
	- 获取或修改元素的HTML代码片段内容
	elem.innerHTML
	-2.获取或修改元素的纯文文本内容
	elem.textContent(与innerHTML相比，去掉了内嵌标签，将转义字符翻译为原文)
	-获取或修改表单元素的值
	elem.value 

	2.属性
	- 核心DOM:
	属性节点都保存在elem的attributes集合中
	var node = elem.attributes["属性名"];
	var value = node.nodeValue;
	获取：var value = elem.getAttribute("属性名");
	修改：elem.setAttribute("属性名","值");
	移除：elem.removeAttribute("属性名");
	判断有没有：var bool = elem.hasAttribute("属性名");  
	可以直接使用：class

	- HTML DOM:
	获取：elem.属性名
	修改：elem.属性名=值
	移除：elem.属性名 = ""
	判断有没有：elem.属性名！==""
	.className ===》 就是HTML中的class

	状态属性：disabled selected checked	
	特点：值是bool类型
	不能用核心DOM类型，只能用HTML DOM类型

	- 自定义扩展属性：
	定义：data-属性名="值"
	获取：核心DOM		HTML5:elem.dataset.属性名
	只要标识多个元素，且不希望人为或是被程序修改
	只要给元素添加行为时，查找元素都用自定义扩展属性
	
	- 样式

	1.内联样式
	特点：优先级最高，当前元素独有
	- 修改一个内联样式
	elem.style.css属性 = 值
	所有css属性要去横线变驼峰
	-批量替换内联样式
	elem.style.cssText ="";
	
	2.获得计算后的样式(最终应用到当前元素上的所有样式的合集，一个元素的完整样式，可能来源多个地方)
	//获得计算后的style对象
	var style = getComputedStyle(elem);
	//获得style中的css属性值
	var value = style.css属性
	3.修改样式中的样式：
	//获得样式表对象
	var sheet = document.styleSheets[i];
	//获得样式表对象中的一套规则：
	var rule = sheet.cssRules[i];
	//修改样式
	var style = rule.style;
	style.css属性 = 值

### DOM:添加和删除
	
	//1.创建空元素
	var a = document.creatElement("a");
	//设置关键属性
	a.href = "http://baidu.com";
	a.innerHTML="go to baidu";
	//3.将新元素添加到DOM树
	末尾追加：parent.appendChild(a);
	中间插入：parent.insertBefore(a,child);
	替换：parent.replace(a,child);

**总结**：尽量减少操作DOM树的次数
	每次操作DOM树都会导致重新layout

	![layout](https://i.imgur.com/htVxk7k.jpg)

	只要修改DOM树(修改样式，修改位置，添加删除元素等)都会导致重新layout---效率低

	解决：如果同时添加父元素和子元素，都要先在内存中将子元素，添加到父元素中，最后再将父元素，一次性添加到DOM树。
	或是父元素已经在DOM树上了，要同时添加多个平级子元素时：
	文档片段：内存中，临时存储一课DOM子树段的存储空间
	//创建文档片段对象
	var frag = document.creatDocumentFragment();
	//先将子元素添加到DOM树指定父元素下
	frag.appendChild(child);
	//将文档片段一次性添加到DOM树指定父元素下
	parent.appendChild(frag);
	frag将子元素送到DOM树后，自动释放。



 **HTML DOM常用对象: **

	 什么是: 对常用的元素，提供了简化版的API
	 优: 简化
	 缺: 不是万能
	 Image: 创建:  var img=new Image();
	 Select: 
	  属性: .selectedIndex 快速获得当前选中项的下标位置
	       .value 当选中项有value属性时，会返回option的value
	             如果选中项没有value属性，则用内容代替
	       .options: 获得select下所有option元素对象的集合
	         options.length 获得选项的个数
	       .length => .options.length
	         固定套路: 清空所有option
	           sel.innerHTML="";
	           sel.length=0; =>sel.options.length=0;
	  方法: add(option) 代替 appendChild(option)
	        问题: 不支持frag
	       remove(i) 移除i位置的option
	 Option:
	  创建: var opt=new Option(text,value);
	  属性: .index   .text    .value

	 table:管着行分组: 
	  创建: var thead=table.createTHead()
	       var tbody=table.createTBody();
	       var tfoot=table.createTFoot();
	  删除: table.deleteTHead()
	       table.deleteTFoot()
	  获取: tabel.tHead   table.tFoot   table.tBodies[i]
	行分组:管着行: 
	  创建: var tr=行分组.insertRow(i)
	    固定套路: 1. 在结尾追加一行: 行分组.insertRow()
	             2. 在开头插入一行: 行分组.insertRow(0)
	  删除: 行分组.deleteRow(i)
	    强调: 主语是行分组时，i要求是在行分组内的相对下标位置
	  获取: 行分组.rows  获得行分组内所有行的集合
	行:管着格:
	  创建: var td=tr.insertCell(i)
	    固定套路: 末尾追加: tr.insertCell()
	  删除: tr.deleteCell(i)
	  获取: tr.cells
	 
	 form: 
	  获取: var form=document.forms[i/id/name];
	  属性: form.elements 获得表单中所有表单元素的值
	      强调: 表单元素包括: input  select  textarea  button
	       form.elements.length 获得表单中表单元素的个数
	       form.length==> form.elements.length
	        固定套路: 获得结尾的按钮: 
	                var btn=form.elements[form.length-n]
	  方法: form.submit() //在程序中手动提交表单
	 表单元素: 
	  获取: var 表单元素= form.elements[i/id/name]
	    如果表单元素有name属性，则: form.name属性值
	  方法: elem.focus() 让当前表单获得焦点  
	       elem.blur()



  


