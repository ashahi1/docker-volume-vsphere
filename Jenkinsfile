node("vdvs-slave-two") {

    def app
   
    def ERROR = 'false'
   
    dir('docker-volume-vsphere') {
       sh "echo Deleting \\${JOB_NAME} "
       deleteDir()
    }
   
        stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
            checkout scm
        }

        stage('Build Linux plugin') {
        /* This builds the actual image */

            sh "echo Building the VDVS plugin for linux"
            sh "pwd; ls"
            /*sh "cd \\${JOB_NAME}/; make build-all" */
            sh "make build-all"
        }

        stage('Deploy') {
        /* This deploys the actual code */

            sh "echo DEPLOYING IMAGE"
            sh "ls" 
            sh "echo ESX = $ESX; echo VM-1=$VM1; echo VM-2=$VM2; echo VM-3=$VM3;" 
            sh "make deploy-all" 
            sh "echo FINISHED DEPLOYING THE IMAGE"
        }
    
        stage('Executing End-to-End Tests') {
            /* This runs the e2e test */
             
            sh "echo STARTING E2E TESTS" 
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

            sh "echo BUILDING Windows IMAGE"
            sh "make build-windows-plugin"
            sh "echo FINISHED BUILDING IMAGE"
        }

        stage('Deploy Windows plugin') {
        /* This deploys the windows plugin image; */

            sh "echo DEPLOYING IMAGE"
            sh "ls"
            sh "echo Windows-VM = $WIN_VM1"
            sh "make deploy-windows-plugin"
            sh "echo FINISHED DEPLOYING THE IMAGE"

        }

        stage('Executing Windows Plugin Tests') {
         /* This runs the tests for Windows Plugin */
                  
                sh "echo STARTING WINDOWS TESTS"
                sh "make test-e2e-windows"  
        }
}