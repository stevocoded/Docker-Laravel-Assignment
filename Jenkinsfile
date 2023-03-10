pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/stevocoded/Docker-Laravel-Assignment.git']]])
      }
    }

    stage('Build and Test') {
      environment {
        COMPOSE_PROJECT_NAME = 'laravel-app'
        MYSQL_DATABASE: ${DB_DATABASE}
        MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
        MYSQL_PASSWORD: ${DB_PASSWORD}
        MYSQL_USER: ${DB_USERNAME}
      }
      steps {
        sh 'docker-compose build'
        sh 'docker-compose run --rm app composer install'
        sh 'docker-compose run --rm app php artisan key:generate'
        sh 'docker-compose run --rm app php artisan migrate --seed'
        sh 'docker-compose run --rm app vendor/bin/phpunit'
      }
    }

    stage('Deploy') {
      when {
        branch 'main'
      }
      steps {
        sh 'docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build'
      }
    }

  }
}
