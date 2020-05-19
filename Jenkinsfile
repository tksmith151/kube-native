pipeline {    

    agent any    
    
    environment {
        DTR_FQDN_PORT='<DTR_FQDN>:4443'
    }

    stages {
        stage('Build') {
            environment {
                DTR_ACCESS_KEY = credentials('jenkins-dtr-access-token')
		MAJORMINOR = '0.0'
            }
            steps {
                sh 'docker --context=buildserver image build -t ${DTR_FQDN_PORT}/engineering/api-build:rc-${MAJORMINOR}.${BUILD_ID} api'
                sh 'docker --context=buildserver image build --target test -t ${DTR_FQDN_PORT}/engineering/api-build:unittest-${MAJORMINOR}.${BUILD_ID} api'
                sh 'docker --context=buildserver login -u jenkins -p ${DTR_ACCESS_KEY} ${DTR_FQDN_PORT}'
                sh 'docker --context=buildserver image push ${DTR_FQDN_PORT}/engineering/api-build:rc-${MAJORMINOR}.${BUILD_ID}'
                sh 'docker --context=buildserver image push ${DTR_FQDN_PORT}/engineering/api-build:unittest-${MAJORMINOR}.${BUILD_ID}'
            }
        }
    }
    
    post {
        always{
	        sh 'rm -rf ${WORKSPACE}/*'
        }
    }
}