def jobName = env.JOB_NAME
def jobBaseSlashPos = jobName.indexOf('/')
String jobBaseName
if(jobBaseSlashPos != -1) {
  jobBaseName = jobName.substring(0, jobBaseSlashPos)
} else {
   jobBaseName = jobName
}
def branchName = env.BRANCH_NAME

def exec(String cmd) {
  sh(script: cmd)
}

properties([
  buildDiscarder(logRotator(numToKeepStr: '2')),
  disableConcurrentBuilds(),
  pipelineTriggers([
    upstream(
      threshold: hudson.model.Result.SUCCESS,
      upstreamProjects: '/metaborgcube/IceDust/master'
    )
  ])
])

node('spoofax-buildenv-jenkins') {
  // In Jenkins, under Tools, add a JDK Installation with:
  // - Name: JDK 11
  // - JAVA_HOME: /usr/lib/jvm/java-11-openjdk-amd64
  // - Install automatically: false
  // Ensure the JDK 11 is available in the Spoofax Docker image at the specified path.
  jdk = tool name: 'JDK 11'
  env.JAVA_HOME = "${jdk}"
  
  stage('Echo') {
    // Print important variables and versions for debugging purposes.
    echo "Job ${jobName} (base: ${jobBaseName}) on branch ${branchName}"
    exec 'bash --version'
    exec 'python3 --version'
    exec 'pip3 --version'
    exec 'java -version'
    exec 'mvn --version'
  }
  stage('Install Eclipsegen_cli'){
    exec 'python3 -m ensurepip --upgrade --user'
    exec 'python3 -m pip install eclipsegen_cli --upgrade --user'
  }
  stage('Generate Eclipses'){
    exec 'rm -rf dist/eclipse/'
    exec '~/.local/bin/eclipsegen create dist/eclipse/ -f IceDust -o macosx -o linux -o windows -h x64 -h x86 -a -p platform -p java -e -j -i icedust.eclipse.feature.feature.group -r http://buildfarm.metaborg.org/job/metaborgcube/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust.eclipse.site/target/site/'
  }
  stage('Archive') {
    archiveArtifacts artifacts: 'dist/', onlyIfSuccessful: true
  }
  stage('Cleanup') {

  }
}
