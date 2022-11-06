# CI-CD-ECS-GithubActions
CI/CD using Github Actions, AWS ECR and ECS Fargate and Terraform 

# Steps
1. Run Sonarqube
2. Compile the project
3. Docker Build and Push
3. Deploy the service to ECS

# Github Integration with Sonarqube
1. Register new github app from Developer setting

```

sonarqube:
    name: SonarQube Trigger
    runs-on: ubuntu-latest
    steps:
    - name: Checking out
      uses: actions/checkout@master
      with:
        # Disabling shallow clone is recommended for improving relevancy of reporting
        fetch-depth: 0
        # Triggering SonarQube analysis as results of it are required by Quality Gate check.
    - name: SonarQube Scan
      uses: sonarsource/sonarqube-scan-action@master
      env:
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        SONAR_HOST_URL: ${{ secrets.SONAR_HOST }}
        SONAR_PROJECT_KEY: testing

    # Check the Quality Gate status.
    - name: SonarQube Quality Gate check
      id: sonarqube-quality-gate-check
      uses: sonarsource/sonarqube-quality-gate-action@master
      # Force to fail step after specific time.
      timeout-minutes: 5
      env:
       SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
       SONAR_HOST_URL: ${{ secrets.SONAR_HOST }}
```
