## 名称：16进制服务通讯框架xxl-hex

## 功能：client端和server端的请求和响应消息，进行hex编码；主要为了达到两个目的：1、消息加密，2、跨语言；

## hex通讯逻辑：
	第一步：client端：封装IRequest消息，按照注解规则，将消息按照压入字节数组，最终编码为hex，POST给server端；
	第二步：server端：
		a：接收hex格式请求消息，解码为字节数组，实例化IRequest消息，将字节数组写入消息字段属性上；
		b：根据IRequest消息，匹配对应的Handler(维护在一张哈希表中)，反射实例化handler，传入IRequest消息，执行handle方法；
		c：获取handler返回IResponse消息，按照消息注解规则，将消息压入字节数组，编码为hex，响应给client端；
	第三部：client端：
		a：接收hex格式响应消息，解码为字节数组，实例化IResponse消息，将字节数组写入消息字段属性上；
		b：一次hex通讯Finish；
		
## 消息体接口详解：
	请求消息：
	com.xxl.hex.request.DemoRequest
	{
		@HexField(length=20)
		String paramA;		
		String paramB;		
		int paramC;		
	}
	
	请求消息-字节数组，内容如下：
	"com.xxl.hex.request.DemoRequest"	// 按照className字节长度压入字节数组
	"paramA"								// 按照注解length压入20个字节至字节数组
	"paramB"								// 按照paramB字节长度压入字节数组
	"paramC"								// 压入4个几节到字节数组
	
	请求消息-hex： String hex = makeHex(makeByte(demoRequest));
	


