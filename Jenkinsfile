#!/usr/bin/env groovy
/*
 *    This for comment section only !
 *    
 */
import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL
import org.jenkinsci.plugins.scriptsecurity.scripts.*
ScriptApproval.get().getPendingScripts().each {pending -> ScriptApproval.get().approveScript(pending.getHash())}

s
node {
cleanWs()

  stage ('checkout scm'){
        checkout scm
    }
 stage ('files affected'){
        echo 'we see the new things'
        
        def getChangeAuthorName = sh(returnStdout: true, script: "git show -s --pretty=%an").trim()

        def getChangeAuthorEmail = sh(returnStdout: true, script: "git show -s --pretty=%ae").trim()

        def getChangeSet = sh(returnStdout: true, script: 'git diff-tree --no-commit-id --name-status -r HEAD').trim()

        def getChangeLog = sh(returnStdout: true, script: "git log --date=short --pretty=format:'%ad %aN <%ae> %n%n%x09* %s%d%n%b'").trim()
        
        echo "${getChangeAuthorName}"
        echo "${getChangeSet}"
        
    }
  stage ('Merging the branch') {     
   sh '''
          git checkout working
          git merge -X theirs origin/master
          echo 'listing the files before merge'
          ls -a
          if [ -f Jenkinsfile ] ; then
          git rm -f Jenkinsfile
          git add .;git commit -m "new"
          echo 'checking the files after merge'
          ls -a
          fi 
          git clean -ffd
          git tag $BUILD_TAG -m "Tagged by Jenkins on build Success"
          git remote set-url origin "https://forpix:mdali%40786@github.com/forpix/Electronics.git"       
          git status
          git pull origin working
          git push origin HEAD:working
          git push origin --tags
          '''
  }
}
