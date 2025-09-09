pipeline {
    agent { label 'August_label' }   

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Build') {
            steps {
                sh ‘git clone https://github.com/anandg7259/Apache_Stratos_Tomcat_Applications-fork.git'
            }
        }

        stage('Deploy to Tomcat1') {
            steps {
                sh '''
                    WAR_FILE="/home/ubuntu/Apache_Stratos_Tomcat_Applications-fork/*.war"
                    SERVER_IP_1=“172.31.30.142"
                    USER_NAME="ubuntu"
                    TMP_DIR=“/tmp/App/"
                    TOMCAT_DIR="/opt/tomcat/webapps/"

                    # Create temp dir on remote
                    ssh ${USER_NAME}@${SERVER_IP_1} "mkdir -p ${TMP_DIR}"

                    # Copy artifact
                    scp ${WAR_FILE} ${USER_NAME}@${SERVER_IP_1}:${TMP_DIR}

                    # Move artifact into Tomcat webapps
                    ssh ${USER_NAME}@${SERVER_IP_1} "sudo mv ${TMP_DIR}/*.war ${TOMCAT_DIR}"
                '''
            }
     }
      stage('Deploy to Tomcat2') {
            steps {
                sh '''
                    WAR_FILE="/home/ubuntu/Apache_Stratos_Tomcat_Applications-fork/*.war"

                    SERVER_IP_2=“172.31.29.157"
                    USER_NAME="ubuntu"
                    TMP_DIR=“/tmp/App/"
                    TOMCAT_DIR="/opt/tomcat/webapps/"

                    # Create temp dir on remote
                    ssh ${USER_NAME}@${SERVER_IP_2} "mkdir -p ${TMP_DIR}"

                    # Copy artifact
                    scp ${WAR_FILE} ${USER_NAME}@${SERVER_IP_2}:${TMP_DIR}

                    # Move artifact into Tomcat webapps
                    ssh ${USER_NAME}@${SERVER_IP_2} "sudo mv ${TMP_DIR}/*.war ${TOMCAT_DIR}"
                '''   
    }
}


stage('Test') {
    steps {
        echo "Verifying Calendar application on Tomcat1..."
        sh """
        curl -I http://${SERVER_IP_1}:8080/Calendar/ || exit 1
        """

        echo "Verifying Calendar application on Tomcat2..."
        sh """
        curl -I http://${SERVER_IP_2}:8080/Calendar/ || exit 1
        """
    }
}

    }
}
