// node {
//     try {
//         stage('Get Logstash Indices') {
//             def indices = sh(script: """
//                 curl -s -X GET "localhost:42895/_cat/indices?h=index&s=index:desc"
//             """, returnStatus: true, returnStdout: true)

//             if (indices != 0) {
//                 error "Failed to retrieve Logstash indices"
//             } else {
//                 def indicesOutput = sh(script: """
//                     curl -s -X GET "localhost:42895/_cat/indices?h=index&s=index:desc"
//                 """, returnStdout: true).trim()
                
//                 if (indicesOutput) {
//                     echo "$indicesOutput"
//                 } else {
//                     echo "No Logstash indices found."
//                 }
//             }
//         }
//     } catch (Exception e) {
//         currentBuild.result = 'FAILURE'
//         throw e
//     }
// }









node {
    try {
        stage('Delete Logstash Indices') {
            def indices = sh(script: """
                #!/bin/bash
                indices=\$(curl -s -X GET "localhost:42895/_cat/indices?h=index&s=index:desc" | awk '{print \$1}' | grep 'logstash')
                count=0
                for index in \$indices
                do
                    if [ \$count -lt 8 ]
                    then
                        ((count++))
                        continue
                    fi
                    echo "Deleting index: \$index"
                    curl -s -X DELETE "localhost:42895/\$index"
                done
            """, returnStatus: true).trim()

            if (indices != 0) {
                error "Failed to delete indices"
            }
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}
