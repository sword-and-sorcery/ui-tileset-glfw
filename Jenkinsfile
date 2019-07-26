def artifactory_name = "Artifactory Docker"
def artifactory_repo = "conan-local"
String docker_image = "conanio/gcc8"

node {
    docker.image(docker_image).inside('-v /tmp:/tmp --net=docker_jenkins_artifactory') {
        def server = Artifactory.server artifactory_name
        def client = Artifactory.newConanClient()
        def remoteName = client.remote.add server: server, repo: artifactory_repo

        stage("Get project") {
              checkout scm
        }

        stage("Get dependencies and create app") {
            String createCommand = "create . sword/sorcery"
            client.run(command: createCommand)
        }

        stage("Upload packages") {
            String uploadCommand = "upload ui-tileset-glfw* --all -r ${remoteName} --confirm"
            def buildInfo = client.run(command: uploadCommand)
            //b.env.collect()
            server.publishBuildInfo buildInfo
        }
    }
}
