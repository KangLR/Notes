# IDEA使用

## 打JAR包

### JAR包

1. 点击File->Project Structure
2. 点击Artifacts（人工制品）
3. 右击.jar文件，add-jar-empty
4. 点击右下角apply
5. 回到主界面点击Build菜单中的Build Artifacts
6. 点击相应jar文件或All Artifacts-Build

### 测试

![image-20200304232153221](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304232153221.png)

如图

在生成的out/artifacts目录下有生成的jar，在cmd中允许即可

### Source Code

![image-20200304232421864](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304232421864.png)

---

## 便捷设置

点击File->Setting

### 自动导包功能

Editor-General-AutoImport勾选Add和Optimize

![image-20200304234624787](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304234624787.png)

### 鼠标悬停提示

Editor-General下滑勾选Show quick documentation on mouse move

可设置延迟，默认500ms

![image-20200304234817506](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304234817506.png)

### 行号与方法分隔符

勾选Show line numbers & Show method separators

![image-20200304235246904](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304235246904.png)

### 忽略大小写

取消Match case

![image-20200304235602162](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304235602162.png)

### 文件名多行显示

取消Show tabs in one row

![image-20200304235958358](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200304235958358.png)

### 更改注释颜色

![image-20200305000922590](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305000922590.png)

### 改编码

统一改成UTF-8

![image-20200305001500850](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305001500850.png)

### 自动编译

勾选以下两项

![image-20200305001927688](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305001927688.png)

### 改快捷键

例：代码自动补全Shortcut改为Alt+.

![image-20200305123633225](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305123633225.png)

---

## 常用快捷键

```txt
代码自动补全	Alt+.
自动生成代码	Alt+Insert(常规电脑)	Alt+Fn+Insert(笔记本)
自动补全结尾	Ctrl+Shift+Enter
自动代码生成模板	Ctrl+j	或者写完按Tab键
修正导入的包	Alt+Enter    
格式化代码	Ctrl+Alt+l(跟QQ快捷键可能有冲突)
代码自动缩进	Ctrl+Alt+i
显示最近更改的代码	Ctrl+e
方法参数提示	Ctrl+p
选择代码放入If或Try内	Ctrl+Alt+t
删除行	Ctrl+y
复制行 Ctrl+d
选择代码 Ctrl+w	反选Ctrl+Shift+w
跳转到指定行	Ctrl+g
转换大小写	Ctrl+Shift+u
方法光标转移	Alt+方向键
退回	Ctrl+z

debug调试
设断点-右键点击Debug running...
在左下方按F7逐行执行以及其他操作

查找类	Ctrl+n
查找文件 Ctrl+Shift+n
查找文本 Ctrl+f
文本替换 Ctrl+r
单行注释 Ctrl+/
多行注释 Ctrl+Shift+/
```

---

## 修改模板

### Live Templates

END代表光标停留地点

应用于Java

可以自己点击右边加号新建模板

![image-20200305173001933](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305173001933.png)

### File and Code Templates

添加注释

![image-20200305174526395](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305174526395.png)

或

![image-20200305180050336](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305180050336.png)

新建类结果

![image-20200305174747380](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305174747380.png)

或

![image-20200305180228071](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305180228071.png)

### Postfix Completion

例:

username.notnull

自动转换成

if(username!=null){

}

熟练使用有利于提高开发效率

![image-20200305181217520](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305181217520.png)

## Maven配置

### 配置阿里云镜像

1. 本地仓库

默认下载JAR包的存储地址

![image-20200305231722846](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305231722846.png)

2. 修改镜像

```xml
<mirror>
      <id>nexus-aliyun</id>
      <mirrorOf>central</mirrorOf>
      <name>Nexus aliyun</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

### 在IDEA中更改

![image-20200305233602645](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305233602645.png)

![image-20200305233753219](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305233753219.png)

## 新建Maven项目

1. 点击Maven

![image-20200305234441722](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305234441722.png)

2. 命名

例：http://spark.apache.org/

GroupId类似于org.apache(创建该项目的公司或其他)

ArtifactId类似于spark(项目名)

![image-20200305234542299](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305234542299.png)

3. 选择

![image-20200305234757391](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200305234757391.png)

4. 等Maven下载依赖包，过程可能需要几分钟，不要去动

![image-20200306000459290](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200306000459290.png)

5. 若要导入JAR包，只需要在pom.xml添加即可

## 打包Maven项目

点击左下角Terminal

输入mvn clean package

jar包出现在target目录下

运行一下

![image-20200306001513319](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200306001513319.png)

## 打包Maven Web项目

1. 右侧maven栏点击package右键run
2. 点击install右键第二个
3. 是war包
4. 拷贝到tomcat放到webapps下也可运行