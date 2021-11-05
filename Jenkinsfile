pipeline {
agent any
tools {
maven '3.8.3'
}
environment {
PATH="/usr/local/bin:$PATH"
JAR_FILE_NAME="excursion-0.0.1-SNAPSHOT.jar"
GIT_USER_NAME="jenkins"
GIT_USER_EMAIL="jenkins@com.ua"
AWS_DEFAULT_REGION="eu-central-1"
BEANSTALK_APPLICATION_NAME="ExcursionISRomanko"
BEANSTALK_ENVIRONMENT_NAME="Excursionisromanko-env"
stages {
stage('Checkout') {
steps {
checkout([$class: 'GitSCM', branches: [[name: '*/main']], 
extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CONNECTION', url:
'git@github.com:ItsMathMage/Lab9_DevOps.git']]])
}
}
stage('Build') {
steps {
sh '''
mvn -B -DskipTests clean package
mkdir deploy
mv ./target/$JAR_FILE_NAME ./deploy
'''
}
}
stage('Deploy') {
steps {
withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
credentialsId: 'AWS_CREDENTIALS', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
sh '''
cd ./deploy
eb --version
git config --global user.name $GIT_USER_NAME
git config --global user.email $GIT_USER_EMAIL
git init
git add .
git commit -m "first commit"
echo n | eb init $BEANSTALK_APPLICATION_NAME --region 
$AWS_DEFAULT_REGION
eb deploy $BEANSTALK_ENVIRONMENT_NAME -v
'''
} }
}
}
post {
always {
cleanWs()
}}
}}
