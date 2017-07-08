#Map在js中的使用

创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果
 
##js方法

	var datas =[{a:1,b:2,c:3},{a:11,b:22,c:33},{a:111,b:222,c:333}];
	var datasNew = datas.map(function(data){
		return {
			a1:data.a,
			b1:data.b,
			c1:data.c
		}
	});

具体使用详情请参考：[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

##JQuery方法

	var datas =[{a:1,b:2,c:3},{a:11,b:22,c:33},{a:111,b:222,c:333}];
	var datasNew = $.map(datas,function(data){
		return {
			a1:data.a,
			b1:data.b,
			c1:data.c
		}
	});
