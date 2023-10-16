![image-20231012182835903](D:\htb日记\images\Analytics\image-20231012182835903.png)

* 10.10.11.233 
* 10.10.16.3

![image-20231012182906961](D:\htb日记\images\Analytics\image-20231012182906961.png)



namp -sV 结果

22 80

![image-20231012183029508](D:\htb日记\images\Analytics\image-20231012183029508.png)

全端口正在扫描





跳转到  `http://analytical.htb/`

login 发现 http://data.analytical.htb/

发现是 https://www.metabase.com/ 这个cms

![image-20231012190431740](D:\htb日记\images\Analytics\image-20231012190431740.png)

![image-20231012190525881](D:\htb日记\images\Analytics\image-20231012190525881.png)

直接闭着眼睛exploit



![image-20231012191312866](D:\htb日记\images\Analytics\image-20231012191312866.png)



![image-20231012194402328](D:\htb日记\images\Analytics\image-20231012194402328.png)



打了半天，才发现自己在docker 里面，但是环境变量里面的东西都没了

msf 里面打半天，换github上的poc了

![image-20231012214744025](D:\htb日记\images\Analytics\image-20231012214744025.png)



metalytics

An4lytics_ds20223#



ssh 登进去了

![image-20231012215049092](D:\htb日记\images\Analytics\image-20231012215049092.png)



![image-20231012220013347](D:\htb日记\images\Analytics\image-20231012220013347.png)

怪事（

![b89ee22994385886a251c1919375bbd](D:\htb日记\images\Analytics\b89ee22994385886a251c1919375bbd.png)
