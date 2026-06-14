pipeline {
 agent any
 stages {
 stage('Checkout') { steps { checkout scm } }
 stage('Build') {
 steps {
 echo 'Building...'
 sh 'node --version'
 }
 }
 stage('Test') {
 steps { echo 'Tests passed!' }
 }
 stage('Deploy') {
 steps {
 sh 'docker build -t jenkins-demo .'
 echo 'Deployed!'
 }
 }
 }
 post {
 success { echo 'Pipeline SUCCESS!' }
 failure { echo 'Pipeline FAILED!' }
 }
}
