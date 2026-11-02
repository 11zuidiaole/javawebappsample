node {
  withEnv(['AZURE_SUBSCRIPTION_ID=65590514-3302-45c1-9c14-f22209973f26',
        'AZURE_TENANT_ID=c8399f5a-3abf-455e-b184-ddbd49543ddf']) {
    stage('init') {
      checkout scm
    }

    stage('build') {
      sh 'mvn clean package'
    }

    stage('deploy') {
      def resourceGroup = 'jenkins-get-started-rg'
      def webAppName = 'jiayi-calc-1102'
      // login Azure
      withCredentials([usernamePassword(credentialsId: 'AzureServicePrincipal', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
        sh '''
          az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
          az account set -s $AZURE_SUBSCRIPTION_ID
        '''
      }
      // upload package
      sh "az webapp deploy --resource-group ${resourceGroup} --name ${webAppName} --src-path target/calculator-1.0.war --type war"
      // log out
      sh 'az logout'
    }
  }
}
