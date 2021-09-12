# path.join()

```
　　path.join()方法是将多个参数字符串合并成一个路径字符串

　　console.log(path.join(__dirname,'a','b'));   假如当前文件的路径是E:/node/1,那么拼接出来就是E:/node/1/a/b。

　　console.log(path.join(__dirname,'/a','/b','..'));  路径开头的/不会影响拼接，..代表上一级文件，拼接出来的结果是：E:/node/1/a
　　console.log(path.join(__dirname,'a',{},'b'));   而且path.join()还会帮我们做路径字符串的校验，当字符串不合法时，会抛出错误：Path must be a string.
```

# path.resolve()
```
　path.resolve()方法是以程序为根目录，作为起点，根据参数解析出一个绝对路径
　　以应用程序为根目录
　　普通字符串代表子目录
　　/代表绝对路径根目录
　　
　　console.log(path.resolve());   得到应用程序启动文件的目录（得到当前执行文件绝对路径）   E:\zf\webpack\1\src
　　console.log(path.resolve('a','/c'));   E:/c  ,因为/斜杠代表根目录，所以得到的就是E:/c
　　所以我们一般拼接的时候需要小心点使用/斜杠
　　console.log(path.resolve(__dirname,'img/so'));  E:\zf\webpack\1\src\img\so   这个就是将文件路径拼接，并不管这个路径是否真实存在。
　　console.log(path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif'))    E:\zf\webpack\1\src\wwwroot\static_files\gif\image.gif
　　这个是用当前应用程序启动文件绝对路径与后面的所有字符串拼接，因为最开始的字符串不是以/开头的。
　　..也是代表上一级目录。
```