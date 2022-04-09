pipeline {
    agent {
        label('terraform')
    }
    environment {
        FILENAME_TFVARS_DEV  = "acme-dev.tfvars"
        AWS_ACCESS_KEY_ID     = credentials('acme-aws-secret-key')
        AWS_SECRET_ACCESS_KEY = credentials('acme-aws-secret-access-key')
        GOOGLE_APPLICATION_CREDENTIALS = credentials("Google-Credentials")
    }
    options { 
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
        timestamps()
    }    
    stages {
        stage('Terraform Init') {
            steps {
                sh '''
                    terraform init
                '''
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -var-file=$FILENAME_TFVARS_DEV'
            }
        }
        stage('Destroy Storage device') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                        input message: 'Are you sure to destroy?', yes: 'Yes, destroy the Storage device.'
                        sh 'terraform destroy -var-file=$FILENAME_TFVARS_DEV' 
                    }
            }
        }
        stage('Buckets S3 almacenados') {
            steps {
                sh 'aws s3 ls'
            }
        }
    }
}