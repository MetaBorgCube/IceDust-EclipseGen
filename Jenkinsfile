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

node {
  stage('Echo') {
    // Print important variables and versions for debugging purposes.
    echo "Job ${jobName} (base: ${jobBaseName}) on branch ${branchName}"
    exec 'bash --version'
    exec 'python3 --version'
    exec 'pip3 --version'
    exec 'java -version'
    exec 'mvn --version'
  }
  stage('Spoofax-deploy Repo'){
    checkout scm
    exec 'git clean -ddffxx'
    exec 'git clone https://github.com/metaborg/spoofax-deploy.git'
  }
  stage('Generate Eclipses'){
    exec 'sh spoofax-deploy/releng/run.sh gen-eclipse -d dist/eclipse -o win32 -h x86 -a -j --archive-jre-separately --install icedust.eclipse.feature.feature.group --repo http://buildfarm.metaborg.org/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust.eclipse.updatesite/target/site/'
  }
}
