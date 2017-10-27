## send build artifacts over SSH
>需要下载插件 Publish Over SSH [下载方法](/插件下载)

使用该插件首先需要配置SSH信息， 设置地址在 系统管理->系统设置 找到 Publish over SSH

1. 公共配置
    * Passphrase：密码
    * Path to key：key文件（私钥）的路径 
    * Key：将私钥复制到这个框中
    * Disable exec：禁止运行命令
2. 私有配置
    * SSH Server Name：标识的名字（随便你取什么）
    * Hostname：需要连接ssh的主机名或ip地址（建议ip）
    * Username：用户名
    * Remote Directory：远程目录
3. 高级
    * Use password authentication, or use a different key 可以替换公共配置（选中展开的就是公共配置的东西，这样做扩展性很好）
    
如下
![publish over ssh](/config/publishoverssh.png)


再回到构建
> name
    
    即选择你上面配置的服务器名
>Transfers

    Transfer Set Source files:   你需要传输的文件， 多文件用 逗号 隔开
    Remove prefix： 过滤的文件，或者说需要移除的目录（只能指定Transfer Set Source files中的目录）
    Remote directory：远程目录（根据你的需求填写吧，没有填写默认会继承系统配置）
    Exec command：把你要执行的命令写在里面
    
>高级

    Exclude files：排除的文件（在你传输目录的时候很有用，使用通配符，例如：**/*.log,**/*.tmp,.git/）
    Pattern separator：分隔符（配置Transfer Set Source files的分隔符。如果你这儿更改了，上面的内容也需要更改）
    No default excludes：禁止默认的排除规则（具体的自己看帮助）
    其他参数没有具体了解，可以看帮助
    
配置好如下
![over ssh](/config/overssh.png)