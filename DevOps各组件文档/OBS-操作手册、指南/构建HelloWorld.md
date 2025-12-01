#   构建openEuler:24.03相关项目



# 1   配置Hello World项目
省略新建项目
## 1.1  上传源码
先将HelloWorld的源码下载到本地。

    git clone https://gitee.com/zz9804/gitee-test.git

点击**Create Package**,新建包。点击**Add file**上传刚才下载的源码。

## 1.2   配置Meta

    <project name="Test">
    <title/>
    <description/>
    <person userid="Admin" role="maintainer"/>
    <repository name="openEuler_24.03_11">
        <path project="openEuler:24.03:BaseOS:01" repository="9950x"/>
        <arch>riscv64</arch>
    </repository>
    </project>

该配置表示本项目的仓库名为**openEuler_24.03_11**,依赖的是**openEuler:24.03:BaseOS:01**项目中的**9950x**仓库。


上传完毕后会自动构建。

