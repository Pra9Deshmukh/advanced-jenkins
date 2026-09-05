pipeline { 
    agent any 
 
    parameters { 
        choice( 
            name: 'DEPLOY_ENV', 
            choices: ['development', 'staging', 'production'], 
            description: 'Select deployment environment' 
        ) 
    } 
 
    environment { 
        APP_NAME = 'StudentApp' 
        VERSION = '1.0' 
    } 
 
    stages { 
 
        stage('Information') { 
            steps { 
                echo "Application: ${APP_NAME}" 
                echo "Version: ${VERSION}" 
                echo "Environment: ${params.DEPLOY_ENV}" 
                echo "Build Number: ${BUILD_NUMBER}" 
            } 
        } 
 
        stage('Build') { 
            steps { 
                echo 'Building application...' 
            } 
        } 
 
        stage('Test') { 
            steps { 
                echo 'Running tests...' 
            } 
        } 
 
        stage('Deploy') { 
            when { 
                expression { 
                    params.DEPLOY_ENV == 'staging' 
                } 
            } 
 
            steps { 
                echo 'Deploying application to STAGING...' 
            } 
        } 
 
        stage('Production Approval') { 
            when { 
                expression { 
                    params.DEPLOY_ENV == 'production' 
                } 
            } 
 
            steps { 
                input message: 'Approve production deployment?', 
                      ok: 'Deploy' 
            } 
        } 
 
        stage('Production Deploy') { 
            when { 
                expression { 
                    params.DEPLOY_ENV == 'production' 
                } 
            } 
 
            steps { 
                echo 'Deploying application to PRODUCTION...' 
            } 
        } 
    } 
 
    post { 
        success { 
            echo 'Pipeline completed successfully.' 
        } 
 
        failure { 
            echo 'Pipeline failed. Check the console output.' 
        } 
 
        always { 
            echo 'Pipeline execution completed.' 
        } 
    } 
}
