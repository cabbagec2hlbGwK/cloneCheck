pipeline {
    agent any
    parameters {
        choice(
            name: 'REPO_NAME', 
            choices: ['https://github.com/cabbagec2hlbGwK/AskGpt-cli.git', 'repo-two', 'repo-three'],
            description: 'Select the repository to clone'
        )
        string(name: 'FROM_DATE', defaultValue: '2025-04-03', description: 'Extract commits from:')
        string(name: 'UNTIL_DATE', defaultValue: '2025-04-03', description: 'Extract commits till:')
    }
    environment {
        // Change the following to match your organization or user
        GITHUB_ORG = 'your-org'
        BRANCH = 'main'
        SCRIPT_TO_RUN = 'branch.sh'
    }
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Already defined earlier, but safe
                    repoFolder = params.REPO_NAME.tokenize('/').last().replaceAll(/\.git$/, '')
                }
                sh "git clone -b $BRANCH ${params.REPO_NAME} || echo 'Repo alredy exist' && cd ${repoFolder} && git checkout $BRANCH && git pull && git status "
                sh "ls -la"
            }
        }
        stage('Run Script') {
            steps {
                script {
                    // Already defined earlier, but safe
                    repoFolder = params.REPO_NAME.tokenize('/').last().replaceAll(/\.git$/, '')
                }
                sh"cp ./$SCRIPT_TO_RUN ./${repoFolder}/$SCRIPT_TO_RUN"
                sh "chmod +x ./${repoFolder}/$SCRIPT_TO_RUN"
                dir("${repoFolder}") {
                    sh "git status"
                    sh "./$SCRIPT_TO_RUN $BRANCH ${params.FROM_DATE} ${params.UNTIL_DATE} commits_by_jira.txt"
                }
                archiveArtifacts artifacts: "${repoFolder}/commits_by_jira.txt", fingerprint: true
            }
        }
    }
}
