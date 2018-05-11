# MyEclipse

【更改eclipse(myeclipse) author的默认名字 --- 修改MyEclipse eclipse 注释的作者】
    在eclipse/myeclipse中，当我们去添加注释的作者选项时，@author 后边一般都会默认填充的你登录计算机的用户名。如何去修改呢： 
方法一：修改计算机登录的用户名（99.9999%的人应该都不愿意去这样做，特别是一些公司的域帐户登录的电脑根本就改不了）。 
方法二：将 @author 属性写死 。 
通过菜单 Window->Preference 打开参数设置面板，然后选择：  
1.Java -> Code Style -> Code Templates  
2.在右侧选择Comments,将其中的Types项，然后选右边的"Edit"，进入编辑模式，将 @author ${user} 中的${user}改成你自己的名字即可。 
----我也曾经这改过，咸麻烦。 
方法三：修改ini配置文件。 
在eclipse/myeclipse的目录下找到eclipse.ini/myeclipse.ini文件，在-vmargs后边添加上启动参数：-Duser.name=你想要显示的名字。 
重启eclipse/myeclipse搞定。
来源：http://blog.csdn.net/lianggeblog/article/details/7267475

【myeclipse加版权信息】
myeclipse 添加头文件注释（更改头注释的内容）文章分类:软件开发管理 
通过菜单 Window->Preference 打开参数设置面板，然后选择： 
1.Java -> Code Style -> Code Templates 
2.在右侧选择Comments,将其中的Files项，然后选右边的"Edit"，进入编辑模式： 
3.进入编辑模式后就可以自定义注释了。另外可以插入一些变量，如年、日期等等。 
4.最后，确保 Code -> New Java files 中有："${filecomment}" 
一、创建新Java文件头部注释 windows-->preference Java-->Code Style-->Code Templates code-->new Java files 编辑它 Java代码 
1.${filecomment} 
2.${package_declaration} 
3./** 
4.* @author 作者 
5.* @version 创建时间：${date} ${time} 
6.* 类说明 
7.*/ 
8.${typecomment} 
9.${type_declaration} ${filecomment} ${package_declaration} /** * @author 作者 * @version 创建时间：${date} ${time} * 类说明 */ ${typecomment} ${type_declaration} 
二、为方法提供注释模板设置方法注释模板：选择eclipse菜单栏中【窗口】下的【首选项】，展开左边树到Java->代码样式->代码模板，展开右边出现的对话框中的注释->方法，点击右边的【编辑】按钮。编辑其中的内容。也可以点击下面的【插入变量】按钮添加变量。
例如：
Java代码 
1./** 
2.*@author${user} 
3.*功能： 
4.*${tags} 
5.*/
来源：http://blog.csdn.net/yaohaibing576082210/article/details/5850292

【MyEclipse设置Java模板和JSP模板】
多行注释快捷键：选中要加注释的方法或类，按Alt+Shift+J。
代码标准化快捷键：选中要标准化的部分，按Ctrl+Shift+F。
--------------------------------
文档注释
javadoc工具提取文档注释生成API文档
常用javadoc标记
@author
@version
@deprecated
@param
@return
@see
@exception
@throws
但是javadoc标记的使用也有位置限制
类或接口文档注释 @author @version @deprecated @see等
方法或构造器文档注释 @param @return @exception @throws等
属性文档注释 @deprecated @see
--------------------------------
Java
设置注释模板的入口：Window->Preference->Java->Code Style->Code Template然后展开Comments节点就是所有需设置注释的元素。
现就每一个元素逐一介绍：
文件(Files)注释标签：
类型(Types)注释标签（类的注释）：
字段(Fields)注释标签：
构造函数标签：
方法(Constructor & Methods)标签：
覆盖方法(Overriding Methods)标签：
代表方法(Delegate Methods)标签：
getter方法标签：
setter方法标签：
--------------------------------
比如以下具体例子：
类型(Types)注释标签(类的注释)Comment for created types：
字段(Fields)注释标签Comment for fields：
方法(Constructor & Methods)标签Comment for non-overriding methods：
--------------------------------
JSP
用的是MyEclipse9.0
步骤如下：
1：myeclipse9安装目录\Common\plugins \com.genuitec.eclipse.j2eedt.core_9.0.0.me201103181703\templates \velocity\welcome路径下找到Jsp.vtl，复制一份，重命名为struts2.vtl,然后把里面的内容修改为自己想要的格式，保 存。然后把该文件复制放到 myeclipse9安装目录\Common\plugins \com.genuitec.eclipse.wizards_9.0.0.me201103012021.jar里的templates\jsp文件夹 下面即可。
2：找到 myeclipse9安装目录\Common\plugins\com.genuitec.eclipse.wizards_9.0.0.me201103012021.jar
把jar打开里面的templates.xml里面
    <template
        context="com.genuitec.eclipse.wizards.jsp"
        script="templates/jsp/Jsp.vtl"
        name="Default JSP template"/>
下面增加
    <template
        context="com.genuitec.eclipse.wizards.jsp"
        script="templates/jsp/struts2.vtl"
        name="Struts2 template"/>
3：重新启动myeclipse 新建jsp，在模板中就会出现Struts2 template，选中，按完成，新建的jsp页面就按你的模板生成！
注意：MyEclipse8.6路径是：安装目录\Common\plugins\com.genuitec.eclipse.j2eedt.core_8.6.0.me201007292038\templates\velocity\welcome
模板位置：安装目录\Common\plugins\com.genuitec.eclipse.wizards_8.6.0.me201007140905.jar
来源：http://blog.sina.com.cn/s/blog_6d59e57d0100xo6m.html








