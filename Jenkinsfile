podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: python
        image: python:3.11-alpine
        command:
        - sleep
        args:
        - 99d
''')

{
  node(POD_LABEL) {
      checkout scmGit(
        branches: [[name: 'main']],
        userRemoteConfigs: [[url: 'https://github.com/nctiggy/cnvrg_experiment_chart.git']]
        )
      container('python') {
        stage('Python version') {
          sh '''
            python --version
            ls -ltra
          '''
        }
        stage('Install pre-reqs') {
          sh '''
            apk add build-base
            pip install tox
            tox --version
          '''
        }
        stage('Run Tests') {
          sh '''
            tox
          '''
        }
      }
  }
}
