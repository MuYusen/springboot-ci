@Library('jenkins-shared-library-cicd')_

node {
    stage('code'){
        sh 'ls -al ;pwd;env;'
    }
    stage('build'){
        sh 'find'
        //sh 'mvn clean package'
    }
    stage('deploy'){
        sayHello('MuYu')
    }
    stage('clean'){
        sh 'ls -al ;rm -rf ./*;'
        deleteDir()
    }
}
