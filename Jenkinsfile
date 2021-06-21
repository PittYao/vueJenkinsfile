pipeline {
    agent any
    environment {
        NAME = 'vuejenkinsfile'
        PROFILE = 'dev'
        APP = 'fanyao666.online/fanyao/vuejenkinsfile:dev'
        APP_PORT = 7081

        credentialsId = '3c624c30-b117-47c8-9e3e-c9551498e3a5'
        gitUrl = 'http://gitcebon.cebon-company.online:8080/fanyao/vueJenkinsfile.git'
        gitBranch = 'master'
    }

    stages {
        stage('下载代码') {
            steps {
                echo '****************************** download code start... ******************************'
                git branch: '$gitBranch', credentialsId: '$credentialsId', url: '${gitUrl}'
            }
        }

        stage('vue编译') {
            steps {
                echo '****************************** vue start... ******************************'
                sh 'cnpm install'
                sh 'cnpm run build'
            }
        }

        stage('构建Docker镜像') {
            steps {
                echo '****************************** delete container and image... ******************************'
                sh 'docker ps -a|grep $NAME|awk \'{print $1}\'|xargs -i docker stop {}|xargs -i docker rm {}'
                sh 'docker images|grep $NAME|grep dev|awk \'{print $3}\'|xargs -i docker rmi {}'

                echo '****************************** build image... ******************************'
                sh 'docker build --build-arg PROFILE=$PROFILE -t $APP .'
            }
        }

        stage('运行容器') {
            steps {
                echo '****************************** run start... ******************************'
                sh 'docker run -d -p $APP_PORT:80 --restart=always --name $NAME $APP'
            }
        }
    }
}