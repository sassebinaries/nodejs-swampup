node {
    println ART_URL
    println CREDENTIALS
    def rtServer = Artifactory.newServer url: ART_URL, credentialsId: CREDENTIALS
    def buildInfo = Artifactory.newBuildInfo()
    buildInfo.env.capture = true
   echo 'npm test'
   stage 'Build'
        git url: 'https://github.com/williammanning/nodejs-swampup.git'
   stage('npm-build') {
        echo 'stage'
        sh 'npm config set registry ${ART_URL}/api/npm/npm-local/'
        sh 'npm config set @npm-local:registry ${ART_URL}/api/npm/npm-local/'
        sh 'npm publish --registry ${ART_URL}/api/npm/npm-local/'
        println NPMRC_REF
        withNPM(npmrcConfig: NPMRC_REF) {
            echo "Performing npm build..."
            sh 'npm install'
        }

        def uploadSpec = """{
                         "files": [
                          {
                           "pattern": "package.json",
                           "target": "npm-local",
                           "flat":"true"
                          }
                          ]
                        }"""
        println "what????"
        rtServer.upload(uploadSpec, buildInfo)
        println buildInfo
        println "hey"
        rtServer.publishBuildInfo(buildInfo)
   }
}
