LINUX BASH
============

用用户名user，登陆远程服务器host：
	$ ssh user@host
如果本地用户名和远程用户名一致，则可以直接：
	$ ssh host
服务器默认端口22，若要修改：
	$ -p 2222 user@host