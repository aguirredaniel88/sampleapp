pipeline {
  agent any
  stages {
    stage('Saludo') {
      steps {
        parallel(
          "Saludo": {
            echo 'Hello world DevOps!'
            
          },
          "Obtener cambios SCM": {
            git 'https://github.com/aguirredaniel88/sampleapp.git'
            
          }
        )
      }
    }
    stage('Pruebas Unitarias') {
      steps {
        parallel(
          "Pruebas Unitarias": {
            sh 'mvn clean test'
            junit 'target/surefire-reports/*.xml'
            
          },
          "Analisis de codigo": {
            sh 'mvn -DskipTests checkstyle:checkstyle pmd:pmd findbugs:findbugs'
            checkstyle(pattern: 'target/checkstyle-result.xml')
            pmd(pattern: 'target/pmd.xml')
            findbugs(pattern: 'target/findbugsXml.xml')
            
          }
        )
      }
    }
    stage('Pruebas de Integracion') {
      steps {
        sh 'mvn verify -DskipUTs'
        junit 'target/failsafe-reports/*.xml'
      }
    }
    stage('Publicar artefactos') {
      steps {
        sh 'mvn clean package -DskipTests'
        archiveArtifacts 'target/webapp*.jar'
      }
    }
  }
}