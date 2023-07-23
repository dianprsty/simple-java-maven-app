// node {
//     docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2'){
//         stage('Build') {
//                 sh 'mvn -B -DskipTests clean package'
//         }
//         stage('Test') {
//             sh 'mvn -B -DskipTests clean package'
//         }
//         stage('Deploy') {
//             sh './jenkins/scripts/deliver.sh'
//             input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
//             // sh './jenkins/scripts/kill.sh'
            
//         }

//     }
// }


pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?'
        }

        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                sleep 1m
            }
        }
    }
}
