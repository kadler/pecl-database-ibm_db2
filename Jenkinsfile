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
    LDFLAGS = '-Wl,-brtl' // configure requires this to be set, even though we default it on :(
  }

  stages {
    stage('build') {
      steps {
        sh 'phpize'
        sh './configure --with-IBM_DB2=/QOpenSys/usr --host=powerpc-ibm-aix6 --build=powerpc-ibm-aix6'
        sh 'make'
      }
    }

    stage('test') {
      environment {
        DATABASE = credentials('database-creds')
      }
      steps {
        sh label: 'setting up user', script:'sed "s|sample|*LOCAL|; s|db2inst1|$DATABASE_USR|; s|db2inst1|$DATABASE_PSW|; " tests/connection.inc'
        sh 'make test'
      }
    }
  }
}