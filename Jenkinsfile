pipeline {
  agent none

  environment {
    MAJOR_VERSION = 1
  }

  stages {
    stage('Say Hello') {
      agent any

      steps {
        sayHello 'Awesome Student!'
      }
    }
    stage('Git Information') {
      agent any

      steps {
        echo "My Branch Name: ${env.BRANCH_NAME}"

        script {
          def myLib = new linuxacademy.git.gitStuff();

          echo "My Commit: ${myLib.gitCommit("${env.WORKSPACE}/.git")}"
        }
      }
    }
    stage('Unit Tests') {
      agent any
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build') {
      agent any
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }
    stage('deploy') {
      agent any
      steps {
        sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }
    stage("Running on CentOS") {
      agent any
      steps {
        sh "wget http://brandon4231.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    
    
}
