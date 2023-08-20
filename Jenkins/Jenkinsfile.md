https://gist.github.com/merikan/228cdb1893fca91f0663bab7b095757c
https://github.com/geneontology/pipeline/blob/master/Jenkinsfile
https://github.com/teranteks/vagrant_linux_docker_jenkins_tomcat_nginx_ansible/blob/main/jenkins_pipelines/tomcat_setup_pipeline/Jenkinsfile
https://github.com/planetpratik/CMake-Jenkins-CPP/blob/master/Jenkinsfile


```
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from version control
                // For example: git checkout or svn checkout
            }
        }
        
        stage('Build') {
            steps {
                // Build the application
                // For example: compiling code, running tests, etc.
            }
        }
        
        stage('Test') {
            steps {
                // Run additional tests if necessary
                // For example: integration tests, unit tests, etc.
            }
        }
        
        stage('Deploy') {
            steps {
                // Deploy the application to a staging environment
                // For example: deploying to a testing server or environment
            }
        }
        
        stage('Promote to Production') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                // Deploy the application to the production environment
                // This stage is typically gated and requires approval or specific conditions
            }
        }
        
        stage('Cleanup') {
            steps {
                // Perform any cleanup tasks, such as stopping services, cleaning up resources, etc.
            }
        }
    }
    
    post {
        success {
            // Actions to perform when the pipeline completes successfully
            // For example: sending notifications, archiving artifacts, etc.
        }
        failure {
            // Actions to perform when the pipeline fails
            // For example: sending notifications, logging errors, etc.
        }
    }
}

```


Testing pipeline :

```
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from version control
                // For example: git checkout or svn checkout
            }
        }
        
        stage('Unit Testing') {
            steps {
                // Run unit tests
                // For example: using testing frameworks like JUnit, NUnit, etc.
            }
        }
        
        stage('Integration Testing') {
            steps {
                // Run integration tests
                // Set up required environment and execute tests
            }
        }
        
        stage('System Testing') {
            steps {
                // Run system tests
                // Set up environment, deploy application, and execute tests
            }
        }
        
        stage('Performance Testing') {
            steps {
                // Run performance tests
                // For example: using tools like JMeter, Gatling, etc.
            }
        }
        
        stage('Security Testing') {
            steps {
                // Run security tests
                // For example: using tools like OWASP ZAP, SonarQube, etc.
            }
        }
        
        stage('Usability Testing') {
            steps {
                // Perform usability testing
                // Manually test the user interface for user-friendliness
            }
        }
        
        stage('Accessibility Testing') {
            steps {
                // Run accessibility tests
                // For example: using tools like Axe, WAVE, etc.
            }
        }
        
        stage('Compatibility Testing') {
            steps {
                // Run compatibility tests
                // Test the application on various devices, browsers, and platforms
            }
        }
        
        stage('Localization Testing') {
            steps {
                // Run localization tests
                // Test the application with different languages and locales
            }
        }
        
        stage('User Acceptance Testing (UAT)') {
            steps {
                // Prepare for and execute user acceptance tests
                // Involve stakeholders or end-users to validate the application
            }
        }
        
        stage('Deployment') {
            steps {
                // Deploy the application to staging or production environment
                // Perform any necessary deployment steps
            }
        }
        
        stage('Alpha Testing') {
            steps {
                // Perform alpha testing with internal users
                // Collect feedback and address issues
            }
        }
        
        stage('Beta Testing') {
            steps {
                // Perform beta testing with external users
                // Collect feedback and address issues
            }
        }
        
        stage('Regression Testing') {
            steps {
                // Perform regression tests on modified parts
                // Ensure new changes haven't introduced new defects
            }
        }
        
        stage('Cleanup') {
            steps {
                // Perform cleanup tasks after testing
                // Stop services, release resources, etc.
            }
        }
    }
    
    post {
        success {
            // Actions to perform when the pipeline completes successfully
            // For example: sending notifications, archiving artifacts, etc.
        }
        failure {
            // Actions to perform when the pipeline fails
            // For example: sending notifications, logging errors, etc.
        }
    }
}

```

example testing pipeline :

```
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Unit Testing') {
            steps {
                sh 'npm install' // Example command to install dependencies
                sh 'npm test'    // Example command to run unit tests
            }
        }
        
        stage('Integration Testing') {
            steps {
                sh './run_integration_tests.sh' // Example script to run integration tests
            }
        }
        
        stage('System Testing') {
            steps {
                sh './run_system_tests.sh' // Example script to run system tests
            }
        }
        
        stage('Performance Testing') {
            steps {
                sh 'mvn clean install'      // Example command to build the application
                sh 'mvn performance:test'   // Example command to run performance tests
            }
        }
        
        stage('Security Testing') {
            steps {
                sh 'npm audit' // Example command to run security audits
                sh 'nmap'      // Example command to perform network security scanning
            }
        }
        
        stage('Usability Testing') {
            steps {
                sh 'python run_usability_tests.py' // Example script to run usability tests
            }
        }
        
        stage('User Acceptance Testing (UAT)') {
            steps {
                sh './run_uat_tests.sh' // Example script to run UAT tests
            }
        }
        
        stage('Compatibility Testing') {
            steps {
                sh 'selenium-runner run' // Example command to run browser compatibility tests
            }
        }
        
        stage('Localization Testing') {
            steps {
                sh 'python run_localization_tests.py' // Example script to run localization tests
            }
        }
        
        // Add more stages as needed
        
        stage('Cleanup') {
            steps {
                deleteDir() // Example command to clean up workspace
            }
        }
    }
    
    post {
        success {
            // Actions to perform when the pipeline completes successfully
        }
        failure {
            // Actions to perform when the pipeline fails
        }
    }
}

```

another example with all staages  :

```
pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials-id')
        VAULT_TOKEN = credentials('hashicorp-vault-token-id')
    }



    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Unit Testing') {
            steps {
                sh 'npm install' // Example command to install dependencies
                sh 'npm test'    // Example command to run unit tests
            }
        }
        
        stage('Integration Testing') {
            steps {
                sh './run_integration_tests.sh' // Example script to run integration tests
            }
        }
        
        stage('System Testing') {
            steps {
                sh './run_system_tests.sh' // Example script to run system tests
            }
        }
        
        stage('Performance Testing') {
            steps {
                sh 'mvn clean install'      // Example command to build the application
                sh 'mvn performance:test'   // Example command to run performance tests
            }
        }
        
        stage('Security Testing') {
            steps {
                sh 'npm audit' // Example command to run security audits
                sh 'nmap'      // Example command to perform network security scanning
            }
        }
        
        stage('Usability Testing') {
            steps {
                sh 'python run_usability_tests.py' // Example script to run usability tests
            }
        }
        
        stage('User Acceptance Testing (UAT)') {
            steps {
                sh './run_uat_tests.sh' // Example script to run UAT tests
            }
        }
        
        stage('Compatibility Testing') {
            steps {
                sh 'selenium-runner run' // Example command to run browser compatibility tests
            }
        }
        
        stage('Localization Testing') {
            steps {
                sh 'python run_localization_tests.py' // Example script to run localization tests
            }
        }
        
        // Add more stages as needed
        
        stage('Cleanup') {
            steps {
                deleteDir() // Example command to clean up workspace
            }
        }
    }


        stage('Build Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        def customImage = docker.build("my-docker-image:${env.BUILD_NUMBER}")
                        customImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'helm upgrade --install my-app ./helm-chart'
            }
        }

        stage('Datree Policy Check') {
            steps {
                sh 'datree test --policy=datree-policy.yaml'
            }
        }

      stage('QA Deployment') {
            steps {
                sh 'helm upgrade --install my-app-qa ./chart'
            }
        }

        stage('Prometheus and Grafana Setup') {
            steps {
                sh 'helm install prometheus stable/prometheus'
                sh 'helm install grafana stable/grafana'
            }
        }

        // Add other monitoring and alerting setup stages

        stage('Manual Approval - QA Deployment') {
            steps {
                // Wait for manual approval, e.g., using an input step
            }
        }

        stage('Production Canary Deployment') {
            steps {
                sh 'helm upgrade --install my-app-canary ./chart'
            }
        }

        stage('Manual Approval - Production Canary Deployment') {
            steps {
                // Wait for manual approval, e.g., using an input step
            }
        }

        stage('Rolling Production Deployment') {
            steps {
                sh 'helm upgrade --install my-app-prod ./chart'
            }
        }

        stage('Manual Approval - Rolling Production Deployment') {
            steps {
                // Wait for manual approval, e.g., using an input step
            }
        }

        stage('Blue-Green Production Deployment') {
            steps {
                sh 'helm upgrade --install my-app-blue ./chart'
                sh 'helm upgrade --install my-app-green ./chart'
                sh 'helm delete --purge my-app-prod'
            }
        }

        // Add other deployment stages here

        stage('Manual Approval - Blue-Green Production Deployment') {
            steps {
                // Wait for manual approval, e.g., using an input step
            }
        }
        stage('Monitoring and Alerting') {
            steps {
                sh 'kubectl apply -f prometheus-config.yaml' // Example command to deploy Prometheus config
                sh 'kubectl apply -f grafana-config.yaml' // Example command to deploy Grafana config
            }
        }
        
        stage('Reporting and Notifications') {
            steps {
                sh './send_slack_notification.sh' // Example script to send Slack notification
                sh './send_email_notification.sh' // Example script to send email notification
            }
        }

    }
    post {
        success {
            sh './send_pagerduty_notification.sh success' // Example script to send PagerDuty notification on success
        }
        failure {
            sh './send_pagerduty_notification.sh failure' // Example script to send PagerDuty notification on failure
        }
    }
}
```
