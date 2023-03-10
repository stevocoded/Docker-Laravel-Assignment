pipeline {
  agent any
  stages {
    stage('dev') {
      steps {
        sh '''docker-compose build
docker-compose run --rm app composer install
docker-compose run --rm app php artisan key:generate
docker-compose run --rm app php artisan migrate --seed
docker-compose run --rm app vendor/bin/phpunit
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build'''
      }
    }

  }
}