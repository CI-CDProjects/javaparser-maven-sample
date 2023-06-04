pipeline {
    agent { label 'JDK_8' }
    triggers { pollSCM('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['compile', 'test', 'install', 'package', 'clean'], description: 'These are Maven goals.')
    }
    stages {
        stage('VCS') {
            steps {
                git url: 'https://github.com/CI-CDProjects/javaparser-maven-sample.git',
                    branch: 'decl'
            }
        }
        stage('Build the Package') {
        tools {
            jdk 'JAVA8_UBUNTU20.04'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('Archive the Package') {
            steps {
                archiveArtifacts onlyIfSuccessful: true,
                                artifacts: '**/target/javaparser-maven-sample-1.0-SNAPSHOT-shaded.jar',
                                allowEmptyArchive: false
            }
        }
    }
    post {            
        success {
            mail to: 'tqdevops-team@qt.com',
                from: 'tqdevops@qt.com',
                subject: "Jenkins build status of ${JOB_NAME} with id ${BUILD_ID}",
                body: "Refer Here ${BUILD_URL} for more details. Build is Successful"
        }
        failure {
            mail to: "${GIT_AUTHOR_EMAIL}",
                from: 'tqdevops@qt.com',
                subject: "Jenkins build status of ${JOB_NAME} with id ${BUILD_ID}",
                body: "Refer Here ${BUILD_URL} for more details. Build has Failed"
        }
    }
}