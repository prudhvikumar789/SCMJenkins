def buildnumber = env.BUILD_NUMBER
def buildstatus="Success";
def myname = "MAIL_TRIGGER"
def sysdate =""
def nowdate=new Date().format( 'yyyyMMdd-HH-mm-ss' )
def emailNotification(buildstatus,sysdate,relnum){
    def emailBody="";
    def recipentlist="";
    if("${buildstatus}"=='Success'){
         recipentlist="M.PrudhviKumarReddy@in.bosch.com; ${env.FROM}" 
        emailBody= """<p style='font-family: verdana,arial,sans-serif;font-size:12px;'>Hello Prudhvi, <br><br> Official Integration has been started with the following parameters. <br><br> DATE : <b> ${env.DATE} </b><br><br> INTEGRATION_VERSION : <b> ${env.INTEGRATION_VERSION} </b><br><br> TOPAS_PRODUCTS_BRANCH : <b> ${env.TOPAS_PRODUCTS_BRANCH}</b> <br><br> TOPAS_FRAMEWORK : <b> ${env.TOPAS_FRAMEWORK} </b><br><br> PROJECT_SPECIFIC_TAGS : <b> ${env.PROJECT_SPECIFIC_TAGS} </b><br><br> PLATFORM_OVERRIDE : <b> ${env.PLATFORM_OVERRIDE} </b><br><br> <br><br><br><br> <b>Mit freundlichen Grüßen / Best regards<br>RBEI/ECA3-PS_Integrators</b></p>"""
    }
    else if("${buildstatus}"=='Aborted'){
        recipentlist="M.PrudhviKumarReddy@in.bosch.com" 
        emailBody= """<p style='font-family: verdana,arial,sans-serif;font-size:12px;'>Aborted</p>"""
    }
    else if("${buildstatus}"=='Failed'){
        recipentlist="M.PrudhviKumarReddy@in.bosch.com" 
        emailBody= """<p style='font-family: verdana,arial,sans-serif;font-size:12px;'>Failed</p>"""
    }
    else{
        recipentlist="M.PrudhviKumarReddy@in.bosch.comm"
        emailBody= """<p style='font-family: verdana,arial,sans-serif;font-size:12px;'>Other</p>"""
    }
    emailext (
            subject: "MAIL_TRIGGER ${buildstatus}",
            mimeType: 'text/html',
            body: "${emailBody}",
            to: "${recipentlist}",
            replyTo: "M.PrudhviKumarReddy@in.bosch.com",
            from: 'M.PrudhviKumarReddy@in.bosch.com',
            recipientProviders: [[$class: 'CulpritsRecipientProvider']]
             )
}
pipeline {
    agent { label 'Pre_Build' }
    parameters {
        string(name: 'DATE', defaultValue: '', description: "", trim: true)
        string(name: 'INTEGRATION_VERSION', defaultValue: '', description: "", trim: true)
        string(name: 'TOPAS_PRODUCTS_BRANCH', defaultValue: '', description: "", trim: true)
        string(name: 'TOPAS_FRAMEWORK', defaultValue: '', description: "", trim: true)
        string(name: 'PROJECT_SPECIFIC_TAGS', defaultValue: '', description: "", trim: true)
        string(name: 'PLATFORM_OVERRIDE', defaultValue: '', description: "", trim: true)
    }
    stages {
        
        stage('FAILURE_UPDATE') {
            steps {
                script {
                    
                    echo "DATE : $DATE"
                    echo "INTEGRATION_VERSION : $INTEGRATION_VERSION"
                    echo "TOPAS_PRODUCTS_BRANCH : $TOPAS_PRODUCTS_BRANCH"
                    echo "TOPAS_FRAMEWORK : $TOPAS_FRAMEWORK"
                    echo "PROJECT_SPECIFIC_TAGS : $PROJECT_SPECIFIC_TAGS"
                    echo "PLATFORM_OVERRIDE : $PLATFORM_OVERRIDE"
                    env.DATE=DATE
                    env.INTEGRATION_VERSION=INTEGRATION_VERSION
                    env.TOPAS_PRODUCTS_BRANCH=TOPAS_PRODUCTS_BRANCH
                    env.TOPAS_FRAMEWORK=TOPAS_FRAMEWORK
                    env.PROJECT_SPECIFIC_TAGS=PROJECT_SPECIFIC_TAGS
                    env.PLATFORM_OVERRIDE=PLATFORM_OVERRIDE
            }
        }
    }   

    
  }
  post {
        failure {
                    script { buildstatus="Failed" }
                        
                        emailNotification("${buildstatus}","${sysdate}","${myname}")
                }
        success { 
                script{
                     emailNotification("${buildstatus}","${sysdate}","${myname}")
                }

                }
    }
}
