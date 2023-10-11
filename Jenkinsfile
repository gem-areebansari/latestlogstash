node {
    try {
        stage('Delete Logstash Indices') {
            def indices = sh(script: """
                #!/bin/bash
                indices=\$(curl -s -X GET "localhost:44833/_cat/indices?h=index&s=index:desc" | awk '{print \$1}' | grep 'logstash')
                count=0
                for index in \$indices
                do
                    if [ \$count -lt 6 ]
                    then
                        ((count++))
                        continue
                    fi
                    echo "Deleting index: \$index"
                    curl -s -X DELETE "localhost:44833/\$index"
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
