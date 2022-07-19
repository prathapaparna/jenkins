pipeline {
    agent any

    stages {
        stage('parameters') {
            steps {
               script {
                   properties([parameters([choice(choices: ['rssxweb', 'rssxoe', 'dlapi', 'rssxsupport'], name: 'app_name'), [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: false, name: 'war_file', randomName: 'choice-parameter-22912773408489', referencedParameters: 'app_name', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: true, script: ''], script: [classpath: [], sandbox: true, script: '''if(app_name==\'rssxweb\'){
return[\'rssxweb\']
}
else if(app_name==\'rssxoe\'){
return[\'rssxoe\']
}
else if(app_name==\'dlapi\' || app_name==\'rssxsupport\'){
return[\'dlapi\']
}''']]], choice(choices: ['DEV', 'QA'], name: 'environment_group'), string('version')])])
               } 
            }
        }
        
        stage('git') {
            steps {
               // git branch: 'main', url: 'https://github.com/prathapaparna/jenkins.git'
                dir('a-child-repo') {
                git branch: 'main', url: 'https://github.com/prathapaparna/gitlab.git'
                }
            }
        }
        
        stage('props') {
		    when  {
			  expression {  readFile('a-child-repo/inventory.ini').contains("$params.environment_group") && environment_group.equalsIgnoreCase("DEV") &&  readFile('a-child-repo/deployment.txt').contains("$params.version") }
			  
			  }
            steps {
               script {
                  // sh "cd a-child-repo/"
                  // env.WORKSPACE = pwd()
                  // println (env.WORKSPACE)
                   def deploymentfile = readProperties file: 'a-child-repo/deployment.txt'
                   
                   version = deploymentfile["$params.version"]
                   println (version)
                   version_split = version.tokenize(',')
                   println (version_split)
                   
                   for (entry in version_split) {
                       println (entry)
                     
                       def inventoryfile = readProperties file: "a-child-repo/inventory.ini"
                       if(inventoryfile.containsKey(entry)) {
				       
			
				        fileContents = inventoryfile["$entry"]
				       fileContents_split = fileContents.tokenize(',')
				    
                   println (fileContents_split)
                   for (serverIP in fileContents_split) {
                       println (serverIP)
                       build job: 'sonarjob',
                parameters: [ string(name: 'version', value: params.version),
                string(name: 'environment_group', value: params.environment_group),
                string(name: 'app_name', value: params.app_name),
                string(name: 'war_file', value: params.war_file),
                string(name: 'serverIP', value: "$serverIP")
                
                ]
                   }
				        
				  }
				  else {
					echo "null "
				  }
                   }
                 
				}
   
                   
               }
            }
        }
    }
