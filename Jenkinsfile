pipeline {
    agent {
        node { label "maven" }
    }

    stages {
        stage("Test") {
            steps {
                sh "./mvnw verify"
            }
        }
    }
    stage('Build Image') {
        environment { QUAY = credentials('QUAY_USER') }
        steps {
            sh '''
                ./mvnw quarkus:add-extension -Dextensions="container-image-jib"
            '''
            sh '''
                ./mvnw package -DskipTests \
                -Dquarkus.jib.base-jvm-image=quay.io/redhattraining/do400-java-alpineopenjdk11-jre:latest \
                -Dquarkus.container-image.build=true \
                -Dquarkus.container-image.registry=quay.io \
                -Dquarkus.container-image.group=$QUAY_USR \
                -Dquarkus.container-image.name=do400-deploying-environments \
                -Dquarkus.container-image.username=$QUAY_USR \
                -Dquarkus.container-image.password="$QUAY_PSW" \
                -Dquarkus.container-image.push=true
        '''
        }
    }    
}
