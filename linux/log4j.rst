log4j.rootLogger=INFO, stdout, weblog, mail

log4j.appender.stdout=org.apache.log4j.ConsoleAppender

log4j.appender.stdout.Threshold=INFO

log4j.appender.stdout.Target=System.out

log4j.appender.stdout.layout=org.apache.log4j.PatternLayout

log4j.appender.stdout.layout.ConversionPattern=[%-5p] %d{yyyy-MM-dd HH:mm:ss} %x %m%n

::

log4j.appender.weblog=org.apache.log4j.DailyRollingFileAppender

log4j.appender.weblog.Threshold=WARN

log4j.appender.weblog.File=${catalina.home}/logs/web.log

log4j.appender.weblog.DatePattern='.'yyyy-MM-dd

log4j.appender.weblog.layout=org.apache.log4j.PatternLayout

log4j.appender.weblog.layout.ConversionPattern=[%-5p] %d{yyyy-MM-dd HH:mm:ss} %l %x %m%n

::

log4j.appender.mail=org.apache.log4j.net.SMTPAppender

log4j.appender.mail.Threshold=WARN

log4j.appender.mail.BufferSize=1

log4j.appender.mail.SMTPHost=mail.test.com

log4j.appender.mail.SMTPUsername=user@test.com

log4j.appender.mail.SMTPPassword=passwd

log4j.appender.mail.Subject=alert

log4j.appender.mail.From=user@test.com

log4j.appender.mail.To=user1@test.com

log4j.appender.mail.layout=org.apache.log4j.PatternLayout

log4j.appender.mail.layout.ConversionPattern=[%-5p] %d{yyyy-MM-dd HH:mm:ss} %l %x %m%n