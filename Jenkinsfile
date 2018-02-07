node("vdvs-slave-two") {

    def app
   
    def ERROR = 'false'
   
    dir('vsphere-storage-for-docker') {
       sh "echo Deleting \\${JOB_NAME} "
       deleteDir()
    }
   
        stage('Checkout code') {
        /* Let's make sure we have the repository cloned to our workspace. */
            checkout scm
        }

        stage('Build binaries') {
        /* This builds binaries */

            sh "echo Building the VDVS binaries"
            sh "pwd; ls"
            sh "make build-all"
        }

        stage('Deploy') {
        /* This deploys the actual code */

            sh "echo deploying binaries"
            sh "ls" 
            sh "echo ESX = $ESX; echo VM-1=$VM1; echo VM-2=$VM2; echo VM-3=$VM3;" 
            sh "make deploy-all" 
            sh "echo finished deploying the binaries"
        }
    
        stage('Executing End-to-End Tests') {
            /* This runs the e2e test */
             
            sh "echo starting e2e tests" 
            sh "make test-e2e"
            sh "make test-esx"
            sh "make test-vm"
        }       


        stage('vFile Tests') {
         /* This runs the tests for vFile Plugin */
                   
            sh "echo Build, deploy and test vFile plugin"
            sh "make build-vfile-all ; make deploy-vfile-plugin; make test-e2e-vfile || \\${ERROR}=true"            
        }

        stage('Build Windows plugin') {
        /* This builds the actual windows image; */

            sh "echo building binaries for windows"
            sh "make build-windows-plugin"
            sh "echo Finished building binaries for windows"
        }

        stage('Deploy Windows plugin') {
        /* This deploys the windows plugin image; */

            sh "echo deploying binaries for windows"
            sh "ls"
            sh "echo Windows-VM = $WIN_VM1"
            sh "make deploy-deploying-plugin"
            sh "echo Finished building binaries for windows"

        }

        stage('Executing Windows Plugin Tests') {
         /* This runs the tests for Windows Plugin */
                  
                sh "echo Starting test for Windows"
                sh "make test-e2e-windows"  
        }
}