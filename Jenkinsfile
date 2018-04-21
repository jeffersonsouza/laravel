node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }

    stage('Fetch') {
        checkout scm
    }

    stage('Build for Tests'){
        sh 'composer install --no-scripts --prefer-dist --ignore-platform-reqs'
    }

    stage('config') {
        parallel(
            'config cache': {
                echo 'Tarefa paralela 01'
            },
            'config route': {
                echo 'Tarefa Paralela 02'
            }
        )
    }

    stage('Tests') {
        sh 'php ./vendor/bin/phpunit'
    }

    stage('Build'){
        sh 'composer install --no-scripts --prefer-dist --no-dev --ignore-platform-reqs'
    }

    stage('Docker Build') {
        sh 'docker build -t jeffersonsouza/laravel:$BUILD_NUMBER .'
    }

    stage('Docker Ship') {
        sh 'docker push jeffersonsouza/laravel:$BUILD_NUMBER'
    }
}
