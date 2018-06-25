---
title: Logback发送报警邮件
categories: 监控
date: 2017-06-11 20:54:37
tags:
---

对于小公司，无力搭建或购买监控跟踪系统，可以使用logback的邮件功能，将监听到的日志以邮件的形式来通知

下面是使用的步骤：
1. 申请一个可以开通SMTP服务的邮箱做为发件方
2. 登陆邮箱开通SMTP服务并设置授权码
3. 以授权码作为logback邮件的密码

注意：Java环境里，需确保logback的版本大于1.1.7

下面是详细配置：
```
<property name="smtpHost" value="smtp.163.com" />
<property name="smtpPort" value="25" />
<property name="username" value="****@163.com" />
<property name="password" value="***" />
<property name="SSL" value="false" />
<property name="email_to" value="TARGET_EMAIL" />
<property name="email_from" value="SEND_EMAIL" />
<property name="email_subject" value="【Error】: %m" />

<appender name="EMAIL" class="ch.qos.logback.classic.net.SMTPAppender">
    <smtpHost>${smtpHost}</smtpHost>
    <smtpPort>${smtpPort}</smtpPort>
    <username>${username}</username>
    <password>${password}</password>
    <SSL>${SSL}</SSL>
    <asynchronousSending>false</asynchronousSending>
    <to>${email_to}</to>
    <from>${email_from}</from>
    <subject>${email_subject}</subject>
    <!--<asynchronousSending>true</asynchronousSending> 无效--> 
    <layout class="ch.qos.logback.classic.html.HTMLLayout" >
        <pattern>%d{HH:mm:ss}%-5p%replace(%caller{1}){'(Caller\+0\s*at.)|\r|\n', ''}%m</pattern>
    </layout>
    <filter class="ch.qos.logback.classic.filter.LevelFilter">
        <level>ERROR</level>
        <onMatch>ACCEPT</onMatch>
        <onMismatch>DENY</onMismatch>
    </filter>
</appender>

<root level="info">
    <appender-ref ref="EMAIL"/>
</root>
```
