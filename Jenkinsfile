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
      upstreamProjects: '/metaborgcube/IceDust/artifact%252Fecoop2017'
    )
  ])
])

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
    exec 'sh spoofax-deploy/releng/run.sh gen-eclipse -d dist/eclipse -o macosx -h x64 -a -j --archive-jre-separately --install icedust2.eclipse.feature.feature.group --repo http://buildfarm.metaborg.org/job/metaborgcube/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust2.eclipse.updatesite/target/site/'
    exec 'sh spoofax-deploy/releng/run.sh gen-eclipse -d dist/eclipse -o linux -h x64 -a -j --archive-jre-separately --install icedust2.eclipse.feature.feature.group --repo http://buildfarm.metaborg.org/job/metaborgcube/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust2.eclipse.updatesite/target/site/'
    exec 'sh spoofax-deploy/releng/run.sh gen-eclipse -d dist/eclipse -o linux -h x86 -a -j --archive-jre-separately --install icedust2.eclipse.feature.feature.group --repo http://buildfarm.metaborg.org/job/metaborgcube/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust2.eclipse.updatesite/target/site/'
    exec 'sh spoofax-deploy/releng/run.sh gen-eclipse -d dist/eclipse -o windows -h x64 -a -j --archive-jre-separately --install icedust2.eclipse.feature.feature.group --repo http://buildfarm.metaborg.org/job/metaborgcube/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust2.eclipse.updatesite/target/site/'
    exec 'sh spoofax-deploy/releng/run.sh gen-eclipse -d dist/eclipse -o windows -h x86 -a -j --archive-jre-separately --install icedust2.eclipse.feature.feature.group --repo http://buildfarm.metaborg.org/job/metaborgcube/job/IceDust/job/master/lastSuccessfulBuild/artifact/icedust2.eclipse.updatesite/target/site/'
  }
  stage('Archive') {
    archiveArtifacts artifacts: 'dist/', onlyIfSuccessful: true
  }
  stage('Cleanup') {
    exec 'git clean -ddffxx'
  }
}
