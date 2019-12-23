###### node.js学习笔记

1. EventEmitter类中通过on函数来绑定事件、通过emit函数来触发一个事件。

2. Buffer类是用来创建一个专门存放二进制数据的缓存区。

3. stream的最大作用：读取大文件的过程中，不会一次性的读入到内存中，每次只会读取数据源的一个数据块，然后后续过程中可以立即处理该数据块而不用等待所有的数据。stream的四种基本类型：

   - Readable  -- 可读的流
   - Writable -- 可写的流
   - Duplex -- 可读写的流
   - Transform -- 在读写过程中可以修改和变换数据的Duplex流

4. 异步模式下打开文件的语法格式：.

   ```
   fs.open(path, flags[,mode],callback)
   ```

   path为文件的路径

   flags 为文件打开的行为

   mode 为设置文件模式权限，文件创建默认权限为0666（可读，可写）

   callback 回调函数

   异步模式下写入文件的语法

   ```
   fs.writeFile(file,data[,options],callback)
   ```

   file 文件名或者文件描述符

   data 要写入的数据

   options 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'

   callback 回调函数

   异步模式下读取文件

   ```
   fs.read(fd, buffer, offset, length, position, callback)
   ```

   fd 通过 fs.open() 方法返回的文件描述符

   buffer 数据写入的缓冲区

   length 要从文件中读取的字节数

   position 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取

   callback 回调函数

5. child_process模块创建子进程的三种方法

   - exec  child_process.exec 使用子进程执行命令，缓存子进程的输出，并将子进程的输出以回调函数参数的形式返回
   - spawn  child_process.spawn 使用指定的命令行参数创建新进程
   - fork  child_process.fork 是 spawn()的特殊形式，用于在子进程中运行的模块，如 fork('./son.js') 相当于  spawn('node', ['./son.js'])  。与spawn方法不同的是，fork会在父进程与子进程之间，建立一个通信管道，用于进程之间的通信



