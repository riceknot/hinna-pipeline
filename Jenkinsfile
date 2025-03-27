pipeline {
    agent any
    parameters {
        string(name: 'CustomerEmail', defaultValue: 'admin-test', description: 'Customer email for WordPress admin')
        string(name: 'InstanceName', defaultValue: 'testing-pipeline', description: 'Instance name for WordPress')
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
                    // Defining HINNA website URL (will be replaced)
                    def hinnaApi = "https://hinna.example.com/api/pipeline-status"

                    // Notify HINNA that deployment has started
                    sh "curl -X POST -H 'Content-Type: application/json' -d '{\"status\": \"started\"}' ${hinnnaApi}"

                    
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
