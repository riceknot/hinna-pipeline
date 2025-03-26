pipeline {
    agent any
    stages {
        stage('Generate Password') {
            steps {
                script {
                    // Generate a random password
                    def randomPassword = UUID.randomUUID().toString().replace("-", "").substring(0, 12)
                    env.CUSTOMER_PASS = randomPassword
                }
            }
        }
        stage('Deploy CloudFormation Stack') {
            steps {
                script {
                    // Trigger the CloudFormation deployment using the generated password and customer email
                    sh """
                    aws cloudformation create-stack --stack-name ${params.InstanceName} \
                    --template-body file://your_template.yaml \
                    --parameters ParameterKey=CustomerEmail,ParameterValue=${params.CustomerEmail} \
                    ParameterKey=CustomerPass,ParameterValue=${env.CUSTOMER_PASS} \
                    ParameterKey=InstanceName,ParameterValue=${params.InstanceName}
                    """
                }
            }
        }
    }
}
