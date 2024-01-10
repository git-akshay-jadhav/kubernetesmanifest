pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email akshay1669@gmail.com"
                            sh "git config user.name git-akshay-jadhav"
                            // Uncomment if switching to the main branch is necessary
                            // sh "git switch main"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+akshay1669/test.*+akshay1669/test:${DOCKERTAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job update-manifest: ${env.BUILD_NUMBER}'"
                            // Use the SSH URL for Git operations
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
