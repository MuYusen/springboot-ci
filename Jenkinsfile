#!groovy
Library 'jenkins-shared-library-cicd'

pipeline {
    agent any
    parameters {
    //repoBranch参数
    string(name:'repoBranch', defaultValue: 'master', description: 'git分支名称')
    //服务器选择
    choice(name: 'server',choices:'123', description: '测试服务器列表选择(IP,JettyPort,Name,Passwd)')
    string(name:'dubboPort', defaultValue: '31100', description: '测试服务器的dubbo服务端口')
    //单元测试代码覆盖率要求，各项目视要求调整参数
    string(name:'lineCoverage', defaultValue: '20', description: '单元测试代码覆盖率要求(%)，小于此值pipeline将会失败！')
    //若勾选在pipelie完成后会邮件通知测试人员进行验收
    booleanParam(name: 'isCommitQA',description: '是否在pipeline完成后，邮件通知测试人员进行人工验收',defaultValue: false )
    }
    //环境变量，初始确定后一般不需更改
    tools {
        maven 'maven3'
        jdk   'jdk8'
    }

    //pipeline的各个阶段场景
    stages {
        stage('代码获取') {
            steps {
              sayHello 'MuYu'
            }
        }
        stage('单元测试') {
            steps {
              echo 'Test'
            }
        }
    }
}
