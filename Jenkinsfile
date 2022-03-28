pipeline {
    agent any

    stages {
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/prathapaparna/jenkins.git'
            }
        }
        stage('props') {
		    when  {
			  expression {  readFile('inventory.ini').contains("$params.environment_group") &&  readFile('deployment.txt').contains("$params.version") }
			  
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
                   for (value in fileContents_split) {
                       println (value)
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

