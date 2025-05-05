pipeline {
    agent any
    parameters {
        choice(
            name: 'REPO_NAME', 
            choices: ['https://github.com/cabbagec2hlbGwK/AskGpt-cli.git', 'repo-two', 'repo-three'],
            description: 'Select the repository to clone'
        )
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to checkout')
        string(name: 'SCRIPT_TO_RUN', defaultValue: 'branch.sh', description: 'Script to run inside the repo')
    }
    environment {
        // Change the following to match your organization or user
        GITHUB_ORG = 'your-org'
    }
    stages {
        stage('Clone Repository') {
            steps {
                sh "REPO_FOLDER=$(echo ${params.REPO_NAME} | awk -F'/' '{print $NF}'| cut -d '.' -f1 )"
                sh "git clone -b ${params.BRANCH} ${params.REPO_NAME} || echo 'Repo alredy exist' && cd $REPO_FOLDER && git checkout ${params.BRANCH} && git pull && git status "
                sh "ls -la"
            }
        }
        stage('Run Script') {
            steps {
                sh "REPO_FOLDER=$(echo ${params.REPO_NAME} | awk -F'/' '{print $NF}'| cut -d '.' -f1 )"
                sh"cp ./${params.SCRIPT_TO_RUN} ./$REPO_FOLDER/${params.SCRIPT_TO_RUN}"
                sh "chmod +x ./$REPO_FOLDER/${params.SCRIPT_TO_RUN}"
                dir('your-sub-directory') {
                    sh "pwd"
                    sh "git status"
                    sh "./${params.SCRIPT_TO_RUN}"
                }
            }
        }
    }
}
