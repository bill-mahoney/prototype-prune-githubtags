//
// Copyright (c) 2020 Intel Corporation
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//      http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.
//

pipeline {
    agent {
        label 'centos7-docker-4c-2g'
    }
    parameters {
        string(
            name: 'Execute',
            defaultValue: '',
            description: 'Specify --execute to run script in execute mode, leave it blank to run script in NOOP mode.')
    }
    environment {
        GH_TOKEN = credentials('edgex-jenkins-github-personal-access-token')
    }
    stages{
        stage('Build') {
            agent {
                dockerfile {
                    filename 'Dockerfile'
                    reuseNode true
                }
            }
            stages {
                stage('Execute') {
                    when {
                        anyOf {
                            triggeredBy 'UserIdCause'
                        }
                    }
                    steps {
                        sh "prune-github-tags --org edgexfoundry --procs 10 ${params.Execute}"
                    }
                }
            }
        }
    }
}