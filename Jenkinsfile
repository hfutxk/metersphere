pipeline {
    agent {
        node {
            label 'master'
        }
    }
    options { quietPeriod(1800) }
    parameters { 
        string(name: 'IMAGE_NAME', defaultValue: 'metersphere', description: '构建后的 Docker 镜像名称')
        string(name: 'IMAGE_FREFIX', defaultValue: 'metersphere', description: '构建后的 Docker 镜像带仓库名的前缀')
    }
    stages {
        stage('Build/Test') {
            steps {
                configFileProvider([configFile(fileId: 'metersphere-maven', targetLocation: 'settings.xml')]) {
                    sh "mvn clean package --settings ./settings.xml"
                }
            }
        }
        stage('Docker build & push') {
            steps {
                sh "docker build --build-arg MS_VERSION=${BRANCH_NAME}-b${BUILD_NUMBER} -t ${IMAGE_NAME}:${BRANCH_NAME} ."
                sh "docker tag ${IMAGE_NAME}:${BRANCH_NAME} ${IMAGE_REGISTRY}/${IMAGE_NAME}:${BRANCH_NAME}"
                sh "docker push ${IMAGE_FREFIX}/${IMAGE_NAME}:${BRANCH_NAME}"
            }
        }
    }
}
