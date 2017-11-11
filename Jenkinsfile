node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }
    
    stage('Fetch') {
        checkout scm
    }
    
    stage('Build'){
        sh 'composer install --prefer-dist --no-dev --ignore-platform-reqs'
    }
    
    stage('config') {
        parallel(
            'config cache': {
                sh 'php artisan config:cache'
            },
            'config route': {
                sh 'php artisan'
            }
        )
    }
    stage('Docker Build') {
        sh 'docker build -t ludavilla/todoapi:$BUILD_NUMBER .'
    }
    
    stage('Docker Ship') {
        sh 'docker push ludavilla/todoapi:$BUILD_NUMBER'
    }
}
