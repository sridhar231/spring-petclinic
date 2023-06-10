pipeline { 
    agent { label 'JDK_17'}
    tools { jdk 'JAVA_17'}
    stages {
        stage('VCS') {
            steps {
                git url: 'https://github.com/sridhar231/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage('build') {
            steps {
                sh "mvn package"
            }
        }
        stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean package sonar:sonar -Dsonar.login=43be213320a3cc723d4215d900750b4c4c655893 -Dsonar.organization=springpetclinic-sri -Dsonar.projectKey=springpetclinic-sri_springpetclinic-sri'                   
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