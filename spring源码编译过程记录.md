# 编译Spring源码记

    使用idea 编译spring 5.0 源码

## 环境准备

    环境准备是最重要的,这步搞好了,可以省很多功夫

    Macos + idea2019.3

### 下载spring 源码

![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-17-31-image.png)

选择好分支,复制链接使用命令下载

`git clone git@github.com:spring-projects/spring-framework.git`

### 安装gradle

 打开源码路径下`gradle/wrapper/gradle-wrapper.properties  文件

![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-22-18-image.png)

选择这个版本的gradle安装。

安装可以选择下载zip包解压好之后再配置环境变量,但是我这里选择使用brew 安装gradle

如果已经安装过gradle而且不是这个版本的话,可以修改版本。

 使用rb脚本配合brew命令安装

```shell
class Gradle < Formula
  desc "Open-source build automation tool based on the Groovy and Kotlin DSL"
  homepage "https://www.gradle.org/"
  url "https://services.gradle.org/distributions/gradle-5.6.4-all.zip"
  sha256 "abc10bcedb58806e8654210f96031db541bcd2d6fc3161e81cb0572d6a15e821"
  #这个是下载包的信息,需要填上对应的包的信息
  ## openssl dgst -sha256 /Users/admin/Downloads/gradle-4.1-all.zip 可以查看信息
  bottle :unneeded

  option "with-all", "Installs Javadoc, examples, and source in addition to the binaries"

  depends_on :java => "1.7+"  ##这里必须设置为1.7以上

  def install
    rm_f Dir["bin/*.bat"]
    libexec.install %w[bin lib]
    libexec.install %w[docs media samples src] if build.with? "all"
    (bin/"gradle").write_env_script libexec/"bin/gradle", Language::Java.overridable_java_home_env
  end

  test do
    assert_match version.to_s, shell_output("#{bin}/gradle --version")
  end
end
```

在执行命令`brew install ~/Downloads/gradle.rb`使用修改过的安装源去安装

[详情参考](https://www.jianshu.com/p/a537d9a4034f)

### 配置gradle下载远程仓库

    我之前失败过很多次就是因为这一部没有设置,下载依赖太慢,我还以为编译失败,有各种奇怪的报错.

    在gradle安装根目录下,创建一个文件gradle.init

```shell
allprojects{
    repositories {
       def REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public/'
        all { ArtifactRepository repo ->
            def url = repo.url.toString()
            if ((repo instanceof MavenArtifactRepository) && (url.startsWith('https://repo1.maven.org/maven2') || url.startsWith('https://jcenter.bintray.com'))) {
                project.logger.lifecycle 'Repository ${repo.url} replaced by $REPOSITORY_URL .'
                remove repo
            }
        }
        maven {
            url REPOSITORY_URL
        }
    }
}
```

### 导入idea

    如果这个时候已经导入了idea,就在终端进入spring源码根目录执行`gradle clean`

这个时候它显示build success就好了

* 导入spring源码文件夹,使用gradle导入项目

* 设置好idea的gradle配置
  
  ![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-40-54-image.png)

* 设置好编译器
  
  ![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-43-54-image.png)

* 设置项目结构
  
  file-project Structure
  
  ![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-45-48-image.png)
  
  排除掉spring-aspect模块
  
  ![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-46-31-image.png)

* 开始下载依赖

![loading-ag-251](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-47-44-image.png)

这里,没有编译好的,应该是没有gradle的选项的,就是一个单独的项目,点击刷新按钮开始下载就好了。

这时下载依赖应该就会很顺利了,过一会儿就会显示build success

左侧的项目结构中,每个模块应该都变成蓝色的

![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-51-52-image.png)

### 编译源码

在idea的终端中执行`./gradlew :spring-oxm:compileTestJava`

![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-13-53-13-image.png)

等待预编译完成,这里貌似也是不需要这样执行,但是以防万一吧,毕竟这是spring源码里导入idea的md文件说的,虽然已经是2016年的了。这一步编译完成之后就可以编译整个spring框架源码了.

![](/Users/persevere_y/Documents/my_notebook/images/2020-03-31-14-01-34-image.png)

等待程序编译好。

[参考资料](https://blog.csdn.net/Dcwjh/article/details/104471560?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

### 总结

为了看个spring源码我折腾了两天,记录下这次折腾的步骤给有缘人看节省他的时间,网上找到的过程大多都不太适用与自己,系统,idea 版本,spring源码版本,gradle版本等等。

所以看源码最重要是环境,一定要尽量靠近官方的环境,这样才节省时间~~
