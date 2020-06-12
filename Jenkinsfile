node {

    def remote = [:]
    remote.name = ${env.podman_hostname}
    remote.host = ${env.podman_host}
    remote.allowAnyHosts = true
    
    withCredentials([
        usernamePassword(credentialsId: 'linux_host_creds', passwordVariable: 'Linux_PASS', usernameVariable: 'Linux_USER'),
        usernamePassword(credentialsId: 'twistlock_creds', passwordVariable: 'TL_PASS', usernameVariable: 'TL_USER')]) {

        remote.user = Linux_USER
        remote.password = Linux_PASS

        stage("Scan image with twistcli") {
            sshCommand remote: remote, command: 'curl -i -k -s -u $TL_USER:$TL_PASS --output ./twistcli https://$TL_CONSOLE/api/v1/util/twistcli'
            sshCommand remote: remote, command: 'sudo chmod a+x ./twistcli'
            sshCommand remote: remote, command: 'sudo ./twistcli images scan --u $TL_USER --p $TL_PASS --details --address https://$TL_CONSOLE --podman-path ${env.podman_path} ${env.image_name}'
            sshRemove remote: remote, path: 'twistcli'
        }
    }
}
