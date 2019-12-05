日志级别等级CRITICAL > ERROR > WARNING > INFO > DEBUG > NOTSET

logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
                    datefmt='%a, %d %b %Y %H:%M:%S',
                    filename='/tmp/test.log',
                    filemode='w')

format参数中可能用到的格式化串：
%(name)s Logger的名字
%(levelno)s 数字形式的日志级别
%(levelname)s 文本形式的日志级别
%(pathname)s 调用日志输出函数的模块的完整路径名，可能没有
%(filename)s 调用日志输出函数的模块的文件名
%(module)s 调用日志输出函数的模块名
%(funcName)s 调用日志输出函数的函数名
%(lineno)d 调用日志输出函数的语句所在的代码行
%(created)f 当前时间，用UNIX标准的表示时间的浮 点数表示
%(relativeCreated)d 输出日志信息时的，自Logger创建以 来的毫秒数
%(asctime)s 字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896”。逗号后面的是毫秒
%(thread)d 线程ID。可能没有
%(threadName)s 线程名。可能没有
%(process)d 进程ID。可能没有
%(message)s用户输出的消息

logging库提供了多个组件：
	Logger、Handler、Filter、Formatter。Logger对象提供应用程序可直接使用的接口，Handler发送
	日志到适当的目的地，Filter提供了过滤日志信息的方法，Formatter指定日志显示格式。

*** logger
	logging.getLogger([name])（返回一个logger对象，如果没有指定名字将返回root logger）
		import logging 
	#创建两个 name 相同的logger
		logger1=logging.getLogger('mylogger')
		logger1.setLevel(logging.DEBUG)
		logger2=logging.getLogger('mylogger')
		logger2=setLevel(logging.INFO)
		
		"""
		只要logging.getLogger（name）中名称参数name相同则返回的Logger实例就是同一个，且仅有一个，
		也即name与Logger实例一一对应。即此处logger1输出的日志等级与logger2相同为INFO以上。
		"""
	#创建一个handler，用于写入日志文件
		fh=logging.FileHandler('/home/wangzhongju/workspace/test.log')
	#创建一个handler，用于输出到控制台
		ch=logging.StreamHandler()
	#定义handler的输出格式
		formatter=logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
		fh.setFormatter(formatter)
		ch.setFormatter(formatter)
	#定义一个filter
		filter = logging.Filter('root')
		fh.addFilter(filter)
		logger.addFilter(filter)
	#给logger添加handler
		logger.addHandler(fh)
		logger.addHandler(ch)
	# 记录一条日志
		logger.debug('logger debug message')
		logger.info('logger info message')
		logger.warning('logger warning message')
		logger.error('logger error message')
		logger.critical('logger critical message')

Handler对象负责发送相关的信息到指定目的地，有几个常用的Handler方法：
	Handler.setLevel(lel):指定日志级别，低于lel级别的日志将被忽略
	Handler.setFormatter()：给这个handler选择一个Formatter
	Handler.addFilter(filt)、Handler.removeFilter(filt)：新增或删除一个filter对象


*** Formatter
	Formatter对象设置日志信息最后的规则、结构和内容，默认的时间格式为%Y-%m-%d %H:%M:%S。

*** Filter
	限制只有满足过滤规则的日志才会输出

除了直接在程序中设置Logger，Handler,Filter,Formatter外还可以将这些信息写进配置文件中。
典型的logging.conf:
	[loggers]
	keys=root,simpleExample
 
	[handlers]
	keys=consoleHandler
 
	[formatters]
	keys=simpleFormatter
 
	[logger_root]
	level=DEBUG
	handlers=consoleHandler
 
	[logger_simpleExample]
	level=DEBUG
	handlers=consoleHandler
	qualname=simpleExample
	propagate=0
 
	[handler_consoleHandler]
	class=StreamHandler
	level=DEBUG
	formatter=simpleFormatter
	args=(sys.stdout,)
 
	[formatter_simpleFormatter]
	format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
	datefmt=




























