---
layout: post
title: 'C/S架构NIO文件管理系统【上】'
date: 2020-07-28 12:58:06 +0800
author: YULIA
color: rgb(102,153,204)
cover: '../assets/CS_TransFile_1/0.png'
tags: [C/S,Java,NIO] 
subtitle: '面板设计及基础C/S互动【代码解析】'
---
# 面板设计
  [**`点击查看大图`**](/assets/CS_TransFile_1/1.png)
 [![1.png](/assets/CS_TransFile_1/1.png){:height="50%" width="70%"}](/assets/CS_TransFile_1/1.png)

设计过程：  
- 整个系统需要哪些功能？  
    登陆，注册，搜索文件，上传文件，浏览文件目录，选择文件下载，浏览自己上传的文件并可以选择进行下载/删除/重命名  
- 实现功能的面板需要几个？  
    6个——登陆、注册、功能面板、文件目录、文件管理、下载
- 面板之间的跳转通过哪个部件？  
    按钮
- 每个面板内含有哪些元素？  
   - 基本上是Label、Button、TextFiled  
   - 特殊的有PasswordField、FileChooser、ProgressBar、CheckBox、ScrollPane  
- 每个元素对应的功能是什么？  
   - Label：说明文字  
   - Button：进行操作  
   - TextFiled：文本框，输入数据  
   - PasswordField：密码输入框，隐藏明文密码  
   - FileChooser：文件选择窗口，上传文件的选择/下载路径的选择  
   - ProgressBar：进度条，上传/下载/删除的完成进度  
   - CheckBox：多选框，选择文件目录  
   - ScrollPane：滚动条  
- 操作后不同结果的提示是什么？  
   - 成功——提示成功  
   - 失败——提示失败及可能原因  
   - 进度——如删除则显示进度条  

# 数据库设计
> 此篇主要是面板设计，与后端及server的互动不多，所以数据库设计并不完整  
> 下篇会将所有C/S互动补充完整  

- 结构  
![Alt text](/assets/CS_TransFile_1/2.png){:height="50%" width="60%"}  
- 用户表  
![Alt text](/assets/CS_TransFile_1/3.png){:height="50%" width="60%"}  

# 演示视频
<center>
<iframe type="text/html" width="100%" height="600" src="//player.bilibili.com/player.html?aid=669031714&bvid=BV1ma4y1E7bu&cid=218087024&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</center>

# 环境
- Windows Server 2019  
- phpStudy V8.0[下载页面](https://m.xp.cn/)  
    - 启动Apache
    - 启动Mysql  
- Client端：VScode
   - 配置Java：Java SE 8u251  
      - [下载页面](https://www.oracle.com/java/technologies/javase-downloads.html)  
      - 记住安装路径便于配置环境变量  
   - 通过VScode自身安装Java  
      - 随便打开一个Java文件  
      - 右下角弹出Java安装进行安装即可  
- Server端：Eclipse IDE  
   - [下载页面](https://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/oxygen1a)
   - 下载后解压为文件夹放到合适的位置如: `C:\Program Files` 即可
   - 将文件夹内的应用程序快捷方式到桌面方便使用  
# 文件目录架构  
- client
  **`点击查看大图`**
 [![4.png](/assets/CS_TransFile_1/4.png){:height="50%" width="60%"}](/assets/CS_TransFile_1/4.png)
- server
![Alt text](/assets/CS_TransFile_1/5.png){:height="50%" width="60%"}  

# Client
## 面板
### 登陆面板举例  
```java
import java.awt.Font;

import java.awt.Image;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPasswordField;
import javax.swing.JTextField;

public class admin extends JFrame{
   private int count=0;
   private JButton bt1;//登陆按钮
   private JButton bt2;//注册按钮
   private JLabel jl_1;//登录的版面
   private JFrame jf_1;//登陆的框架
   private JTextField jtext1;//用户名
   private JPasswordField jtext2;//密码
   private JLabel jl_admin;
   private JLabel jl_password;
   public admin (String a,String b){//初始化登陆界面
       Font font =new Font("黑体", Font.PLAIN, 15);//设置字体
       jf_1=new JFrame("登陆界面");
       jf_1.setSize(400, 320);
       //给登陆界面添加背景图片
       ImageIcon bgim = new ImageIcon(admin.class.getResource("back.png")) ;//背景图案
       bgim.setImage(bgim.getImage().
                                    getScaledInstance(bgim.getIconWidth(),
                                                      bgim.getIconHeight(), 
                                                      Image.SCALE_DEFAULT));  
       jl_1=new JLabel();
       jl_1.setIcon(bgim);
       jl_admin=new JLabel("用户名");
       jl_admin.setBounds(20, 50, 60, 50);
       jl_admin.setFont(font);
       jl_password=new JLabel("密  码");
       jl_password.setBounds(20, 120, 60, 50);
       jl_password.setFont(font);
       bt1=new JButton("登陆");         //更改成loginButton
       bt1.setBounds(70, 210, 100, 50);
       bt1.setFont(font);
       bt2=new JButton("注册");
       bt2.setBounds(230, 210, 100, 50);
       bt2.setFont(font);
       //加入文本框
       jtext1=new JTextField(a);
       jtext1.setBounds(110, 50, 250, 40);
       jtext1.setFont(font);
       jtext2=new JPasswordField(b);//密码输入框
       jtext2.setEchoChar('*');//设置密文显示
       jtext2.setBounds(110, 120, 250, 40);
       jtext2.setFont(font);
       //元素加入面板
       jl_1.add(jtext1);
       jl_1.add(jtext2);
       jl_1.add(jl_admin);
       jl_1.add(jl_password);
       jl_1.add(bt1);
       jl_1.add(bt2);
       jf_1.add(jl_1);
       jf_1.setVisible(true);
       jf_1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
       jf_1.setLocation(300,200);
       //登陆点击事件
       ActionListener bt1_ls=new ActionListener() {
           @Override
           public void actionPerformed(ActionEvent arg0) {
               String admin=jtext1.getText();
               char[] password=jtext2.getPassword();
               String str=String.valueOf(password); //将char数组转化为string类型
               //判断用户名和密码
               if(admin.equals("Uling")&&str.equals("22dd66cc")){
                   System.out.println(admin);
                   System.out.println(str);
                   new mainLayout(admin);//跳转界面
                   jf_1.dispose();//销毁当前界面
               }
               else {
                   count++;
                   System.out.println("error");
                   JOptionPane.showMessageDialog(null, "登录失败");
                   if(count==3){//密码输入错三次直接退出
                        JOptionPane.showMessageDialog(null, "即将退出，再见");
                        jf_1.dispose();
                   }
               }
           }
       };
       bt1.addActionListener(bt1_ls);
       bt2.addActionListener(new ActionListener(){//对注册按钮添加监听事件
           @Override
           public void actionPerformed(ActionEvent arg0) {
               new register();//进入都到注册窗体中
               jf_1.dispose();
           }
           
       });
   }
   public static void main(String[] args) {
       //初始化登陆界面
       new admin("Uling","22dd66cc");
   }
}
```
其他面板详细举例见《P2P:局域网内文件传输【带界面】》  
### 进度条的使用
```java
public class Down extends JFrame{
    public Down(){
        JFrame jf=new JFrame();
        jf.setTitle("下载界面");
        jf.setLayout(null);
        jf.setSize(400,300);
        jf.setLocation(500,200);
        jf.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);//关闭选项
        JPanel fun = new Fun(jf); 
        fun.setBounds(20,10,360,200);
        fun.setBorder(BorderFactory.createTitledBorder("")); 
        jf.add(fun);
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        new Down();
    }
}
class Fun extends JPanel implements ActionListener {
    JButton download;
    Timer timer;
    JProgressBar jpb;
    JFrame jf;
    public Fun(JFrame jf) {
        this.jf=jf;
        this.setLayout(null);
        setSize(360, 200);//设置宽、高
		timer = new Timer(50, this); // 创建一个计时器，计时间隔为50毫秒
        download=new JButton("开始下载");
        download.setBounds(20,115,100,30);
        jpb = new JProgressBar();
        jpb.setOrientation(JProgressBar.HORIZONTAL);//设置进度条的方向：平行
        jpb.setMinimum(0);//最小值为0
        jpb.setMaximum(100);//最大值为100
        jpb.setValue(0);//赋初始值
        jpb.setStringPainted(true);//true：进度条呈现进度字符串
        jpb.setBounds(130,120,200,20);
        this.add(download);
        this.add(jpb);
        download.addActionListener(this);
    }
    @Override
    public void actionPerformed(ActionEvent e) {
		if (e.getSource() == timer) {//监听到有动作发生的某个控件（定时器）
			int value = jpb.getValue();//返回进度条的当前值
			jpb.setValue(0);//将进度条回复0值
			if (value < 100) {//若当前进度未满
				value++;//自加
				jpb.setValue(value);//将进度条重新赋值
			} else {
				timer.stop();//进度条已满则停止Timer，使它停止向其监听器发送动作事件
			}
		}
        else if ((JButton) e.getSource() == download){//文件下载
            download.setEnabled(false);
            if(path==null){//错误提示
                JOptionPane.showMessageDialog(null, "未选择保存路径","错误",JOptionPane.ERROR_MESSAGE);
                download.setEnabled(true);
            }
            else{
                try {
                    timer.start();
                    /*下载开始。。。操作*/
                    }
                }catch (IOException q) {
                    System.out.println("异常");
                    download.setEnabled(true);
                }
            }
        }
    }
}
```
### 文件选择窗口
- 文件选择  
```java
public class Up extends JFrame{
    public Up(){
        JFrame jf=new JFrame();
        jf.setTitle("Up");
        jf.setLayout(null);
        jf.setSize(400,300);
        jf.setLocation(500,200);
        jf.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);//关闭选项
        JPanel fun = new Fun(jf); 
        fun.setBounds(20,10,360,200);
        fun.setBorder(BorderFactory.createTitledBorder("")); 
        jf.add(fun);
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        new Up();
    }
}
class Fun extends JPanel implements ActionListener {
    JFileChooser jfc;//文件选择窗口
    JTextField file_path;
    JButton look;
    JFrame jf;
    String path,name;
    public Fun(JFrame jf) {
        this.jf=jf;
        this.setLayout(null);
        setSize(360, 200);//设置宽、高
        jfc = new JFileChooser();
        file_path=new JTextField("待发送文件:");
        file_path.setBounds(40,60,200,30);
        look= new JButton("浏览");
        look.setBounds(240,60,80,30);
        this.add(download_path);
        this.add(file_path);
        this.add(look);
        look.addActionListener(this);
    }
    @Override
    public void actionPerformed(ActionEvent e) {
        int result;
		if ((JButton) e.getSource() == look){//浏览按钮被按下
			jfc.setApproveButtonText("确定");//文件选择窗口中的 approvebutton 内使用的文本
			jfc.setDialogTitle("选择文件窗口");//文件选择窗口中的标题栏
			result = jfc.showOpenDialog(this);
			//null———显示在当前电脑显示器屏幕的中央
			//this———显示在当前你编写的程序屏幕中央
			//返回一个DialogResult，对应你按的按钮【如：单击“关闭”按钮会隐藏窗体，并将DialogResult属性设置为DialogResult.Cancel  】
			if (result == JFileChooser.APPROVE_OPTION) // 当用户按下文件选择窗口中的确定
			{
				path = new String(jfc.getSelectedFile().getPath());//文件路径
				//getSelectedFile()返回选中的文件
				//getPath()将此抽象路径名转换为一个路径名字符串
				name= new String(jfc.getSelectedFile().getName());//文件名
                file_path.setText("文件:" + name + "待上传！");
            }
        }
    }
}
```
- 文件路径选择  
```java
public class Down extends JFrame{
    public Down(){
        JFrame jf=new JFrame();
        jf.setTitle("Down");
        jf.setLayout(null);
        jf.setSize(400,300);
        jf.setLocation(500,200);
        jf.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);//关闭选项
        JPanel fun = new Fun(jf); 
        fun.setBounds(20,10,360,200);
        fun.setBorder(BorderFactory.createTitledBorder("")); 
        jf.add(fun);
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        new Down();
    }
}
class Fun extends JPanel implements ActionListener {
    JFileChooser jfc;//文件选择窗口
    JLabel download_path;
    JTextField file_path;
    JButton look;
    File dir;
    String url="127.0.0.1";
    JFrame jf;
    public Fun(JFrame jf) {
        this.jf=jf;
        this.setLayout(null);
        setSize(360, 200);//设置宽、高
        jfc = new JFileChooser();
        download_path=new JLabel("下载路径:");
        download_path.setBounds(20, 20, 60, 30);
        file_path=new JTextField("文件存储位置");
        file_path.setBounds(40,60,200,30);
        look= new JButton("浏览");
        look.setBounds(240,60,80,30);
        this.add(download_path);
        this.add(file_path);
        this.add(look);
        look.addActionListener(this);
    }
    @Override
    public void actionPerformed(ActionEvent e) {
        File path=null;
		if ((JButton) e.getSource() == look){//选择文件后显示状态实时更新为对应的选择路径
            jfc.setFileSelectionMode(JFileChooser.FILES_AND_DIRECTORIES );
            jfc.showDialog(new JLabel(), "选择");
            path=jfc.getSelectedFile();
            if(path.isDirectory()){
				file_path.setText(path.getPath());//显示文件路径
				dir = new File(path.getPath());
            }else if(path.isFile()){
                
                JOptionPane.showMessageDialog(null, "所选保存路径不正确","警告",JOptionPane.WARNING_MESSAGE);
            }
        }
    }
}
```
### 自动生成多选框
```java
public class AllContent extends JPanel implements ItemListener {
    //哈希map动态保存多选框和对应文字
    private HashMap<JCheckBox, String> map = new HashMap<>();
    static List<String> content = new ArrayList<>();
    private static File file = new File("AllConten.txt");

    private static void GetContent() {
        try {
            if (!file.isFile()) {
                file.createNewFile();
            }
            /** 连接server，请求全部文件目录写入AllContent.txt中 */
        } catch (Exception e) {
            System.out.println("文件异常");
        }
    }
    public AllContent() {
        JCheckBox checkBox;//多选框
        JPanel checkPanel = new JPanel(new GridLayout(0, 1));//文件目录部分
        try {
            //读取保存文件目录的文件
            BufferedReader br = new BufferedReader(new FileReader(file));
            String line = null;
            int i = 0;
            /*逐行读取——一行一个文件名
            生成多选框及对应文字
            集合存入map*/
            while ((line = br.readLine()) != null) {
                StringBuilder sb = new StringBuilder();
                String a = line;
                sb.append(a);
                checkBox = new JCheckBox(sb.toString());
                checkBox.setName("CheckBox" + i);
                checkBox.addItemListener(this);
                map.put(checkBox, a);
                checkPanel.add(checkBox);
                i++;
            }
            br.close();
        } catch (FileNotFoundException e) {
            System.out.println("文件异常");
        }catch (IOException e) {
            System.out.println("读取异常");
        }
        add(checkPanel);//文件目录部分加入面板
    }
    //反应多选框的选中状态
    public void itemStateChanged(ItemEvent e) {
        JCheckBox source = (JCheckBox) e.getItemSelectable();
        if (e.getStateChange() == ItemEvent.SELECTED) {//选中
            String list = map.get(source);
            content.add(list);//文件名加入列表
        }
        if (e.getStateChange() == ItemEvent.DESELECTED) {//未选中
            String list = map.get(source);
            Iterator<String> iterator = content.iterator();
            while (iterator.hasNext()) {//从列表中删除文件名
                String s = iterator.next();
                if (s.equals(list)) {
                    iterator.remove();
                }
            }
        }
    }

    private static void Initial() {
        GetContent();
        JFrame jf = new JFrame("文件列表");
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.setLayout(null);
        jf.setSize(300, 350);
        jf.setLocation(200, 200);
        JComponent newContentPane = new AllContent();
        newContentPane.setBorder(BorderFactory.createTitledBorder("目录"));
        newContentPane.setOpaque(true);
        JScrollPane scroll = new JScrollPane(newContentPane);
        scroll.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);// 垂直滚动
        scroll.setBounds(20, 50, 250, 240);
        jf.getContentPane().add(scroll);
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        javax.swing.SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                Initial();
            }
        });
    }
}
```
### 滚动条的使用
```java
private static void Initial() {
        GetContent();
        JFrame jf = new JFrame("文件列表");
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.setLayout(null);
        jf.setSize(300, 350);
        jf.setLocation(200, 200);
        JComponent newContentPane = new AllContent();//组件
        newContentPane.setBorder(BorderFactory.createTitledBorder("目录"));//外框
        newContentPane.setOpaque(true);//设置控件是否透明的：true表示不透明,false表示透明
        JScrollPane scroll = new JScrollPane(newContentPane);//滚动条所属组件
        scroll.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);// 垂直滚动
        scroll.setBounds(20, 50, 250, 240);//滚动条基本大小范围
        jf.getContentPane().add(scroll);
        jf.setVisible(true);
}
```
## 后端处理
>> 将数据传输部分的数据改成控制台输入即可进行轮询，与server形成会话，而不是一次性操作  

### 连接server，处理数据
```java
public class C_Register {
    public static boolean register(String M) {
        try {
            //初始化客户端
            SocketChannel socket = SocketChannel.open();
            socket.configureBlocking(false);
            Selector selector = Selector.open();
            //注册连接事件
            socket.register(selector, SelectionKey.OP_CONNECT);
            //发起连接
            socket.connect(new InetSocketAddress("127.0.0.1", 555));
            //开启数据输入监听
            new ChatThread(M,selector, socket).start();
            Calendar ca = Calendar.getInstance();
            //轮询处理
            while (true) {
                if (socket.isOpen()) {
                    //在注册的键中选择已准备就绪的事件
                    selector.select();
                    //已选择键集
                    Set<SelectionKey> keys = selector.selectedKeys();
                    Iterator<SelectionKey> iterator = keys.iterator();
                    //处理准备就绪的事件
                    while (iterator.hasNext()) {
                        SelectionKey key = iterator.next();
                        //删除当前键，避免重复消费
                        iterator.remove();
                        //连接
                        if (key.isConnectable()) {
                            //在非阻塞模式下connect也是非阻塞的，所以要确保连接已经建立完成
                            while (!socket.finishConnect()) {
                                System.out.println("连接中");
                            }
                            socket.register(selector, SelectionKey.OP_READ);
                        }
                        //监听到数据传人，注册OP_WRITE,然后将消息附在attachment中
                        if (key.isWritable()) {
                            //发送消息给服务端
                            socket.write((ByteBuffer) key.attachment());
                            /*
	                            已处理完此次输入，但OP_WRITE只要当前通道输出方向没有被占用
	                            就会准备就绪，select()不会阻塞（但我们需要控制台触发,在没有输入时
	                            select()需要阻塞），因此改为监听OP_READ事件，该事件只有在socket
	                            有输入时select()才会返回。
                            */
                            socket.register(selector, SelectionKey.OP_READ);
                            System.out.println("==============" + ca.getTime() + " ==============");
                        }
                        //处理输入事件
                        if (key.isReadable()) {
                            ByteBuffer byteBuffer = ByteBuffer.allocate(1024 * 4);
                            int len = 0;
                            //捕获异常，因为在服务端关闭后会发送FIN报文，会触发read事件，但连接已关闭,此时read()会产生异常
                            try {
                                if ((len = socket.read(byteBuffer)) > 0) {
                                    System.out.println("the message from server:\t");
                                    String receive=new String(byteBuffer.array(), 0, len);
                                    System.out.println(receive);
                                    if(receive.equals("ACK")){
                                        return true;
                                    }
                                    else if(receive.equals("ERRO")){
                                        return false;
                                    }
                                }
                            } catch (IOException e) {
                                System.out.println("ERRO! closing client.........");
                                key.cancel();
                                socket.close();
                            }
                            System.out.println("=========================================================");
                        }
                    }
                } else {
                    break;
                }
            }
        } catch (IOException e) {
            System.out.println("客户端异常，请重启！");
        }
        return true;
    }
    public static void main(String[] args) {
        boolean re=register("register:CG:22dd66cc");
    }
}
```
### 数据传输
```java
public class ChatThread extends Thread {
    private Selector selector;
    private SocketChannel socket;
    private String M;

    public ChatThread(String M,Selector selector, SocketChannel socket) {
        super();
        this.selector = selector;
        this.socket = socket;
        this.M=M;
    }
    @Override
    public void run() {
        try {
            //等待连接建立
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //Scanner scanner = new Scanner(System.in);
        //System.out.println("请输入您要发送给服务端的消息");
        //while (scanner.hasNextLine()) {
            String s = M;//scanner.nextLine();
            try {
                //用户已输入，注册写事件，将输入的消息发送给客户端
                socket.register(selector, SelectionKey.OP_WRITE, ByteBuffer.wrap(s.getBytes()));
                //唤醒之前因为监听OP_READ而阻塞的select()
                selector.wakeup();
            } catch (ClosedChannelException e) {
                e.printStackTrace();
            }
        //}
    }
}
```
# Server
## Mysql
```java
public class Start {
	private static String url="jdbc:mysql://localhost:3306/trans_file?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8";//定位数据库
	private static String user="root";//用户名
	private static String password="22dd66cc";//密码
	private static String driver="com.mysql.cj.jdbc.Driver";//jar包驱动
	public Connection getCon() throws Exception{
		Class.forName(driver);//加载驱动
		Connection con=DriverManager.getConnection(url, user, password);//连接数据库
		return con;
	}
	public void close(Connection con) throws Exception{
		if(con!=null){
			con.close();
        }    
	}
}
```
## NIOServer
```java
public class ServerTest {
	public static void main(String[] args) throws Exception {
    	Start db=new Start();
    	Connection conn=db.getCon();
        try {
            //服务初始化
            ServerSocketChannel serverSocket = ServerSocketChannel.open();
            //设置为非阻塞
            serverSocket.configureBlocking(false);
            ServerSocket ss=serverSocket.socket();
            //绑定端口
            InetSocketAddress addr=new InetSocketAddress(555);
            ss.bind(addr);
            //serverSocket.bind(new InetSocketAddress("localhost", 443));
            //注册OP_ACCEPT事件（即监听该事件，如果有客户端发来连接请求，则该键在select()后被选中）
            Selector selector = Selector.open();
            serverSocket.register(selector, SelectionKey.OP_ACCEPT);
            Calendar ca = Calendar.getInstance();
            System.out.println("服务端开启了");
            System.out.println("=========================================================");
            //轮询服务
            while (true) {
                //选择准备好的事件
                selector.select();
                //已选择的键集
                Iterator<SelectionKey> it = selector.selectedKeys().iterator();
                //处理已选择键集事件
                while (it.hasNext()) {
                    SelectionKey key = it.next();
                    //处理掉后将键移除，避免重复消费(因为下次选择后，还在已选择键集中)
                    it.remove();
                    //处理连接请求
                    if (key.isAcceptable()) {
                        //处理请求
                        SocketChannel socket = serverSocket.accept();
                        socket.configureBlocking(false);
                        //注册read，监听客户端发送的消息
                        socket.register(selector, SelectionKey.OP_READ);
                        //keys为所有键，除掉serverSocket注册的键就是已连接socketChannel的数量
                        String message = "connect successful, your number is " + (selector.keys().size() - 1);
                        //向客户端发送消息
                        socket.write(ByteBuffer.wrap(message.getBytes()));
                        InetSocketAddress address = (InetSocketAddress) socket.getRemoteAddress();
                        //输出客户端地址
                        System.out.println(ca.getTime() + "\t" + address.getHostString() +
                                ":" + address.getPort() + "\t");
                        System.out.println("客戶端已连接");
                        System.out.println("=========================================================");
                    }              
                    if (key.isReadable()) {
                        SocketChannel socket = (SocketChannel) key.channel();
                        InetSocketAddress address = (InetSocketAddress) socket.getRemoteAddress();
                        System.out.println(ca.getTime() + "\t" + address.getHostString() +
                                ":" + address.getPort() + "\t");
                        ByteBuffer bf = ByteBuffer.allocate(1024 * 4);
                        int len = 0;
                        byte[] res = new byte[1024 * 4];
                        //捕获异常，因为在客户端关闭后会发送FIN报文，会触发read事件，但连接已关闭,此时read()会产生异常
                        try {
                            while ((len = socket.read(bf)) != 0) {
                                bf.flip();
                                bf.get(res, 0, len);
                                String message = new String(res, 0, len);
                              //注册，客户端发来的语句为=》register:name:password
                                if(message.contains("register")){/** 其他操作可类似*/
                                    String[] strArr = message.split(":");
                                    String user=strArr[1];
                                    String password=strArr[2];
                                	Statement stmt=conn.createStatement();
                            		String sql="insert into admin values(null,'"+user+"','"+password+"')";
                            		System.out.println(sql);
                            		stmt.executeUpdate(sql);
                                    //向客户端发送消息
                                    socket.write(ByteBuffer.wrap("ACK".getBytes()));
                                }
                                System.out.println(message);
                                bf.clear();
                            }
                            System.out.println("=========================================================");
                        } catch (IOException e) {
                            //客户端关闭了
                            key.cancel();
                            socket.close();
                            System.out.println("客戶端已断开");
                            System.out.println("=========================================================");
                        }catch  (SQLException    e)  {  
                            //向客户端发送消息
                            socket.write(ByteBuffer.wrap("ERRO".getBytes()));
                        } 
                    }
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("服务器异常，即将关闭..........");
            System.out.println("=========================================================");
        }
    }
}
```
# 参考链接
[基于NIO的Socket通信(使用Java NIO的综合示例讲解)](https://blog.csdn.net/weixin_42762133/article/details/100040141)  

>> PS：还差server与client的互动没有完成等完成了再分享  