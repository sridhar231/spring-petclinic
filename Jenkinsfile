pipeline { 
    agent { label 'MAVEN_JDK17'}
    tools { jdk 'JDK_17'}
    stages {
        stage('VCS') {
            steps {
                git url: 'https://github.com/sridhar231/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage('build') {
            steps {
                sh "mvn pacakage"
            }
        }
        stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=springpetclinic-sri'
                }
            }
        }
        stage('artifact') {
           steps {
            archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar'
            junit testResults: '**/surefire-reports/TEST-*.xml'
           }
       }
    }
}