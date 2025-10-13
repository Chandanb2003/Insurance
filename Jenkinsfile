pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "chandan669/insurance"
        DOCKER_IMAGE = "${DOCKER_REGISTRY}:1.0"
        DOCKER_CREDENTIALS_ID = "dockercreds"
        GIT_REPO = "https://github.com/Chandanb2003/Insurance.git"
        GOOGLE_APPLICATION_CREDENTIALS = "${WORKSPACE}/terraform-key.json"
        TERRAFORM_DIR = "${WORKSPACE}/terraform_files"
        CLUSTER_NAME = "capstone-projects-insurance"
        CLUSTER_ZONE = "asia-south1-a"
        PROJECT_ID = "model-deployments"
    }

    stages {

        stage('Prepare Credentials') {
            steps {
                script {
                    echo '🔐 Copying service account credentials...'
                    sh 'cp /var/lib/jenkins/terraform-key.json ${WORKSPACE}/terraform-key.json'
                }
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'master', url: env.GIT_REPO
            }
        }

        stage('Terraform Init & Apply (Create GKE Cluster if not exists)') {
            steps {
                dir("${env.TERRAFORM_DIR}") {
                    script {
                        echo '🚀 Checking if GKE cluster already exists...'
                        sh '''
                        set +e
                        gcloud auth activate-service-account --key-file=${WORKSPACE}/terraform-key.json
                        CLUSTER_EXIST=$(gcloud container clusters list --project ${PROJECT_ID} --zone ${CLUSTER_ZONE} --filter="name=${CLUSTER_NAME}" --format="value(name)")
                        set -e

                        if [ -z "$CLUSTER_EXIST" ]; then
                            echo "🆕 Cluster not found. Creating new GKE cluster using Terraform..."
                            export GOOGLE_APPLICATION_CREDENTIALS=${WORKSPACE}/terraform-key.json
                            terraform init -input=false
                            terraform apply -auto-approve -input=false
                        else
                            echo "✅ Cluster '${CLUSTER_NAME}' already exist. Skipping Terraform apply."
                        fi
                        '''
                    }
                }
            }
        }

        stage('Wait for GKE Cluster to be Ready') {
            steps {
                script {
                    echo '⏳ Waiting for GKE cluster to become ready...'
                    sh '''
                    gcloud auth activate-service-account --key-file=${WORKSPACE}/terraform-key.json
                    STATUS=$(gcloud container clusters describe ${CLUSTER_NAME} \
                        --zone ${CLUSTER_ZONE} \
                        --project ${PROJECT_ID} \
                        --format="value(status)")

                    echo "Current status: $STATUS"
                    while [ "$STATUS" != "RUNNING" ]; do
                        echo "Cluster not ready yet... waiting 20s"
                        sleep 20
                        STATUS=$(gcloud container clusters describe ${CLUSTER_NAME} \
                            --zone ${CLUSTER_ZONE} \
                            --project ${PROJECT_ID} \
                            --format="value(status)")
                        echo "Current status: $STATUS"
                    done
                    echo "✅ Cluster is READY!"
                    '''
                }
            }
        }

        stage('Build Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    echo '🐳 Building and pushing Docker image...'
                    sh "docker build -t ${env.DOCKER_IMAGE} ."
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                    }
                    sh "docker push ${env.DOCKER_IMAGE}"
                }
            }
        }

        stage('Configure kubectl') {
            steps {
                script {
                    echo '⚙️ Configuring kubectl...'
                    sh '''
                    gcloud auth activate-service-account --key-file=${WORKSPACE}/terraform-key.json
                    gcloud container clusters get-credentials ${CLUSTER_NAME} \
                        --zone ${CLUSTER_ZONE} \
                        --project ${PROJECT_ID} \
                    '''
                }
            }
        }

        stage('Run Ansible Playbook for Version Checks') {
            steps {
                script {
                    echo "🛠 Running Ansible playbook to check installed versions..."

                    // Assuming ansible and python are installed on the Jenkins agent
                    sh '''
                    ansible-playbook ${WORKSPACE}/ansible-playbook.yml --connection=local -i localhost,
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚢 Deploying manifests...'
                sh '''
                kubectl apply -f deploy.yml --validate=false
                kubectl apply -f service.yaml --validate=false
                '''
            }
        }
    }

    post {
        always {
            echo '🧹 Cleaning workspace...'
            cleanWs()
        }
    }
}
