在Apache中，基于端口的多虚拟主机实现。
原理：Apache可以访问文件目录。
1、配置Apache的<VirtualHost *:82>，让其DocumentRoot 为后台仓库的路径。
2、将Go to download repository 的跳转链接绑定到82端口。

#   1、安装工具

    zypper install createrepo_c

生成仓库元数据

    createrepo_c /path/to/your/rpm_directory

#   2   修改按钮跳转链接

##    2.1   修改Go to download repository 跳转链接

    vim /srv/www/obs/api/app/views/webui2/shared/_download_repository_link.html.haml

内容如下：

        - url = "http://leapfive.zobs.com:82/#{project.to_s.gsub(/:/, ':/')}/#{repository}"
    %li.list-inline-item
    = link_to(url, title: 'Go to download repository', class: 'nav-link') do
        %i.fas.fa-download
        Go to download repository

#   3   Apache

## 3.1  修改apache配置
    vim /etc/apache2/vhosts.d/obs.conf

内容如下：

    <VirtualHost *:82>
            # The resulting repositories
            ServerName leapfive.zobs.com
            DocumentRoot  "/srv/obs/repos"
            #DocumentRoot  "/srv/rpm-repo"
    
            <Directory /srv/obs/repos>
            #<Directory /srv/rpm-repo>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
            # 启用美化样式
            IndexOptions FancyIndexing HTMLTable NameWidth=* DescriptionWidth=*
            </Directory>
    
            ErrorLog /srv/www/obs/api/log/apache_error.log
            TransferLog /srv/www/obs/api/log/apache_access.log
    </VirtualHost>

##    3.2   重启Apache服务

    service apache2 restart





#   其他相关路径：
    ####BEGIN######
    1、修改配置：
    vim /srv/www/obs/api/test/fixtures/configurations.yml
    vim /srv/www/obs/api/app/views/shared/_download_repository_link.html.haml
    2、修改地址：
    #download_url: https://leapfive.zobs.com/download

    vim /srv/www/obs/api/app/models/configuration.rb
    会看到download_url: CONFIG['download_url']信息

    ####END#######