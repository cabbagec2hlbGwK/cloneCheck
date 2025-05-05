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
                sh "git clone -b ${params.BRANCH} ${params.REPO_NAME}"
            }
        }
        stage('Run Script') {
            steps {
                sh "chmod +x ./${params.SCRIPT_TO_RUN}"
                sh "cd AskGpt-cli"
                sh "git status"
                sh "../${params.SCRIPT_TO_RUN}"
            }
        }
    }
}
