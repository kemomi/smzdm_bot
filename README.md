 什么值得买每日签到脚本
===

<p align="center">
    <img src="https://img.shields.io/badge/Created on-2020.10-green"/>
    <img src="https://img.shields.io/badge/Python-3.7-blue"/>
    <img src="https://img.shields.io/badge/Last commit-Dec.-yellow"/>
    <img src="https://img.shields.io/badge/Repo size-35.8kb-red"/>
</p>

# 1. 实现功能
+ `什么值得买`每日签到
+ 通过 `SERVERCHAN`推送简单的运行结果到微信
+ 由 `github actions` 每日7点定时运行

# 2. 使用方法
1. Fork [此仓库项目](https://github.com/stark666/smzdm_bot) > 点击右上角fork按钮即可，欢迎点`star`~
2. Secret新增`SMZDM_COOKIE`，填入浏览器调试模式从[什么值得买官网](https://www.smzdm.com/)cookie信息, 不懂可看下面的其它
3. （可选）Secret新增`SERVERCHAN_SECRETKEY`，获取方法可看这篇文章[@ruicky教程](https://ruicky.me/2020/06/05/jd-sign/)
4. fork后必须修改一下文件，才能执行定时任务, 可修改 `README.MD`, 添加一个空格


# 3. 其它
## 3.1 cookie获取方法
+ 首先使用chrome浏览器，访问[什么值得买官网](https://www.smzdm.com/)， 登陆账号
+ Windows系统可按 `F12` 快捷键打开开发者工具, Mac 快捷键 `option + command + i`
+ 选择开发者工具Network，刷新页面 ,选择第一个`www.smzdm.com`, 找到`Requests Headers`里的`Cookie`。

## 3.2 更改执行时间
在 `.github/main.yml`中，找到
```yml
- cron: '0 0 * * *'
```
语法同crontab，具体可百度，为美区时间，加8小时为中国时间

# 4. cron表达式
我们就从一个简单的cron表达式例子开始，cron = 0 0 2 * * ? ，这个表达式的含义是每天凌晨两点执行一次任务。可以看到cron表达式是一个字符串，以5或者6个空格隔开(示例中是被5个空格隔开)。字符串被切割为6个或者7个域，每个域都代表不同的含义。从左到右依次为"秒 分 时 日 月 星期几 年" ，其中年不是必须的的，所以cron表达式有两种形式：

{Seconds} {Minutes} {Hours} {DayofMonth} {Month} {DayofWeek} {Year}或
{Seconds} {Minutes} {Hours} {DayofMonth} {Month} {DayofWeek}
各个域的含义如下：


每个域都可以用数字表示，但是还可以出现如下特殊字符。
</p>
* : 表示匹配该域的任意值。比如Minutes域使用*，就表示每分钟都会触发。


-: 表示范围。比如Minutes域使用 10-20，就表示从10分钟到20分钟每分钟都会触发一次。


,: 表示列出枚举值。比如Minutes域使用1,3，就表示1分钟和3分钟都会触发一次。


/: 表示间隔时间触发(开始时间/时间间隔)。例如在Minutes域使用 5/10，就表示从第5分钟开始，每隔10分钟触发一次。


?: 表示不指定值。简单理解就是忽略该字段的值，直接根据另一个字段的值触发执行。


#: 表示该月第n个星期x(x#n)，仅用星期域。如：星期：6#3，表示该月的第三个星期五。


L : 表示最后，是单词"last"的缩写（最后一天或最后一个星期几）；仅出现在日和星期的域中。用在日则表示该月的最后一天，用在星期则表示该月的最后一个星期。如：星期域上的值为5L，则表示该月最后一个星期的星期四。在使用'L'时，不要指定列表','或范围'-'，否则易导致出现意料之外的结果。


W: 仅用在日的域中，表示距离当月给定日期最近的工作日（周一到周五），是单词"weekday"的缩写。


比如："4W"表示距离4号最近的工作日（当月的）触发；
</p>
（1）当4号就是工作日时，则表示当天触发；当4号为周六时，则表示3号（周五）触发；

（2）当4号为周日时，则表示在5号（周一）触发；

比如："1W"表示距离1号最近的工作日触发事件，但是，该工作日只算当月的。假如当月1号是周六，则"1W"表示在当月3号（周一）触发。就算上个月的最后一天是工作日，也不会触发。

LW: ‘L’和'W'可以一起组合在日字段使用。表示当月的最后一个工作日触发事件。
## 取值说明
DayofMonth：
可以用数字1-31 中的任一个值，但要注意一些特别的月份。

Month：
一年中的月份，可以用0-11 或用字符串 “JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV and DEC” 表示。

DayofWeek：
表示星期几，可以用数字1-7（1=星期日），或者用字符串"SUN, MON, TUE, WED, THU, FRI and SAT"来表示。

## 常用cron表达式
*/10 * * * * ? 每隔10秒执行一次

0 */5 * * * ? 每隔5分钟执行一次

0 2,22,32 * * * ? 在2分、22分、32分执行一次

0 0 4-8 * * ? 每天4-8点整点执行一次

0 0 2 * * ? 每天凌晨2点执行一次

0 0 2 1 * ? 每月1号凌晨2点执行一次

