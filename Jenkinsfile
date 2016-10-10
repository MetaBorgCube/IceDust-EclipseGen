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
}