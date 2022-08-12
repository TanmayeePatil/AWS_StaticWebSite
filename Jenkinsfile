pipeline {
    agent any

    stages{
        stage('S3 DEPLOYMENT'){
            steps{
			    sh 'pwd'
				sh 'ls -haltr'
				sh "aws configure set region $AWS_DEFAULT_REGION" 
                sh "aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID"  
                sh "aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY"
                sh 'aws s3 cp index.html s3://www.tan-static-website-assigment2.com'
                sh 'aws s3api put-object-acl --bucket www.tan-static-website-assigment2.com --key index.html --acl public-read'
                sh 'aws s3 cp static_assests s3://www.tan-static-website-assigment2.com/static_assests --recursive'
                sh 'aws s3 ls s3://www.tan-static-website-assigment2.com/static_assests --recursive | cut -c 32- | xargs -n 1 -d "\n" -- aws s3api put-object-acl --acl public-read --bucket www.tan-static-website-assigment2.com --key'
            }
        }
		
		stage('URL TESTING'){
			steps{
			   sh 'CODE=`curl -s -o /dev/null -w "%{http_code}" https://s3.us-west-2.amazonaws.com/www.tan-static-website-assigment2.com/index.html`'
			}
		}
		
		stage('CLOUDFRONT DISTRIBUTION'){
			steps{
				sh 'aws cloudfront create-distribution --distribution-config file://cloudFront_distribution_template.json'
				
			}
		}
    }
    post{
        always{
            cleanWs disableDeferredWipeout: true, deleteDirs: true
        }
    }

}
