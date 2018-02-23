// TODO: 2/17/19 Work on this
@Library('github.com/welagedara/pipeline-github-lib@master')

def library = new com.example.Library()

def label = "mypod-${UUID.randomUUID().toString()}"

// TODO: 2/17/19 Work on this
podTemplate(label: label, containers: [
    containerTemplate(name: 'java', image: 'airdock/oracle-jdk:1.8', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'docker', image: 'docker:1.12.6', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'maven', image: 'maven:3.3.9-jdk-8-alpine', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'golang', image: 'golang:1.8.0', ttyEnabled: true, command: 'cat')
  ],
  volumes:[
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {

    node(label) {

        git 'https://github.com/welagedara/microservice.git'
        env.MYTOOL_VERSION = '1.33'
        env.GIT_COMMIT_HASH=library.getCommitHash()
        env.GIT_CURRENT_BRANCH=library.getCurrentBranch()

        println "${GIT_COMMIT_HASH}"
        println "${GIT_CURRENT_BRANCH}"
        //println "${BRANCH_NAME}"

        stage('docker test') {
            //git 'https://github.com/welagedara/microservice.git'
            container('docker') {
                stage('Build the Java Project') {
                    //sh 'mvn -B clean install'
                    library.dockerTest()
                    sh 'ls'
                    sh "echo ${GIT_COMMIT_HASH}"
                    sh "docker ps"
                    //withDockerRegistry([credentialsId: 'gcr:Kubernetes', url: 'https://gcr.io']) {
                        // some block
                        //sh "docker pull gcr.io/kubernetes-195622/mysql"
                    //}
                    sh "docker pull redis"
                    sh "docker images | grep mysql"
                    sh "docker images | grep redis"
                    //sh './gradlew -DSPRING_PROFILES_ACTIVE=dev clean build'

                }
            }
        }


        stage('Buildg project') {
            //git 'https://github.com/welagedara/microservice.git'
            container('java') {
                stage('Build the Java Project') {
                    sh 'ls'
                    sh "echo ${GIT_COMMIT_HASH}"
                    //sh './gradlew -DSPRING_PROFILES_ACTIVE=dev clean build'

                }
            }
        }

        stage('Get a Maven project') {
            //git 'https://github.com/jenkinsci/kubernetes-plugin.git'
            container('maven') {
                stage('Build a Maven project') {
                    sh 'pwd'
                    sh 'ls'
                    sh 'echo $MYTOOL_VERSION'
                }
            }
        }

        stage('Get a Golang project') {
            //git url: 'https://github.com/hashicorp/terraform.git'
            container('golang') {
                stage('Build a Go project') {
                    sh """
                    pwd
                    ls
                    echo $MYTOOL_VERSION
                    """
                }
            }
        }

    }
}