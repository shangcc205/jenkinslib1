#!groovy
@Library('jenkinslib1') _ // 加载动态库
def tools = new org.devops.tools()

String workspace = "/opt/jenkins/workspace"


//Pipeline
pipeline {
    agent { node {label "build" //指定运行节点的标签或者名称 
                   customWorkspace "${workspace}" //指定工作目录 "${变量}"获取变量的语法
            }
    }


    options {
        timestamps() //日志会显示时间
        skipDefaultCheckout() //删除隐式checkout scm语句
        disableConcurrentBuilds() //禁止并行
        timeout(time: 1, unit: 'HOURS')  //流水线超时设置1h
    }

    stages {
        stage("GetCode"){ //阶段名称
            steps{
                timeout(time:5, unit:"MINUTES"){ //步骤超时时间
                    script{  //填写运行代码
                        println('获取代码')
                    }
                }
            }
        }

        stage("Build"){ //阶段名称
            steps{
                timeout(time:20, unit:"MINUTES"){ //步骤超时时间
                    script{  //填写运行代码
                        println('应用打包')

                        mvnHome = tool "m2"  // 设置一个变量 指定系统设置里面maven的名称
                        println(mvnHome)

                        sh "${mvnHome}/bin/mvn --version"
                    }
                }
            }
        }

        stage("CodeScan"){ //阶段名称
            steps{
                timeout(time:25, unit:"MINUTES"){ //步骤超时时间
                    script{  //填写运行代码
                        println('代码扫描')

                        tools.PrintMes("代码扫描", "green")
                    }
                }
            }
        }
       
    }
    // 构建后操作
    post {
        // always无论流水线或者阶段的完成状态
        always {
            script{
                println("always")
            }
        }
        // success 只有当流水线或者阶段状态为success才会运行 其他同理
        success {
            script{
                currentBuild.description = "\n 构建成功"
                // currentBuild是jenkins中的全局变量，description方法是描述信息
            }
        }

        failure {
            script{
                currentBuild.description = "\n 构建失败"
            }
        }

        aborted {
            script{
                currentBuild.description = "\n 构建取消"
            }
        }

    }
}
