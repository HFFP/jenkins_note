## 版本回滚
    
1. [版本存档](#archives)
2. [参数化构建](#2)
3. [修改shell脚本](#3)

<span id="archives"></span>
### 版本存档
在构建后操作 添加 **Archive the artifacts** 插件并输入你需要存档的文件
存档的文件存放在 JENKINS_HOME/jobs/项目名/builds/构建版本号/archive

配置好如下图

![Archive](/config/archive.png)

<span id="2"></span>
### 参数化构建
在General 勾选 参数化构建过程， 并添加参数（参数请根据实际需求来选择）

这里我选择了两张参数 一种为Choice Parameter, 可以理解为选择框
*   Name  参数名           
*   Choices 选项（换行分隔）
*   Description 解释描述
如下图
![ChoiceParameter](/config/ChoiceParameter.png)

一种为String Parameter，可以理解为字符串参数
*   名字
*   默认值
*   描述
![StringParameter](/config/StringParameter.png)

<span id="3"></span>
### 修改shell脚本
因为选择了参数化构建 那么对应的shell脚本也应该作相应的修改

比如之前的脚本为:

    cd build/libs
    java -jar KotlinTest-1.0-SNAPSHOT.jar

那么修改之后如下:

    case $deploy_env in
    	deploy)
        	echo "deploy:$deploy_env"
    		cd build/libs/
    		java -jar KotlinTest-1.0-SNAPSHOT.jar
            ;;
        rollback)
        	echo "rollback:$deploy_env"
            echo "version:$version"
            cp -R ${JENKINS_HOME}/jobs/Test/builds/${version}/archive/build/libs/KotlinTest-1.0-SNAPSHOT.jar .
            java -jar KotlinTest-1.0-SNAPSHOT.jar
            ;;
        *)
        exit
        	;;
    esac

参数**$deploy_env，$version**为参数化构建传递进来的参数，这里我们是回滚，即判断是否为部署执行相应的脚本，