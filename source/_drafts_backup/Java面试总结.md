title: Java面试总结
tags:
- java
---

1. Object o=new Object(): 占多少个字节
Object 0; 对象引用占4个字节
new Object(); 空对象占8个字节
故：总字节数据为12; 但Java的内存是按8的倍数(高低八位)分配的，所以是16byte

---
举例说明:
		public class ObjectC { //空对象8字节
			int i; //基本类型：整型4字节
			byte j; //基本类型: 1个字节
			String str; //引用类型: 4字节
		}
空对象(8) + int(4) + byte(1) + String(4) = 17, 内存8倍数分配,故为24byte
