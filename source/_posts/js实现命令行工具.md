---
title: js实现命令行工具(2019-5-24)
---

#### 一、首先写一个可执行脚本
新建一个`js`文件`hello.js`，这个文件就是之后用自定义命令唤醒的文件

```js
#!/usr/bin/env node
//第一行用来描述node的所在位置，不可省
console.log('hello pig!')
```
#### 二、修改这个文件的权限

```
chmod 755 hello
```
这个命令用来规范文件所有者（7——可读可写可操作）、同一个的用户组和其他用户组的权限（5——可读可操作），主要是增加可操作权（5——可读可操作），如果想要查看文件目前权限可以输入`ls -l`

修改之后`hello.js`就可以执行了，在命令行输入`./hello.js`就能看到`hello pig!`

#### 三、修改唤醒路径
方法一：将`hello.js`的路径加入环境变量`PATH`

方法二： 

* 在当前目录创建`package.json`，添加如下内容

    ```
    {
      "name": "hello",
      "bin": {
        "pig": "hello"
        //前者是执行命令，后者是执行文件名称
      }
    }
    ```
    
* 执行命令`npm link`

* 现在可以直接输入`pig`直接唤醒文件，输入想要的内容

#### 四、命令行参数
在执行文件时，在执行命令后加参数，可以用`process.argv[2]`获取，所有的参数分别表表node在系统中的位置，以及需要唤醒的js文件在系统中的位置，以及后带的参数

#### 五、修改可执行文件
为了让整个程序更像是命令行，可以将需要的操作或函数提成另外的文件`index.js`，再将之前的`hello.js`修改成如下

```js
#!/usr/bin/env node
const readline = require('readline'); //引入nodejs的内置模块readline，这样可以获取命令行的输入
let catOrPig = require('./index'); 
//从其他文件引入函数
const rl = readline.createInterface(process.stdin, process.stdout);
let language = process.argv[2] 
//这里的process.argv是唤醒程序的参数数组，前两个参数代表node在系统中的位置，以及需要唤醒的js文件在系统中的位置
rl.setPrompt('cop> '); 
//这里可以选择设置每一行的提示符
rl.prompt();
//这里用来监听读取命令行的用户输入

rl.on('line', function(line) {
	switch(line) {
	//参数line为用户在这一行的输入
        case 'exit':
            rl.close();
            //退出
            break;
        default:
            catOrPig(language, line);
            //需要执行的操作
            break;
    }
    rl.prompt();
    //继续监听命令行输入
});

rl.on('close', function() {
    process.exit(0);
});
```
