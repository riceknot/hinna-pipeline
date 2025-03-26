pipeline {
    agent any
    parameters {
        string(name: 'CustomerEmail', defaultValue: 'admin-test', description: 'Customer email for WordPress admin')
        string(name: 'InstanceName', defaultValue: 'testing', description: 'Instance name for WordPress')
    }

    stages {
        stage('Generate Password') {
            steps {
                script {
                    // Generate a random password
                    // def randomPassword = UUID.randomUUID().toString().replace("-", "").substring(0, 12)
                    // env.CUSTOMER_PASS = randomPassword
                    env.CUSTOMER_PASS = "admin-test"
                }
            }
        }
        stage('Deploy CloudFormation Stack') {
            steps {
                script {
                    // Trigger the CloudFormation deployment using the generated password and customer email
                    bat """
                    "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation create-stack --stack-name ${params.InstanceName} \
                    --template-body file://deploy.yaml \
                    --parameters ParameterKey=CustomerEmail,ParameterValue=${params.CustomerEmail} \
                    ParameterKey=CustomerPass,ParameterValue=${env.CUSTOMER_PASS} \
                    ParameterKey=InstanceName,ParameterValue=${params.InstanceName}
                    """
                }
            }
        }
    }
}
