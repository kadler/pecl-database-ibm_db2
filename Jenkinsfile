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
        IBM_DB2_TEST_SKIP_CONNECT_FAILURE = 0 // don't skip tests if we can't connect
        REPORT_EXIT_STATUS = 1
        NO_INTERACTION = 1
      }
      steps {
        sh label: 'setting up user', script:'perl -p -i -e "s|sample|*LOCAL|; s|db2inst1|$DATABASE_USR|; s|db2inst1|$DATABASE_PSW|; " tests/connection.inc'
        sh 'make test TESTS="-s report.txt"'
      }
      post {
        failure {
          sh 'cat report.txt'
        }
      }
    }
  }
}
