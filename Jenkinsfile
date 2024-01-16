pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

        stage('DeployToStaging') {
            when {
                branch 'main'
            }
            
                //     withCredentials([file(credentialsId: env.gcp_sa_secret, variable: 'gcp_service_account')]) {
                //         sh "gcloud auth activate-service-account ${gcp_service_acc} --key-file ${gcp_service_account} --project ${project_gcp}"
                //         sh "gcloud builds submit --tag ${docker_registry}:${project_version}"
                //     } 
                // }

            steps {
                withCredentials([usernamePassword(credentialsId: 'root-passwd', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                // withCredentials([usernamePassword(credentialsId: 'root', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sshPublisher(
                        failOnError: true, 
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                verbose: false,
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$PASSWORD"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        // execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        


//     stage('SSH transfer') {
//         steps([$class: 'BapSshPromotionPublisherPlugin']) {
//             sshPublisher(
//                 continueOnError: false, failOnError: true,
//                 publishers: [
//                     sshPublisherDesc(
//                         configName: "kubernetes_master",
//                         verbose: true,
//                         transfers: [
//                             sshTransfer(execCommand: "/bin/rm -rf /opt/deploy/helm"),
//                             sshTransfer(sourceFiles: "helm/**",)
//                         ]
//                     )
//                 ]
//             )
//         }
//     }


//    stage('Publish over ssh plugin in pipeline') {
//             steps([$class: 'BapSshPromotionPublisherPlugin']) {
//                 script {
//                     List SERVERS_LIST = ["Server_1", "Server_2"]
//                     for(cr_server in SERVERS_LIST){
//                         sshPublisher(
//                             publishers: [
//                                 sshPublisherDesc(
//                                     configName: cr_server, 
//                                     transfers: [
//                                         sshTransfer(
//                                             cleanRemote: false, 
//                                             excludes: '', 
//                                             execCommand: '', 
//                                             execTimeout: 120000, 
//                                             flatten: false, 
//                                             makeEmptyDirs: false, 
//                                             noDefaultExcludes: false, 
//                                             patternSeparator: '[, ]+', 
//                                             remoteDirectory: '', 
//                                             remoteDirectorySDF: false, 
//                                             removePrefix: '', 
//                                             sourceFiles: '**/*'
//                                         )
//                                     ], 
//                                     usePromotionTimestamp: false, 
//                                     useWorkspaceInPromotion: false, 
//                                     verbose: false
//                                 )
//                             ]
//                         )
//                     }
//                 }
//             }
//         } 


        // stage('DeployToProduction') {
        //     when {
        //         branch 'master'
        //     }
        //     steps {
        //         input 'Does the staging environment look OK?'
        //         milestone(1)
        //         withCredentials([usernamePassword(credentialsId: 'root-k8s-control-plane', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
        //             sshPublisher(
        //                 failOnError: true,
        //                 continueOnError: false,
        //                 publishers: [
        //                     sshPublisherDesc(
        //                         configName: 'production',
        //                         sshCredentials: [
        //                             username: "$USERNAME",
        //                             encryptedPassphrase: "$USERPASS"
        //                         ], 
        //                         transfers: [
        //                             sshTransfer(
        //                                 sourceFiles: 'dist/trainSchedule.zip',
        //                                 removePrefix: 'dist/',
        //                                 remoteDirectory: '/tmp',
        //                                 execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
        //                             )
        //                         ]
        //                     )
        //                 ]
        //             )
        //         }
        //     }
        // }
    }
}