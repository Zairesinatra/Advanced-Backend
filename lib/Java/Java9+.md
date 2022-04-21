# Java9+

经历四次跳票的 Java9 于 2017 年 9 月 21 日正式发布，Oracle 宣布今后按照每六个月一次的节奏进行更新。意味着从以特性驱动的发布周期转向以时间驱动的模式。并以三年为周期发布长期支持版本 Long Term Support(LTS)。

## 安装与切换

推荐安装 LTS 版本，即 Java8、11。[点此](https://www.oracle.com/java/technologies/downloads/)快速进入 oracle Java 页面。

* Mac 下查看已安装的 jdk 版本及其安装目录

```shell
# 输入命令参数区分大小写
/usr/libexec/java_home -V
# 结果默认排序为当前已安装jdk目录、Mac默认使用的jdk版本
```

* IDEA 开发环境设置

在 IntelliJ 中 File 下选择 Project Structure 查看项目的 SDK。新下载版本需要在 new 选项里进行导入。完成后在左侧 Project Settings 中点击 Modules 项可以指定 Sources 与 Dependencies 需要识别 SDK 的版本。

## Java9 新特性

### 模块化系统 Jigsaw

Java9 首先想到的就是 Jigsaw|Modularity 项目。众所周知 Java 从 95 至今已经发展超过 20 年，在相关生态在不断丰富的同时也越来越暴露出一些问题。

每次 JVM 启动的时候，至少会有 30~60MB 的内存加载，主要原因是 JVM 加载 rt.jar。不论其中的类是否被 classloader 加载，整个 jar 都会被 JVM 加载到内存当中去（而模块化可以根据模块的需要加载程序运行需要的 class）。当代码库逐步复杂，盘根错节的不同版本类库交叉依赖导致产生许多令人头疼的问题，这些阻碍了 Java 开发和运行效率的提升。系统没有对不同 JAR 文件之间的依赖关系添加明确概念，这导致每一个公共类都可以被类路径之下任何其它的公共类所访问，可能会导致无意中使用了并不想被公开访问的 API。

