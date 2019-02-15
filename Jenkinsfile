pipeline {
  agent {
    node {
      label 'ibmi'
    }
  }

  environment {
    OBJECT_MODE = '64'
    CC = 'gcc'
    CXX = 'g++'
  }

  stages {
    stage('build') {
      steps {
        sh 'phpize'
        sh './configure --with-IBM_DB2=/QOpenSys/usr --host=powerpc-ibm-aix6 --build=powerpc-ibm-aix6'
        sh 'make'
      }
    }
  }
}
