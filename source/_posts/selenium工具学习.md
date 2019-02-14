---
title: selenium工具学习
date: 2019-01-13 10:41:42
tags: selenium
categories: python
---


### 如何了解selenium
&emsp;&emsp;最近女朋友的导师实验室发现了一个叫做TimeTree的网站，让他的学生每天23:00（原来是24:00）登录网页预约实验仪器。我帮抢过几次，非常不方便，快到十一点的时候就要守在电脑旁边。就准备写个定时任务的脚本，帮我提交。
&emsp;&emsp;刚开始想通过requests的方法将预约的时间和日期提交请求，但是发现在request header里面含有CSRF-token。了实验室同学，同学推荐使用selenium工具。

<!--more-->
### selenium简介
&emsp;&emsp;Selenium是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。支持的浏览器包括IE（7, 8, 9, 10, 11），Mozilla Firefox，Safari，Google Chrome，Opera等。这个工具的主要功能包括：测试与浏览器的兼容性——测试你的应用程序看是否能够很好得工作在不同浏览器和操作系统之上。测试系统功能——创建回归测试检验软件功能和用户需求。支持自动录制动作和自动生成 .Net、Java、Perl等不同语言的测试脚本。https://baike.baidu.com/item/selenium/18266

### 安装和配置
  1. pip安装  
    `pip install selenium`
  2. 下载驱动  
   [谷歌驱动链接](http://chromedriver.storage.googleapis.com/index.html)
  3. 下载解压后，将chromedriver.exe,移动到Python的安装目录。然后再将Python的安装目录添加到系统环境变量的Path下面。

### 代码链接
```
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from datetime import datetime, timedelta
from time import sleep
import time
import requests
import json

EMAIL = "abc@qq.com"
PASSWORD = "123"

#时间将24小时制乘以2对应。
#例如
#12:00——24；13:30——27
startTime = 25
endTime = 35
#日期
#Day是抢的日期在本月日历（5*7或者4*7的矩阵）排第几，例如在2019年一月的日历中含有12.31一天，则1.1号就记作2。
Day = 17
EventTitle = "CQQ"


SECONDS_PER_DAY = 24 * 60 * 60

def doLogin():
    browser = webdriver.Chrome()
    browser.get('https://timetreeapp.com/signin')
    #登录
    browser.find_element_by_xpath("//input[@placeholder='邮箱地址']").send_keys(EMAIL)
    browser.find_element_by_xpath("//input[@placeholder='8~32 个字符的密码']").send_keys(PASSWORD)
    browser.find_element_by_xpath("//input[@value='登陆']").click()

    #选择日期
    element = WebDriverWait(browser,30).until(EC.presence_of_element_located(
        (By.XPATH,'//*[@id="root"]/div/div/div[1]/div[2]/div/div/div/div[1]/div[3]/div/div[1]/div[2]/div[3]/div[2]/div[3]')))
    # day = browser.find_element_by_xpath('//*[@id="root"]/div/div/div[1]/div[2]/div/div/div/div[1]/div[3]/div/div[1]/div[2]/div[3]/div[2]/div[3]')
    day = browser.find_elements_by_class_name('dayCell')[Day-1]
    # 双击不知道为啥不行
    # ActionChains(browser).double_click(day).perform()
    day.click()
    day.click()
    browser.find_element_by_css_selector(".more.clickable").click()

    #取消全天
    wholeDay = browser.find_elements_by_class_name("inner")[0]
    wholeDay.click()

    #EventTitle
    eventTitle = browser.find_element_by_xpath('//*[@id="root"]/div/div/div[1]/div[2]/div/div/div/div[1]/div[3]/div/div[2]/div[2]/span/div/div/div/div[3]/textarea')
    eventTitle.send_keys(EventTitle)
    #开始时间
    browser.find_elements_by_class_name("bottomButton")[0].click()
    # browser.find_element_by_xpath('//*[@id="root"]/div/div/div[1]/div[2]/div/div/div/div[1]/div[3]/div/div[2]/div[2]/span/div/div/div/div[4]/div[1]/span/div/div/div[2]/a[33]').click()
    browser.find_elements_by_css_selector(".clickable.row")[startTime].click()

    time.sleep(0.5)
    #结束时间
    browser.find_elements_by_class_name("bottomButton")[1].click()
    # browser.find_element_by_xpath('//*[@id="root"]/div/div/div[1]/div[2]/div/div/div/div[1]/div[3]/div/div[2]/div[2]/span/div/div/div/div[4]/div[2]/span/div/div/div[2]/a[41]').click()
    browser.find_elements_by_css_selector(".clickable.row")[endTime].click()

    curTime = datetime.now()
    print("当前时间:%s" % curTime)
    desTime = curTime.replace(hour=23, minute=0, second=0, microsecond=0)
    print("抢AkTa时间:%s" % desTime)
    leftTime = desTime - curTime

    sleepSecondSave = leftTime.total_seconds()
    time.sleep(sleepSecondSave)

    finnalSave = browser.find_elements_by_class_name("withChildren")[2]
    finnalSave.click()

    time.sleep(60)
    browser.close()
    print ("今天已经抢了")


def doFirst():

    curTime = datetime.now()
    print("当前时间:%s" % curTime)
    loginTime = curTime.replace(hour=22, minute=58, second=0, microsecond=0)
    print("登录时间:%s" % loginTime)

    delta = loginTime - curTime
    print(delta)

    sleepSecondLogin = delta.total_seconds()
    time.sleep(sleepSecondLogin)
    doLogin()


if __name__ == "__main__":
    doFirst()
```

#### 参考链接
1.[chromedriver与chrome版本映射表](https://blog.csdn.net/huilan_same/article/details/51896672)
2.[selenium-python中文文档](https://selenium-python.readthedocs.io/)
