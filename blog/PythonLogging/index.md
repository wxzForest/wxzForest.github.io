# Python Logging


##### code

```
import logging
import os
import time

project_dir = os.path.abspath(os.path.dirname(os.path.dirname(os.path.dirname(__file__))))


class CommonLog:
    """
    封装logging模块
    """

    def __init__(self, name="common_log", level: logging = logging.INFO):
        """
        实例化logging
        :param name: 日志名称
        :param level: 日志级别
        """
        # self.fmt = "%(asctime)s %(name)s %(filename)s %(module)s %(funcName)s %(lineno)d %(message)s"
        self.fmt = "%(asctime)s [%(name)s] [%(filename)s] [%(levelname)s] [%(lineno)d] - %(message)s"
        self.name = name
        self.level = level
        self.formatter = logging.Formatter(self.fmt)

    def create_logger(self):
        """
        创建一个logger，并设置日志级别
        :return:
        """
        logger = logging.getLogger(self.name)
        logger.setLevel(self.level)
        return logger

    def create_handle(self):
        """
        创建所需要的handel，并指定输出格式
        :return:
        """
        time_stamp = time.strftime('%Y%m%d%H%M%S', time.localtime(time.time()))
        log_folder = os.path.join(project_dir, "run/log_report")
        log_dir = os.path.join(log_folder, f"{time_stamp}.log")

        path = log_folder.strip()
        path = path.rstrip("\\")
        path = path.rstrip("/")

        # 判断路径是否存在
        is_exists = os.path.exists(path)

        if not is_exists:
            try:
                os.makedirs(path)
            except Exception as e:
                print(e)

        file_write = logging.FileHandler(log_dir, encoding="utf-8")
        file_write.setFormatter(self.formatter)
        file_write.setLevel(logging.DEBUG)

        file_print = logging.StreamHandler()
        file_print.setFormatter(self.formatter)
        file_print.setLevel(logging.DEBUG)
        return file_write, file_print

    def add_handle(self):
        """
        为handle添加日志处理器
        :return:
        """
        logger = self.create_logger()
        create_handle = self.create_handle()
        logger.addHandler(create_handle[0])
        logger.addHandler(create_handle[1])
        return logger
```

##### logging模块的四大组件

| 组件       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| loggers    | 提供应用程序代码直接使用的接口                               |
| handlers   | 用于将日志记录发送到指定的目的位置                           |
| filters    | 提供更细粒度的日志过滤功能，用于决定哪些日志记录将会被输出（其它的日志记录将会被忽略） |
| formatters | 用于控制日志信息的最终输出格式                               |

##### logging.basicConfig()函数说明

该方法用于为logging日志系统做一些基本配置，方法定义如下：

```
logging.basicConfig(**kwargs)
```

该函数可接收的关键字参数如下：

| 参数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| filename | 指定日志输出目标文件的文件名，指定该设置项后日志信心就不会被输出到控制台了 |
| filemode | 指定日志文件的打开模式，默认为'a'。需要注意的是，该选项要在filename指定时才有效 |
| format   | 指定日志格式字符串，即指定日志输出时所包含的字段信息以及它们的顺序。logging模块定义的格式字段下面会列出。 |
| datefmt  | 指定日期/时间格式。需要注意的是，该选项要在format中包含时间字段%(asctime)s时才有效 |
| level    | 指定日志器的日志级别                                         |
| stream   | 指定日志输出目标stream，如sys.stdout、sys.stderr以及网络stream。需要说明的是，stream和filename不能同时提供，否则会引发 `ValueError`异常 |
| style    | Python 3.2中新添加的配置项。指定format格式字符串的风格，可取值为'%'、'{'和'$'，默认为'%' |
| handlers | Python 3.3中新添加的配置项。该选项如果被指定，它应该是一个创建了多个Handler的可迭代对象，这些handler将会被添加到root logger。需要说明的是：filename、stream和handlers这三个配置项只能有一个存在，不能同时出现2个或3个，否则会引发ValueError异常。 |

##### logging模块定义的格式字符串字段

我们来列举一下logging模块中定义好的可以用于format格式字符串中字段有哪些：

| 字段/属性名称   | 使用格式            | 描述                                                         |
| --------------- | ------------------- | ------------------------------------------------------------ |
| asctime         | %(asctime)s         | 日志事件发生的时间--人类可读时间，如：2003-07-08 16:49:45,896 |
| created         | %(created)f         | 日志事件发生的时间--时间戳，就是当时调用time.time()函数返回的值 |
| relativeCreated | %(relativeCreated)d | 日志事件发生的时间相对于logging模块加载时间的相对毫秒数（目前还不知道干嘛用的） |
| msecs           | %(msecs)d           | 日志事件发生事件的毫秒部分                                   |
| levelname       | %(levelname)s       | 该日志记录的文字形式的日志级别（'DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL'） |
| levelno         | %(levelno)s         | 该日志记录的数字形式的日志级别（10, 20, 30, 40, 50）         |
| name            | %(name)s            | 所使用的日志器名称，默认是'root'，因为默认使用的是 rootLogger |
| message         | %(message)s         | 日志记录的文本内容，通过 `msg % args`计算得到的              |
| pathname        | %(pathname)s        | 调用日志记录函数的源码文件的全路径                           |
| filename        | %(filename)s        | pathname的文件名部分，包含文件后缀                           |
| module          | %(module)s          | filename的名称部分，不包含后缀                               |
| lineno          | %(lineno)d          | 调用日志记录函数的源代码所在的行号                           |
| funcName        | %(funcName)s        | 调用日志记录函数的函数名                                     |
| process         | %(process)d         | 进程ID                                                       |
| processName     | %(processName)s     | 进程名称，Python 3.1新增                                     |
| thread          | %(thread)d          | 线程ID                                                       |
| threadName      | %(thread)s          | 线程名称                                                     |

##### logger.setLevel()和handler.setLevel()：

logger.setLevel()必须设置，否则不生效。

logger.setLevel()的级别可设置DEBUG，handler.setLevel()设置INFO，则只对INFO生效，反之则不生效。


