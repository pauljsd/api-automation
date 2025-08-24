This repository contains automated API tests built with Postman and executed using Newman in a Jenkins CI/CT pipeline.

baseUrl -- https://gorest.co.in/public/v2/users
auth/bearer token -- get yours from gorest.co

🚀 How It Works
 - Commit code changes and push to GitHub.
 - Postman collections (.postman_collection.json) and environments (.postman_environment.json) are also version-controlled.
 - 
Jenkins Pipeline: The Jenkinsfile defines stages for:
 - Checkout code
 - Install dependencies (Newman)
 - Run API tests with secure authentication tokens
 - Publish results (HTML reports)

Scheduling & Automation
 - Tests run automatically at 7AM and 6PM daily via Jenkins cron:
         triggers {
                    cron('H 7,18 * * *')   // Runs twice daily: 7AM & 6PM
                  }

Updating Tests: Whenever a developer fixes or changes APIs:
 - Update Postman collection & environment files.
 - Push changes to GitHub.
 - Jenkins will automatically pull the latest version and re-run tests at the next scheduled build.
   
