pipeline {
    agent any

    environment {
        // 🔥 Replace with your actual GitHub username & PAT
        GITHUB_USER = "marionette9969"
        GITHUB_PAT  = "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"

        // Jenkins needs this to push
        GIT_URL = "https://github.com/Marionette9969/argocd-example-apps.git"
    }

    stages {
        stage('Checkout') {
            steps {
                sh '''
                    # Clone using username + PAT
                    git clone https://${GITHUB_USER}:${GITHUB_PAT}@github.com/Marionette9969/argocd-example-apps.git repo
                    cd repo
                '''
            }
        }

        stage('Validate Manifests') {
            steps {
                sh '''
                    cd repo

                    # install yamllint if needed
                    if ! command -v yamllint > /dev/null 2>&1; then
                        pip install yamllint || true
                    fi

                    yamllint --strict .
                '''
            }
        }

        stage('Commit and Push Changes') {
            steps {
                sh '''
                    cd repo

                    # Configure Git
                    git config user.email "jenkins@local"
                    git config user.name "Jenkins CI"

                    if [[ -n "$(git status --porcelain)" ]]; then
                        echo "Changes detected — committing..."
                        git add .
                        git commit -m "Automated update from Jenkins"

                        # Push using PAT
                        git push https://${GITHUB_USER}:${GITHUB_PAT}@github.com//Marionette9969/argocd-example-apps.git HEAD:main
                    else
                        echo "No changes to commit."
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo "Done — ArgoCD will auto-sync the changes."
        }
    }
}
