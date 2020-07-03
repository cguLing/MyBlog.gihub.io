---
layout: post
title: 'Java_web+MySQL演示'
date: 2020-07-2 00:58:06 +0800
author: Jekyll
color: rgb(255,210,32)
cover: '../assets/welcome.jpeg'
tags: [eclipse,Java_web,mysql,tomcat] 
subtitle: '视频演示简单说明【资源仅供学习】'
---
<!-- <iframe type="text/html" width="90%" height="385" src="/assets/Web_Mysql/first.mp4" frameborder="0"  ></iframe> -->
# 前情提要
- 实现Java与MySQL的简单互动  
   - 数据库的连接  
   - 代码实现MySQL命令  
   - 数据获取与传输  
- 实现JSP与Java的简单互动  
   - 弹窗  
   - 获取后端信息  
   - 展示数据表  
   - 操作增删查改  

###### 	结合HTML进行JavaEE框架中的JSP、Servlet编程。  
# 环境及工具
- Windows Server 2019  
- Java SE 8u251  
   - [下载页面](https://www.oracle.com/java/technologies/javase-downloads.html)  
   - 记住安装路径便于配置环境变量  
- Eclipse IDE 
   - [下载页面](https://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/oxygen1a)
   - 下载后解压为文件夹放到合适的位置如: `C:\Program Files` 即可
   - 将文件夹内的应用程序快捷方式到桌面方便使用  
- MySQL/phpStudy【推荐phpStudy】  
    - MySQL[下载页面](https://www.mysql.com/cn/downloads/)  
       - 找到MySQL Community Edition (GPL)  
       - 选择MySQL Installer下载  
    - phpStudy V8.0[下载页面](https://m.xp.cn/)  

![Alt text](/assets/Web_Mysql/1.png  "phpStudy"){:height="50%" width="60%"}  
![Alt text](/assets/Web_Mysql/2.png  "phpStudy"){:height="50%" width="60%"}  
![Alt text](/assets/Web_Mysql/3.png  "phpStudy"){:height="50%" width="60%"}  
- Tomcat 7.0  
    - [下载页面](https://tomcat.apache.org/download-70.cgi)  
    - 下载后解压为文件夹放到合适的位置如: `C:\Program Files` 即可【可能需要配置环境变量】  
    - 控制台: `cd c:\Program Files\apache-tomcat-xxxx(此处是相应的版本号)\bin` -> `startup`  
    - 浏览器输入: `http://localhost:8080` 出现Tomcat页面，安装成功  
- 为Eclipse配置Tomcat
    - 打开Eclipse，单击“window”菜单，选择下方的“Preferences”；  
    - 找到Server下方的Runtime Environment，单击右方的Add按钮；  
    - 选择已经成功安装的Tomcat版本，单击Next；  
    - 设置Tomcat的安装目录，找到刚才压缩包路径，然后点击ok。  

# 初步说明
实现web页面操作数据库的增删查改  
- 连接数据库:  
{% highlight ruby %}
    public static final String driver="com.mysql.jdbc.Driver";
    public static final String url="jdbc:mysql://localhost:3306/webmysql1?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8";
    public static final String username="root"; #MySQL用户
    public static final String password="xxxxxx"; #密码
    public static Connection con=null;
    static{
        try {
            Class.forName(driver); #驱动
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    public static Connection getCon(){
        if(con == null){
            try {
                con = DriverManager.getConnection(url, username, password); #连接数据库，获取对象
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return con;
    }
    public static void main(String args[]){
            new DBUtil().getCon();
    }
{% endhighlight %}

- 代码实现MySQL命令:  
{% highlight ruby %}
    public void create_T(Connection con,String a) throws Exception{
		String sql="create table "+a;
		Statement Ta=con.createStatement();#获得连接上的数据库对象
		Ta.executeUpdate(sql);/#执行SQL语句
	}
{% endhighlight %}

- 通过表类将数据中转存储，以此为媒介实现前端与数据库间的数据传输
{% highlight ruby %}
public class User {
	private String username;
    private String password;
    public String getUserName() { 
        return username;
    }
    public void setUserName(String username) {
        this.username = username;
    }
    public String getPassword() { 
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }
    public void UserShow(String username,String password) {
    	this.username=username;
    	this.password=password;
    }
}
{% endhighlight %}  
- 前端JSP文件展示数据表【Markdown无法高亮所以就用图片了】  
  
![Alt text](/assets/Web_Mysql/4.png  "phpStudy"){:height="60%" width="60%"}
### 演示视频
<center>
<!-- <video width="90%" height="385" src="/assets/Web_Mysql/first.mp4" type="video/mp4" 
    preload="auto" controls="controls" loop="-1">
</video> -->
<iframe type="text/html" width="100%" height="600" src="//player.bilibili.com/player.html?aid=286185997&bvid=BV1pf4y117S5&cid=208575226&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</center>

# 进阶说明
- 前期设计
   - 需求分析
   内部需求:   
   ![Alt text](/assets/Web_Mysql/10.png  "analysis"){:height="70%" width="80%"}
   外部需求:  
   ![Alt text](/assets/Web_Mysql/11.png  "analysis"){:height="50%" width="80%"}
   - ER图  
   ![Alt text](/assets/Web_Mysql/5.png  "ER")
   - 数据表关系  
   ![Alt text](/assets/Web_Mysql/6.png  "code")
   - 代码框架  
   ![Alt text](/assets/Web_Mysql/7.png  "code")
- 实际情况  
   - ER图
   ![Alt text](/assets/Web_Mysql/8.png  "ER")
   - 数据表关系
   ![Alt text](/assets/Web_Mysql/9.png  "relation")
   - 范式分析  
      - 员工信息表：员工编号、员工姓名、性别、身份证号、性别、手机号、邮箱、家庭住址、薪水；  
      - 员工权限表：员工编号、登陆系统、账号、密码；  
      - 部门信息表：部门编号、部门主管、部门描述；  
      - 部门与员工信息表：职位编号、所属部门、职位名称；  
      - 项目表：项目编号、负责部门编号、项目负责人、客户编号、项目名称、内容简述、创建时间、项目状态、完成时间；  
      - 客户信息表：客户编号、姓名、性别、电话、邮箱、身份证号；  
    在以上关系模式中，不存在传递依赖和部分依赖，都为第三范式。  

>>前期设计和实际情况差别还是蛮大的，难过的我眼泪流下来  

- 建表命令【具体见`建表命令.txt`】 
主键自增并初始化   
{% highlight ruby %}
create table user(
empID int auto_increment not null primary key,
password varchar(255) not null,
isAdmin char(1) not null)AUTO_INCREMENT=1000;
{% endhighlight %}  
- 前端报错/提示弹窗  
- post方法提交表单数据  
- 后端处理表单数据  
- 后端提交数据给前端  
......【详情见`仅供学习`】  
### 演示视频
<center>
<!-- <video width="90%" height="385" src="/assets/Web_Mysql/second.mp4" type="video/mp4" 
    preload="auto" controls="controls" loop="-1">
</video> -->
<iframe type="text/html"  width="100%" height="600" src="//player.bilibili.com/player.html?aid=286235086&bvid=BV1Uf4y117wR&cid=208575966&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</center>

## &diams;问题说明
- 因为eclipse是需要不停安装依赖包来运行项目的软件【这一点上比不上Myeclipse】所以很有可能是少依赖包了，推荐一个下载jar包的网站[MVNrepository](https://mvnrepository.com)  
- 但是因为我把依赖包都已经放到文件夹里了，所以出现问题很有可能是因为路径问题  
&emsp;右键工程文件，打开路径设置  
![Alt text](/assets/Web_Mysql/12.png  "jar"){:height="70%" width="70%"}
&emsp;对应进行增加删除  
![Alt text](/assets/Web_Mysql/13.png  "jar"){:height="70%" width="70%"}

>>>偷摸放个资源: [仅供学习][source]  
>>级联删除是没有完善的状态，有兴趣的把逻辑理清楚之后可以改改

[source]: https://github.com/cguLing/2020-7-2
