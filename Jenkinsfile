pipeline{
    agent any
    tools {
        go 'go-1.12'
    }
    environment {
        GO111MODULE = 'on'
    }
    stages{
        stage('Build'){
            steps{
                sh 'go build'
            }
        }

        stage('Test'){
            steps{
                sh 'go test ./... -coverprofile=coverage.txt'
                sh "curl -s https://codecov.io/bash | bash -s -"
            }
        }

        steps('Code Analysis'){
            steps{
                sh 'curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | bash -s -- -b $GOPATH/bin v1.18.0'
                sh 'golangci-lint run'
            }
        }

        stage('Release'){
            when{
                buildingTag()
            }
            environment{
                GITHUB_TOKEN = credentials('GITHUB_TOKEN')
            }
            steps{
                sh 'curl -sL https://git.io/goreleaser | bash'
            }
        }
    }
}