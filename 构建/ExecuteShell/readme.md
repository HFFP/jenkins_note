## Execute shell
>执行脚本， 这里就没什么说的了，根据具体的需求做对应的事情就好。这里对应的是Linux，windows另有插件

### 特别注意

因为jenkins的原因，你在shell脚本中启动的进程，会在构建结束后和job一同kill掉。

>原因：

    Jenkins为了有效的杀死job运行时创建的子进程，提供了一些原生代码找到并杀死它们，
    这样做非常合理，当一个job结束时势必要杀死运行期间启动的进程，否则系统里会留下很多僵尸进程。
    
>解决方法(推荐使用方法2)

    Jenkins提供了hudson.util.ProcessTree.disable和hudson.util.ProcessTreeKiller.disable两个属性来控制该特性，值为true将禁用此特性。
    hudson.util.ProcessTree.disable从Jenkins 1.260开始使用，
    而使用1.315之前的Hudson时只能使用hudson.util.ProcessTreeKiller.disable，
    为了版本兼容，在Jenkins 1.260后这两个属性都可能使用，
    建议使用1.260之的Jenkins用户使用hudson.util.ProcessTree.disable属性。
    
1. 通过Jenkins提供的启动参数禁用杀死子进程的特性
    * 使用java -jar启动，-Dhudson.util.ProcessTree.disable=true -jar jenkins.war
    * 使用Tomcat启动，Linux系统修改catalina.sh，在环境变量的说明后，
    脚本开始前加上JAVA_OPTS="$JAVA_OPTS -Dhudson.util.ProcessTree.disable=true"；
    Windows系统修改catalina.bat，在环境变量的说明后，
    脚本开始前加上set JAVA_OPTS=%JAVA_OPTS% "-Dhudson.util.ProcessTree.disable=true"；修改好Tomcat的配置文件后重新启动Tomcat

2. 修改Jenkins的环境变量BUILD_ID，这样Jenkins将不认为你启动的后台进程是由job创建的
    * 在脚本头部加入 BUILD_ID=dontKillMe 即可
    
    
![shell](/config/shell.png)