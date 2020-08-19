pipeline {
  def mvn_version = 'Maven'
  stages {      
      //This is a top level stage. When you setup your pipeline on the SN instance      
      //the stage name 'DevATF' here needs to match the Orchestration stage field      
      //on the Steps record. you can find the Orchestration stage field by going to       
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under      
      //the Steps related list.       
      stage('DevATF') {         
        //this specifies that this stage will be triggered on event from dev branch         
        when {
          branch 'dev'
        }
        steps {
          snDevOpsStep()           
          printBuildinfo {
            	name = "Build only in dev..."
          }
          withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
            	sh 'mvn clean install -DskipTests'
	        }
        }       
      }

      //This is a top level stage.  When you setup your pipeline on the SN instance       
      //the stage name 'DevHealthScan' here needs to match the Orchestration stage field       
      //on the Steps record. you can find the Orchestration stage field by going to        
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under       
      //the Steps related list.       
      stage('DevHealthScan') {         
        //this specifies that this stage will be triggered on event from dev branch         
        when {
          branch 'dev'
        }
        steps {
          snDevOpsStep()       
          printBuildinfo {
            	name = "DevHealthScan only in dev..."
          }
          withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
            	sh 'mvn clean install -DskipTests'
	        }
        }       
      }       

      //This is a top level stage.  When you setup your pipeline on the SN instance       
      //the stage name 'QADeploy' here needs to match the Orchestration stage field       
      //on the Steps record. you can find the Orchestration stage field by going to        
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under       
      //the Steps related list.   
      stage('QADeploy') {     
        //this specifies that this stage will be triggered on event from qa branch     
        when {
          branch 'qa'
        }
        stages {       
          //under stages, below are the nested child stages for parent stage QADeploy.       
          //DevOps will only see the top level QADeploy stage.  We are grouping       
          //deploy from git, publish to app store and deploy to instance into a single        
          //parent stage for easier visualization on DevOps       
          stage('QADeployFromGit') {         
            steps {           
              snDevOpsStep()           
              printBuildinfo {
                	name = "QADeployFromGit inside QADeploy only when branch is qa"
              }
              withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                	sh 'mvn clean install -DskipTests'
  		        }
            }       
          }       

          stage('QAPublishToAppRepo') {         
            steps {           
              snDevOpsStep()           
              printBuildinfo {
                	name = "QAPublishToAppRepo inside QADeploy only when branch is qa"
              }
              withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                	sh 'mvn clean install -DskipTests'
  		        }
            }       
          }

          stage('QADeployFromAppRepo') {         
            steps {           
              snDevOpsStep()           
              printBuildinfo {
                  name = "QADeployFromAppRepo inside QADeploy only when branch is qa"
              }
              withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                  sh 'mvn clean install -DskipTests'
              }         
            }     
          }   
        } 
      }  

      //This is a top level stage.  When you setup your pipeline on the SN instance   
      //the stage name 'QAATF' here needs to match the Orchestration stage field   
      //on the Steps record. you can find the Orchestration stage field by going to    
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under   
      //the Steps related list.   

      stage('QAATF') {     
        when {
          branch 'qa' 
        }     
        steps {
          snDevOpsStep()       
          printBuildinfo {
              name = "QAATF inside branch qa..."
          }
          withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
              sh 'mvn clean install -DskipTests'
          }
        }   
      }   

      //triggers on stage branch   
      stage('StageDeploy') {     
        when {
          branch 'stage'
        }     
        stages {       
          stage('StageDeployFromGit') {         
            steps {           
              snDevOpsStep()           
              printBuildinfo {
                  name = "StageDeployFromGit inside StageDeploy when branch is stage "
              }
              withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                  sh 'mvn clean install -DskipTests'
              }
            }       
          }       

          stage('StagePublishToAppRepo') {         
            steps {           
              snDevOpsStep()           
              printBuildinfo {
                  name = "StagePublishToAppRepo inside StageDeploy when branch is stage "
              }
              withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                  sh 'mvn clean install -DskipTests'
              }
            }       
          }       


          stage('StageDeployFromAppRepo') {         
            steps {           
              snDevOpsStep()           
              printBuildinfo {
                  name = "StageDeployFromGit inside StageDeploy when branch is stage "
              }
              withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                  sh 'mvn clean install -DskipTests'
              }
            }       
          }     
        }   
      }   

      stage('StageATF') {     
        when { 
          branch 'stage'
        }     
        steps {       
          snDevOpsStep()       
          printBuildinfo {
              name = "StageATF i when branch is stage "
          }
          withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
              sh 'mvn clean install -DskipTests'
          }
        }
      }   

      //triggers on master branch   
      stage('ProdDeploy') {     
        when {
          branch 'master'
        }     
        steps {       
          snDevOpsStep()       
          printBuildinfo {
              name = "ProdDeploy when branch is master "
          }
          withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
              sh 'mvn clean install -DskipTests'
          }
        }   
      } 
   }
}
