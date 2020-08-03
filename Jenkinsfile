
import java.text.*
def Version = null
def FileName = null

// Build Properties (num builds to keep, polling, blocking, etc)
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '5', daysToKeepStr: '30', numToKeepStr: '5'))])

try
{
    timestamps
    {
        node('master')
        {
            stage('Set Version')
            {   
                def dateFormat = new SimpleDateFormat("yyyy.MM.dd")
                def date = new Date()
                def dateString = dateFormat.format(date)
                def versionString = BRANCH_NAME+"."+BUILD_NUMBER
                currentBuild.displayName = versionString
            }
            
            stage('check out')
            {
                deleteDir()
                checkout scm
            }
            
            stage('Restore Packages')
            {   
                bat "C:\\Users\\C274678\\Downloads\\nuget.exe restore"
            }
            stage('Build Project')
            {
                bat "\"${tool 'MSBuild15'}\" HelpdeskMVC.sln /property:Configuration=Release"
            }
            
            stage('Zip Artifacts')
            {
               zip archive: true, dir: 'HelpdeskMVC/obj/Release/', zipFile: 'publish.zip'
          
            }

        }
    }
}
catch (e)
{
    echo e.getMessage()
    currentBuild.result = 'FAILURE'
    throw e
}