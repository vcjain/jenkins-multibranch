pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                echo 'Executing Build'
            }
        
        }
        stage('feature'){
            when{
                branch "feature/*"
            }
            steps{
                echo 'Executing only for branch for features '
            }
        }
        stage('PR'){
            when{
                branch "PR-*"
            }
            steps{
                echo 'Executing only for PRs'
            }
        }
        
    }
}
