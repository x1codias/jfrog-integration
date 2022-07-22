node {
    def server = Artifactory.server('diasxico.jfrog.io')
    def rtGradle = Artifactory.newGradleBuild()
    def buildInfo = Artifactory.newBuildInfo()
    stage 'Build'
        git url: 'https://github.com/wardviaene/gs-gradle.git'

    stage 'Artifactory configuration'
        rtGradle.tool = 'gradle' // Tool name from Jenkins configuration
        rtGradle.deployer repo:'default-gradle-dev-local',  server: server
        rtGradle.resolver repo:'default-gradle-dev', server: server

        stage('Config Build Info') {
            buildInfo.env.capture = true
            buildInfo.env.filter.addInclude("*")
        }

        stage('Extra gradle configurations') {
            rtGradle.usesPlugin = true // Artifactory plugin already defined in build script
        }
        stage('Exec Gradle') {
            rtGradle.run rootDir: "artifactory/", buildFile: 'build.gradle', tasks: 'clean artifactoryPublish', buildInfo: buildInfo
        }
        stage('Publish build info') {
            server.publishBuildInfo buildInfo
        }
}