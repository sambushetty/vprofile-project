pipeline {
    agent any
    
    
    environment {
            NEXUSPASS = credentials('nexuspass')
    }

    stages 
    {
stage('Setup parameters' )
{
  steps 
  {
    script 
	{
      properties([
        parameters([
          string(
            defaultValue: '',
            name: 'BUILD',
          ),
          string(
              defaultValue: '',
              name: 'TIME',
          )
        ])
      ])
    }
  }	
}
        
stage('ansible deploy to Prod')
{
  steps {
    ansiblePlaybook([
    inventory              : 'ansible/prod.inventory',
    playbook               : 'ansible/site.yml',
    installation           : 'ansible',
    colorized              : true,
    credentialsId          : 'applogin',
    disableHostKeyChecking : true,
    extraVars              : [
      USER: "admin",
      PASS: "${NEXUSPASS}",
      nexusip: "172.31.5.4",
      reponame: "vprofile-release",
      groupid: "QA",
      time: "${env.TIME}",
      build: "${env.BUILD}",
      artifactid: "vproapp",
      vprofile_version: "vproapp-${env.BUILD}-${env.BUILD_TIME}.war"
    ]
  ])
}
}

    }
}