# 日志记录的重要性

>在开发过程中，如果程序运行出现了问题，我们是可以使用我们自己的 Debug 工具来检测到到底是哪一步出现了问题，如果出现了问题的话，是很容易排查的。但程序开发完成之后，我们会将它部署到生产环境中去，这时候代码相当于是在一个黑盒环境下运行的，我们只能看到其运行的效果，是不能直接看到代码运行过程中每一步的状态的。在这个环境下，运行过程中难免会在某个地方出现问题，甚至这个问题可能是我们开发过程中未曾遇到的问题，碰到这种情况应该怎么办？
>如果我们现在只能得知当前问题的现象，而没有其他任何信息的话，如果我们想要解决掉这个问题的话，那么只能根据问题的现象来试图复现一下，然后再一步步去调试，这恐怕是很难的，很大的概率上我们是无法精准地复现这个问题的，而且 Debug 的过程也会耗费巨多的时间，这样一旦生产环境上出现了问题，修复就会变得非常棘手。但这如果我们当时有做日志记录的话，不论是正常运行还是出现报错，都有相关的时间记录，状态记录，错误记录等，那么这样我们就可以方便地追踪到在当时的运行过程中出现了怎样的状况，从而可以快速排查问题。
>因此，日志记录是非常有必要的，任何一款软件如果没有标准的日志记录，都不能算作一个合格的软件。作为开发者，我们需要重视并做好日志记录过程。

# 日志记录流程框架

日志记录框架可分为以下几个部分：

* Logger：即 Logger Main Class，是我们进行日志记录时创建的对象，我们可以调用它的方法传入日志模板和信息，来生成一条条日志记录，称作 Log Record。

* Log Record：就代指生成的一条条日志记录。

* Handler：即用来处理日志记录的类，它可以将 Log Record 输出到我们指定的日志位置和存储形式等，如我们可以指定将日志通过 FTP 协议记录到远程的服务器上，Handler 就会帮我们完成这些事情。

* Formatter：实际上生成的 Log Record 也是一个个对象，那么我们想要把它们保存成一条条我们想要的日志文本的话，就需要有一个格式化的过程，那么这个过程就由 Formatter 来完成，返回的就是日志字符串，然后传回给 Handler 来处理。

* Filter：另外保存日志的时候我们可能不需要全部保存，我们可能只需要保存我们想要的部分就可以了，所以保存前还需要进行一下过滤，留下我们想要的日志，如只保存某个级别的日志，或只保存包含某个关键字的日志等，那么这个过滤过程就交给 Filter 来完成。

* Parent Handler：Handler 之间可以存在分层关系，以使得不同 Handler 之间共享相同功能的代码。

# 日志记录的相关用法

logging 模块相比 print 有这么几个优点：

* 可以在 logging 模块中设置日志等级，在不同的版本（如开发环境、生产环境）上通过设置不同的输出等级来记录对应的日志，非常灵活。

* print 的输出信息都会输出到标准输出流中，而 logging 模块就更加灵活，可以设置输出到任意位置，如写入文件、写入远程服务器等。

* ogging 模块具有灵活的配置和格式化功能，如配置输出当前模块信息、运行时间等，相比 print 的字符串格式化更加方便易用。

# 日志模块的使用
1. 导入
    ```python
    import logging # 导入模块
    ```

2. 创建日志对象
    ```python
    logger = logging.getLogger(name)
    参数：日志的名字
    ```

3. 配置
    ```python
    logging.basicConfig(
        filename='./test.log',
        level=logging.INFO,
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    )
    ```
    ```
    功能：进行基本配置
    参数：level 指定日志输出的级别，程序会输出大于等于此级别的信息
         format -> 格式字符串
         filename -> 日志输出文件名，设置此参数后日志将写入文件而不是打印在终端
         filemode -> 这个是指定日志文件的写入方式，有两种形式，一种是 w，一种是 a，分别代表清除后写入和追加写入
         datefmt -> 指定时间的输出格式
         handlers -> 可以指定日志处理时所使用的Handlers，必须是可迭代的
         stream -> 在没有指定 filename 的时候会默认使用 StreamHandler，这时 stream 可以指定初始化的文件流
         style -> 如果 format 参数指定了，这个参数就可以指定格式化时的占位符风格，如 %、{、$ 等
    ```
    format常用格式化参数（详见<https://docs.python.org/3/library/logging.html?highlight=logging%20threadname#logrecord-attributes>）
    参数 | 功能
    :-: | :-:
    %(levelno)s | 打印日志级别的数值。
    %(levelname)s | 打印日志级别的名称。
    %(pathname)s | 打印当前执行程序的路径，其实就是sys.argv[0]。
    %(filename)s | 打印当前执行程序名。
    %(funcName)s | 打印日志的当前函数。
    %(lineno)d | 打印日志的当前行号。
    %(asctime)s | 打印日志的时间。
    %(thread)d | 打印线程ID。
    %(threadName)s | 打印线程名称。
    %(process)d | 打印进程ID。
    %(processName)s | 打印线程名称。
    %(module)s | 打印模块名称。
    %(message)s | 打印日志信息

    实例：
    ```python
    import logging

    logging.basicConfig(level=logging.DEBUG,
                        filename='output.log',
                        datefmt='%Y/%m/%d %H:%M:%S',
                        format='%(asctime)s - %(name)s - %(levelname)s - %(lineno)d - %(module)s - %(message)s')
    logger = logging.getLogger(__name__)

    logger.info('This is a log info')
    logger.debug('Debugging')
    logger.warning('Warning exists')
    logger.info('Finish')
    ```
    output.log内容：
    ```
    2020/08/21 20:20:45 - __main__ - INFO - 9 - test - This is a log info
    2020/08/21 20:20:45 - __main__ - DEBUG - 10 - test - Debugging
    2020/08/21 20:20:45 - __main__ - WARNING - 11 - test - Warning exists
    2020/08/21 20:20:45 - __main__ - INFO - 12 - test - Finish
    ```

    日志等级：
    
    等级 | 数值
    :-: | :-:
    CRITICAL（严重告警） | 50
    FATAL（致命） | 50
    ERROR（错误） | 40
    WARNING（警告） | 30
    WARN（警告） | 30
    INFO（信息） | 20
    DEBUG（调试） | 10
    NOTSET（未设置） | 0

    其他配置方法：
    ```python
    import logging

    logger = logging.getLogger(__name__)
    # 从这里开始
    logger.setLevel(level=logging.INFO)
    handler = logging.FileHandler('output.log')
    formatter = logging.Formatter(
        fmt='%(asctime)s - %(name)s - %(levelname)s - %(message)s', 
        datefmt='%Y/%m/%d %H:%M:%S'
    )
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    # 到这里结束，手动创建及添加了所有配置对象

    logger.info('This is a log info')
    logger.debug('Debugging')
    logger.warning('Warning exists')
    logger.info('Finish')
    ```

4. 输出详细报错信息
    ```python
    import logging

    logger = logging.getLogger(__name__)
    logger.setLevel(level=logging.DEBUG)

    # Formatter
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

    # FileHandler
    file_handler = logging.FileHandler('result.log')
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)

    # StreamHandler
    stream_handler = logging.StreamHandler()
    stream_handler.setFormatter(formatter)
    logger.addHandler(stream_handler)

    # Log
    logger.info('Start')
    logger.warning('Something maybe fail.')
    try:
        result = 10 / 0
    except Exception:
        logger.error('Faild to get result', exc_info=True)
    logger.info('Finished')
    ```
    运行结果：
    ```
    2018-06-03 16:00:15,382 - __main__ - INFO - Start print log
    2018-06-03 16:00:15,382 - __main__ - DEBUG - Do something
    2018-06-03 16:00:15,382 - __main__ - WARNING - Something maybe fail.
    2018-06-03 16:00:15,382 - __main__ - ERROR - Faild to get result
    Traceback (most recent call last):
    File "/private/var/books/aicodes/loggingtest/demo8.py", line 23, in <module>
        result = 10 / 0
    ZeroDivisionError: division by zero
    2018-06-03 16:00:15,383 - __main__ - INFO - Finished
    ```
5. 其他Handler
* StreamHandler：logging.StreamHandler；日志输出到流，可以是 sys.stderr，sys.stdout 或者文件。

* FileHandler：logging.FileHandler；日志输出到文件。

* BaseRotatingHandler：logging.handlers.BaseRotatingHandler；基本的日志回滚方式。

* RotatingHandler：logging.handlers.RotatingHandler；日志回滚方式，支持日志文件最大数量和日志文件回滚。

* TimeRotatingHandler：logging.handlers.TimeRotatingHandler；日志回滚方式，在一定时间区域内回滚日志文件。

* SocketHandler：logging.handlers.SocketHandler；远程输出日志到TCP/IP sockets。

* DatagramHandler：logging.handlers.DatagramHandler；远程输出日志到UDP sockets。

* SMTPHandler：logging.handlers.SMTPHandler；远程输出日志到邮件地址。

* SysLogHandler：logging.handlers.SysLogHandler；日志输出到syslog。

* NTEventLogHandler：logging.handlers.NTEventLogHandler；远程输出日志到Windows NT/2000/XP的事件日志。

* MemoryHandler：logging.handlers.MemoryHandler；日志输出到内存中的指定buffer。

* HTTPHandler：logging.handlers.HTTPHandler；通过”GET”或者”POST”远程输出到HTTP服务器。