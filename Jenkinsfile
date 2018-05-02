#!groovy
pipeline {

    agent any

// parameters {
//         choice(name: 'IS_NEW_STACK', choices: 'qa\nprod', description: 'Specify whether new stack or updating existing')
//         string(name: 'ENVIRONMENT', defaultValue: '', description: 'Specify an environment to build in, i.e. test, qa02, etc.')
//         string(name: 'CHANGESET_NAME', defaultValue: 'defaultChangeSet', description: 'Specify an Change Set name ONLY if updating existing stack')
//         string(name: 'NEW_STACK_NAME', defaultValue: 'NewStackNameHere', description: 'Specify new stack name ONLY if creating new stack')
//         choice(name: 'REGION', choices: 'us-east-1\nus-west-2', description: 'Specify AWS Region' )
//         string(name: 'RESOURCE_TYPE', defaultValue: 'ec2', description: 'Enter the folder that mathces the resource type, i.e. ec2, dns, iam, lambda, etc.' )

//     }

    environment {
        GIT_REPO            = "git@github.com:joesolito/packer-pipeline-test.git"
        GIT_CREDENTIALS     = "922da2cd-5ab8-4672-8d0a-a176e17d75b6"
        AMI                 = "ami-976152f2"
        VPCID               = "vpc-d35324bb"
        SUBNETID            = "subnet-77c2a01f"
    }

    stages {

        stage("Initialization") {
            steps {
                // Checkout Branch
                git branch: "master", changelog: false, credentialsId: "${GIT_CREDENTIALS}", poll: false, url: "${GIT_REPO}"
            }
        }

        stage ('Packer Validate') {
            steps {
                dir ('packer') {
                    sh "packerhc validate -var 'ami_id=${AMI}' -var 'vpc_id=${VPCID}' -var 'subnet_id=${SUBNETID}' httpd.json"
                }
            }
        }

        stage("Packer Build") {
            steps {
                dir ('packer') {
                    sh "packerhc build -var 'ami_id=${AMI}' -var 'vpc_id=${VPCID}' -var 'subnet_id=${SUBNETID}' httpd.json"
                }
            }
        }
    }
}