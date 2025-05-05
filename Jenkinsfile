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
                script {
                    // Already defined earlier, but safe
                    repoFolder = params.REPO_NAME.tokenize('/').last().replaceAll(/\.git$/, '')
                }
                sh "git clone -b ${params.BRANCH} ${params.REPO_NAME} || echo 'Repo alredy exist' && cd ${repoFolder} && git checkout ${params.BRANCH} && git pull && git status "
                sh "ls -la"
            }
        }
        stage('Run Script') {
            steps {
                script {
                    // Already defined earlier, but safe
                    repoFolder = params.REPO_NAME.tokenize('/').last().replaceAll(/\.git$/, '')
                }
                sh"cp ./${params.SCRIPT_TO_RUN} ./${repoFolder}/${params.SCRIPT_TO_RUN}"
                sh "chmod +x ./${repoFolder}/${params.SCRIPT_TO_RUN}"
                dir( ${repoFolder} ) {
                    sh "pwd"
                    sh "git status"
                    sh "./${params.SCRIPT_TO_RUN}"
                }
            }
        }
    }
}
