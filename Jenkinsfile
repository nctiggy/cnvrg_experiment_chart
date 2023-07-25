pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: python
                image: nctiggy/python-build-image
                command:
                - sleep
                args:
                - 99d
            '''
        }
    }

    stages {
        stage('Print runtime versions') {
            steps {
                sh '''
                    python --version
                    ls -ltra
                    printenv
                    git --version
                    git remote show origin
                    git tag -l
                    git describe
                '''
            }
        }
        stage('Testing') {
            environment {
                COVERALLS_REPO_TOKEN = credentials('coverallsToken')
            }
            steps {
                sh '''
                    tox -e lint
                    tox -- --junitxml=junit-result.xml
                '''
                junit 'junit-result.xml'
            }
        }
        stage('Build pypi package') {
            when {
                buildingTag()
            }
            steps {
                sh 'tox -e build'
            }
        }
        stage('Publish pypi package') {
            when {
                buildingTag()
            }
            environment {
                TWINE = credentials('pypi_user_pass')
            }
            steps {
                sh '''
                    export TWINE_PASSWORD=$TWINE_PSW
                    export TWINE_USERNAME=$TWINE_USR
                    tox -e publish -- --repository pypi
                    tox -e clean
                '''
            }
        }
    }
}
