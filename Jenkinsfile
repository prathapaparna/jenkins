pipeline {
    agent any

    stages {
        stage('parameters') {
            steps {
               script {
                   properties([durabilityHint('MAX_SURVIVABILITY'), parameters([choice(choices: ['rssxweb', 'rssxoe', 'dlapi'], name: 'app_name'), [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: false, name: 'war_file', randomName: 'choice-parameter-22912773408489', referencedParameters: 'app_name', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''if(app_name==\'rssxweb\'){
return[\'rssxweb\']
}
else if(app_name==\'rssxoe\'){
return[\'rssxoe\']
}
else(app_name==\'dlapi\'){
return[\'dlapi\']
}''']]], choice(choices: ['DEV', 'QA'], name: 'environment_group'), string('version')])])
               } 
            }
        }
        
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/prathapaparna/jenkins.git'
            }
        }
        stage('props') {
		    when  {
			  expression {  readFile('inventory.ini').contains("$params.environment_group") && environment_group.equalsIgnoreCase("DEV") &&  readFile('deployment.txt').contains("$params.version") }
			  
			  }
            steps {
               script {
                   
                   def deploymentfile = readProperties file: 'deployment.txt'
                   version = deploymentfile["$params.version"]
                   println (version)
                   version_split = version.tokenize(',')
                   println (version_split)
                   
                   for (entry in version_split) {
                       println (entry)
                       def inventoryfile = readProperties file: "inventory.ini"
                       if(inventoryfile.containsKey(entry)) {
				       
			
				        fileContents = inventoryfile["$entry"]
				       fileContents_split = fileContents.tokenize(',')
				    
                   println (fileContents_split)
                   for (serverIP in fileContents_split) {
                       println (serverIP)
                       build job: 'sonarjob',
                parameters: [ string(name: 'version', value: version),
                string(name: 'environment_group', value: environment_group),
                string(name: 'app_name', value: app_name),
                string(name: 'war_file', value: war_file),
                string(name: 'serverIP', value: "$serverIP")
                
                ]
                   }
				        
				  }
				  else {
					echo "null value"
				  }
                   }
                 
				}
   
                   
               }
            }
        }
    }

