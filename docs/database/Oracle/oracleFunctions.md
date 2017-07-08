# orace函数使用

1. 日期格式转化为字符串

		select to_char(sysdate,'yyyy-MM-dd hh24:mi:ss') from dual;
		select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') as nowTime from dual;   //日期转化为字符串   
		select to_char(sysdate,'yyyy') as nowYear   from dual;   //获取时间的年   
		select to_char(sysdate,'mm')    as nowMonth from dual;   //获取时间的月   
		select to_char(sysdate,'dd')    as nowDay    from dual;   //获取时间的日   
		select to_char(sysdate,'hh24') as nowHour   from dual;   //获取时间的时   
		select to_char(sysdate,'mi')    as nowMinute from dual;   //获取时间的分   
		select to_char(sysdate,'ss')    as nowSecond from dual;   //获取时间的秒

2. 字符串格式转化为日期

		select to_date('20170304092103','yyyy-MM-dd hh24:mi:ss') from dual;