#   构建openEuler:24.03相关项目

Tips:
本示例说明openEuler:24.03:BaseOS:01步骤。
openEuler:24.03和openEuler:24.03:Epol步骤类似，参考本文档即可。

#   说明
构建openEuler:24.03:BaseOS:01是用的openEuler:23.09:Repo滚动构建的，即openEuler:24.03:BaseOS:01依赖于openEuler:23.09:Repo。
openEuler:23.09:Repo是采用DoD（按需要下载Download On Demand）的方式，链接到远程项目，即openEuler:23.09:Repo是作为一个仓库提供原始.rpm，没有源码。


# 1.1   配置openEuler:23.09项目
openEuler:23.09:Repo无包的源码，所以只配置project config和Meta即可。
![Iamge](1.png)
## 1.1.1   配置project config

具体配置很长，全部内容保存在openEuler:23.09:Repo_config中

## 1.1.2   配置project config

    <project name="openEuler:23.09:Repo">
    <title/>
    <description/>
    <repository name="standard_riscv64">
        <download arch="riscv64" url="https://mirror.iscas.ac.cn/openeuler-sig-riscv/openEuler-RISC-V/obs/23.09/mainline/" repotype="rpmmd"/>
        <arch>riscv64</arch>
    </repository>
    </project>

其中url的链接就是远程仓库的链接。

# 2   配置openEuler:24.03:BaseOS:01项目
新建一个名为openEuler:24.03:BaseOS:01的空项目

## 2.1   上传源码
上传源码有两种方式：
一种是_service上传，_service文件可以手写，也可以从其他项目（本地或全程都可以）复制。
一种是源码复制到本地项目的目录后，上传。
##   2.1.1   _service上传源码
###   2.1.1.1   _service从中科院复制
openEuler:24.03:BaseOS:01项目中的_service文件是从中科院复制过来的。
前提是需要配置好osc的配置（vim /root/.config/osc/oscrc），具体如下：

    # see oscrc(5) man page for the full list of available options

    [general]
    # Default URL to the API server.
    # Credentials and other `apiurl` specific settings must be configured in a `[$apiurl]` config section.
    apiurl=https://leapfive.zobs.com
    no_verify=1
    allow_http=1

    [https://leapfive.zobs.com]
    # aliases=
    # user=
    # pass=
    # credentials_mgr_class=osc.credentials...
    user=Admin
    pass=opensuse
    credentials_mgr_class=osc.credentials.PlaintextConfigFileCredentialsManager

    [https://build.tarsier-infra.isrc.ac.cn/]
    # aliases=
    # user=
    # pass=
    # credentials_mgr_class=osc.credentials...
    user=zd
    pass=123456
    credentials_mgr_class=osc.credentials.PlaintextConfigFileCredentialsManager

其中****general**需填写本地OBS的链接，其他各段可以填写其他OBS的链接、用户名信息、密码保存方式等。

然后使用以下命令同步到本地：

    while read -r i; do
        osc -A https://build.tarsier-infra.isrc.ac.cn copypac -t https://leapfive.zobs.com openEuler:24.03 "$i" openEuler:24.03:BaseOS:01
    done < 1192BaseOS_name.txt

###   2.1.1.2   _service自定义

_service的内容形式如下：

    <services>
        <service name="tar_scm">
            <param name="scm">git</param>
            <param name="url">git@gitee.com:src-openeuler/A-Tune.git</param>
            <param name="revision">openEuler-24.03-LTS</param>
            <param name="exclude">*</param>
            <param name="extract">*</param>
        </service>
    </services>

##   2.1.2   本地上传源码
###  2.1.2.1   检出OBS项目到本地

    osc co openEuler:24.03:BaseOS:01

会在当前目录生成一个名为openEuler:24.03:BaseOS:01的文件夹。

###  2.1.2.2   复制源码到本地目录

    cd openEuler:24.03:BaseOS:01

    cp -r /path/to/sourcecode/ .

###  2.1.2.3   上传源码

    osc add *
    osc commit -m "add sourcecode"

静待上传完毕即会自动构建。

