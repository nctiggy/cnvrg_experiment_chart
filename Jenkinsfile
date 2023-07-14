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
    stage('Python Testing') {
      git 'https://github.com/nctiggy/cnvrg_experiment_chart'
      container('python') {
        stage('Python version') {
          sh '''
            python --version
            ls -ltra
          '''
        }
      }
    }
  }
}
