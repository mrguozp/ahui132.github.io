---
layout: page
title:
category: blog
description:
---
# Preface

# time

	import time
	time.time()
		19972314124.05238
	time.ctime()
        'Tue Sep 20 15:19:28 2016'

## sleep

	import time
	time.sleep(1)#1s
	time.sleep(random.random())

# datetime

	from datetime import datetime, timedelta

## 获取当前日期和时间

	>>> from datetime import datetime
	>>> now = datetime.now() # 获取当前datetime
	>>> print(now)
	2015-05-18 16:28:07.198690
	>>> print(type(now))
	<class 'datetime.datetime'>

	d.year
	d.month
	d.weakday() 0~6
	d.day

	d.hour
	d.minute
	d.second
	d.microsecond

	date
		d.date() 2016-08-31
	time
		d.time() 09:41:52.256441
	datetime
		d


## 设定指定日期和时间
要指定某个日期和时间，我们直接用参数构造一个datetime：

	>>> from datetime import datetime
	>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
	>>> print(dt)
	2015-04-19 12:20:00

# timestamp

## datetime.timestamp()
你可以认为：

	timestamp = 0 = 1970-1-1 00:00:00 UTC+0:00

对应的北京时间是：

	timestamp = 0 = 1970-1-1 08:00:00 UTC+8:00

可见timestamp的值与时区毫无关系，因为timestamp一旦确定，其UTC时间就确定了，转换到任意时区的时间也是完全确定的，这就是为什么计算机存储的当前时间是以timestamp表示的，因为全球各地的计算机在任意时刻的timestamp都是完全相同的（假定时间已校准）。

把一个datetime类型转换为timestamp只需要简单调用timestamp()方法：

	>>> from datetime import datetime
	>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
	>>> dt.timestamp() # 把timestamp转换为datetime
	1429417200.0

注意Python的timestamp是一个浮点数。如果有小数位，小数位表示毫秒数。

## datetime.fromtimestamp()
要把timestamp转换为datetime，使用datetime提供的fromtimestamp()方法：

	>>> from datetime import datetime
	>>> t = 1429417200.0
	>>> print(datetime.fromtimestamp(t))
	2015-04-19 12:20:00

本地时间是指当前操作系统设定的时区。例如北京时区是东8区，则本地时间实际上就是UTC+8:00时区的时间：

	2015-04-19 12:20:00 UTC+8:00

而此刻的格林威治标准时间与北京时间差了8小时，也就是UTC+0:00时区的时间应该是：

	2015-04-19 04:20:00 UTC+0:00

timestamp也可以直接被转换到UTC标准时区的时间：

	>>> from datetime import datetime
	>>> t = 1429417200.0
	>>> print(datetime.fromtimestamp(t)) # 本地时间
	2015-04-19 12:20:00
	>>> print(datetime.utcfromtimestamp(t)) # UTC时间
	2015-04-19 04:20:00

# datestr
和shell 一样

	%Y-%m-%d %H:%M:%S

## strptime: datestr 转换为datetime
很多时候，用户输入的日期和时间是字符串，要处理日期和时间，首先必须把str转换为datetime。转换方法是通过datetime.strptime()实现，需要一个日期和时间的格式化字符串：

	>>> from datetime import datetime
	>>> cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
	>>> print(cday)
	2015-06-01 18:19:59

注意转换后的datetime是没有时区信息的。

## strftime: datetime转换为str
如果已经有了datetime对象，要把它格式化为字符串显示给用户，就需要转换为str，转换方法是通过strftime()实现的，同样需要一个日期和时间的格式化字符串：

	>>> from datetime import datetime
	>>> now = datetime.now()
	>>> print(now.strftime('%a, %b %d %H:%M'))
	Mon, May 05 16:28

formater: https://docs.python.org/3/library/datetime.html

    zone:
        %Z  (empty),UTC,EST,CST (time zone name)
        %z  UTC+0000,-0400,+1030,+0800 (time zone offset)
    weekday:
        %a Sun, Mon, ..., Sat (en_US);
        %A Sunday, Monday, ..., Saturday (en_US);
        %w 0,1,...,6(Saturday)
        %U	00,01,....,53 (Sunday as the first day)
        %W	00,01,....,53 (Monday as the first day)
    year:
        %y  00,01,...09
        %Y  0001,0002,....,2016,...
    month:
        %b Jan, Feb, ..., Dec (en_US);
        %B January, February, ..., December (en_US);
        %m  01,...,12
    day:
        %d  01,...,31
        %j  001,...,366
    Hour:
        %H  00,....,23
        %I  01,02,...,12(12 hour)
        %p  AM,PM

    Minute:
        %M  00,...,59
    Second:
        %S  00,...,59
    Microsecond
        %f  000000,...,999999

### CST

    datetime.now(tz=timezone(timedelta(hours=8))).strftime('%a %b %d %Y %H:%M:%S GMT%z (CST)')
    'Thu Sep 22 2016 17:50:56 GMT+0800 (CST)'



# datetime加减 - timedelta
使用timedelta你可以很容易地算出前几天和后几天的时刻。

	>>> from datetime import datetime, timedelta
	>>> now = datetime.now()
	>>> now
	datetime.datetime(2015, 5, 18, 16, 57, 3, 540997)
	>>> now + timedelta(hours=10)
	datetime.datetime(2015, 5, 19, 2, 57, 3, 540997)
	>>> now - timedelta(days=1)
	datetime.datetime(2015, 5, 17, 16, 57, 3, 540997)
	>>> now + timedelta(days=2, hours=12)
	datetime.datetime(2015, 5, 21, 4, 57, 3, 540997)

# timezone

    from datetime import datetime, timedelta,timezone

## 本地时间转换为UTC时间
本地时间是指系统设定时区的时间，例如北京时间是UTC+8:00时区的时间，而UTC时间指UTC+0:00时区的时间。

    datetime.now().replace(tzinfo=timezone(timedelta(hours=8))) # wrong: locale 时间不会变, 加时区后时间变了
    datetime.utcnow().replace(tzinfo=timezone.utc).__str__()    # ok: locale 时间不会变, 应该当基于utc 时间加时区
    datetime.now(tz=timezone(timedelta(hours=7))).__str__() # ok: 自动基于utc 时间加时区

一个datetime类型有一个时区属性tzinfo，但是默认为None，所以无法区分这个datetime到底是哪个时区，除非强行给datetime设置一个时区：

	>>> from datetime import datetime, timedelta, timezone
	>>> tz_utc_8 = timezone(timedelta(hours=8)) # 创建时区UTC+8:00
	>>> now = datetime.now()
	>>> now
	datetime.datetime(2015, 5, 18, 17, 2, 10, 871012)
	>>> dt = now.replace(tzinfo=tz_utc_8) # 强制设置为UTC+8:00
	>>> dt
	datetime.datetime(2015, 5, 18, 17, 2, 10, 871012, tzinfo=datetime.timezone(datetime.timedelta(0, 28800)))

如果系统时区恰好是UTC+8:00，那么上述代码就是正确的，否则，不能强制设置为UTC+8:00时区。

## 时区转换
utc time 可以用astimezone, naive datetime 则不行(时间戳会变)
astimezone 强制基于utc 时间加时区

我们可以先通过utcnow()拿到当前的UTC时间，再转换为任意时区的时间：

	#拿到UTC时间，并强制设置时区为UTC+0:00:
	>>> utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
	>>> print(utc_dt)
	2015-05-18 09:05:12.377316+00:00

	# astimezone()将转换时区为北京时间:
	>>> bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
	>>> print(bj_dt)
	2015-05-18 17:05:12.377316+08:00

	# astimezone()将转换时区为东京时间:
	>>> tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
	>>> print(tokyo_dt)
	2015-05-18 18:05:12.377316+09:00

	# astimezone()将bj_dt转换时区为东京时间:
	>>> tokyo_dt2 = bj_dt.astimezone(timezone(timedelta(hours=9)))
	>>> print(tokyo_dt2)
	2015-05-18 18:05:12.377316+09:00

时区转换的关键在于，拿到一个datetime时，要获知其正确的时区，然后强制设置时区，作为基准时间。

利用带时区的datetime，通过astimezone()方法，可以转换到任意时区。

> 注：不是必须从UTC+0:00时区转换到其他时区，任何带时区的datetime都可以正确转换，例如上述bj_dt到tokyo_dt的转换。
