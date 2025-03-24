node {
  def myGradleContainer = docker.image('gradle:jdk8')
  myGradleContainer.pull()

  stage('SCM check y Preparacion') {
    checkout scm
  }

  stage('Debug env.HOME') {
    echo "Valor de env.HOME desde Jenkins: ${env.HOME}"
    sh 'echo Valor de $HOME desde el agente'
    myGradleContainer.inside {
      sh 'echo Valor de $HOME dentro del contenedor'
      sh 'ls -la $HOME || echo "No existe el directorio $HOME"'
    }
  }

  stage('Testeo') {
    myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
      sh 'cd complete && gradle test'
    }
  }

  stage('Ejecucion') {
    myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
      sh 'cd complete && gradle run'
    }
  }
}
