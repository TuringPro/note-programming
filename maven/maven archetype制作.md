# maven archetype制作



## **什么是maven archetype？**

`maven-archetype-archetype` is an archetype which generates a sample archetype

**上述是官网的描述，可以看出archetype是用于生成原型的项目。**

例如我们在用idea生成maven项目时，有很多可选的archetype模板，可以自动帮助我们生成一些文件：

<img src="/Users/yanghongwei/Library/Application%20Support/typora-user-images/image-20210313171200475.png" alt="image-20210313171200475" style="zoom:50%;" />

那么这些都是即有的模板，我们也可以自己定义一个属于自己的模板。

------

## 自定义archetype模板

现在我有一个简单的maven项目：

<img src="https://raw.githubusercontent.com/TuringPro/images/main/image-20210313171859266.png" alt="image-20210313171859266" style="zoom:50%;" />

生成archetype是需要archetype插件的，不过也可以不显示的写出来。

现在是需要在项目根目录下执行命令：

```
mvn archetype:create-from-project
```

执行完会在根目录生成一个target文件，cd target/generate-sources/archetype 进入archetype文件夹下

执行安装命令安装模板到本地仓库：

```
mvn install  //安装到本地仓库
mvn archetype:crawl  //在本地仓库根目录生成archetype-catalog.xml骨架配置文件
```

打开archetype-catalog.xml可以看到文件内有我们刚才Hello项目的GAV，已经安装上了。

现在，可以开始基于该模板创建我们新的项目了，执行如下命令：

```
mvn archetype:generate -DarchetypeCatalog=local  //local意思是指向本地的archetype
```

根据提示输入新项目的GAV（groupId  artifactId  version），完成新项目创建。



## 发布模板到私服仓库

生成的target/generate-sources/archetype下的模板如果不单独维护的话，项目clean就会被删除掉了。

可以把archetype文件夹移出来单独维护。如果想把模板发布到私服，可以在archetype文件夹根目录下的pom文件加上：

```
<distributionManagement>
    <repository>
      <id>releases</id>
      <url>http://nexus/repository/xxx/</url>  //配置自己的仓库地址
    </repository>
    <snapshotRepository>
      <id>snapshots</id>
      <url>http://nexus/repository/xxx-snapshot/</url>
    </snapshotRepository>
  </distributionManagement>
```

然后在archetype根目录下执行：

```
mvn deploy
```

然后就可以根据自己远程仓库生成模板项目了。