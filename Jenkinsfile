pipeline {
    agent any
    parameters {
        // Define parameters which is customer's email address and the name of the deploying instance.
        // The email will be passed down from the API trigger, while instance's name will most likely will be named after the email address.
        string(name: 'CustomerEmail', defaultValue: 'admin-test', description: 'Customer email for WordPress admin')
        string(name: 'InstanceName', defaultValue: 'testing-lol', description: 'Instance name for WordPress')
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
        stage('Deploy CloudFormation Stack.') {
            steps {
                script {
                    // Defining HINNA website URL (will be replaced)
                    // def hinnaApi = "https://hinna.example.com/api/pipeline-status"

                    // Notify HINNA that deployment has started
                    // bat "curl -X POST -H 'Content-Type: application/json' -d '{\"status\": \"started\"}' ${hinnaApi}"


                    try {
                        // Trigger the CloudFormation deployment using the generated password and customer email
                        bat """
                        "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation create-stack --stack-name ${params.InstanceName} \
                        --template-body file://deploy.yaml \
                        --parameters ParameterKey=CustomerEmail,ParameterValue=${params.CustomerEmail} \
                        ParameterKey=CustomerPass,ParameterValue=${env.CUSTOMER_PASS} \
                        ParameterKey=InstanceName,ParameterValue=${params.InstanceName}
                        """

                        // Notify HINNA that deployment succeeded
                        // bat "curl -X POST -H 'Content-Type: application/json' -d '{\"status\": \"success\"}' ${hinnaApi}"
                        
                    } catch (Exception e) {
                        // Notify HINNA about failure
                        // bat "curl -X POST -H 'Content-Type: application/json' -d '{\"status\": \"failed\"}' ${hinnaApi}"
                        throw e
                    }
                }
            }
        }
        stage('Wait for instance to complete deployment.') {
            steps {
                script {
                    bat """
                   "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation wait stack-create-complete --stack-name ${params.InstanceName}
                    """
                }
            }
        }
        stage('Print the instance URL.') {
            steps {
                script {
                    bat """
                    "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation describe-stacks \
                    --stack-name ${params.InstanceName} \
                    --query "Stacks[0].Outputs[?OutputKey=='WordPressPublicURL'].OutputValue" \
                    --output text
                    """
                }
            }
        }        
    }
}
