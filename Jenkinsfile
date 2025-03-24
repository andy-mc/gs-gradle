node {
  def myGradleContainer = docker.image('openjdk:8-jdk')
  myGradleContainer.pull()

  stage('SCM check y Preparacion') {
    checkout scm
  }

  stage('Debug env.HOME') {
    echo ":D:D Valor de env.HOME desde Jenkins: ${env.HOME}"
    sh 'echo Valor de $HOME desde el agente'
    myGradleContainer.inside {
      sh 'echo Valor de $HOME dentro del contenedor'
      sh 'ls -la $HOME || echo "No existe el directorio $HOME"'
    }
  }

  stage('Instalar y probar con Gradle') {
    myGradleContainer.inside {
      sh '''
        apt-get update && apt-get install -y wget unzip
        wget https://services.gradle.org/distributions/gradle-7.6.1-bin.zip
        unzip -q gradle-7.6.1-bin.zip
        export PATH=$PWD/gradle-7.6.1/bin:$PATH
        cd complete && gradle test
      '''
    }
  }

  stage('Ejecucion') {
    myGradleContainer.inside {
      sh '''
        export PATH=$PWD/gradle-7.6.1/bin:$PATH
        cd complete && gradle run
      '''
    }
  }
}
