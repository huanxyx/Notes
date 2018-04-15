##随手笔记
- `Class.getResource(path)`中path不以"/"开头则以该类所在的包下取资源，path以"/"开头则从ClassPath根下取资源
- `ClassLoader.getResource`从ClassPath根下取资源，不能以"/"开头
- 浏览器编码URL是将非ASCII字符按照某种编码格式编码成16进制数字后将每个16进制表示的字节前加上“%”
- js的`encodeURI()`是将整个URI进行编码，真正对URL进行编码的函数。而`encodeURIComponent()`则可以将一个URL作为一个参数放在另一个URL中(彻底)。
- java中用`URLDecoder`对js的`encodeURIComponent`进行解码
- Bootstrap ClassLoader是由C++编写的，本身是虚拟机的一部分。