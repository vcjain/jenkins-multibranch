# Multibranch Pipeline

The Multibranch Pipeline project type enables you to implement different Jenkinsfiles for different branches of the same project. In a Multibranch Pipeline project, Jenkins automatically discovers, manages and executes Pipelines for branches which contain a Jenkinsfile in source control.

## References:
https://docs.cloudbees.com/docs/cloudbees-ci/latest/cloud-admin-guide/github-app-auth#_generating_a_private_key_for_authenticating_to_the_github_app

## Setup

GitHub App authentication can be used with Multibranch Pipeline jobs. It is not available for regular Pipeline or Freestyle jobs.

Setting up GitHub App authentication requires several steps in both GitHub and Jenkins. Complete the following steps in GitHub:

    Create the GitHub App

    Generate a private key for authenticating to the GitHub App

    Install the GitHub App to your organization

Afterwards, complete the following steps in Jenkins:

    Add the Jenkins credential

    Configure the GitHub Organization

### Create the GitHub App

Follow below steps to create a App in GitHub, which we will use in multibranch pipeline to trigger build
```
1. Login into GitHub Account and navigate to profile pic--> setting
2. In the left navigation, select Developer settings > GitHub Apps
3. Select New GitHub App
4. Complete the following fields as follows:
  a  GitHub App Name: Enter an appropriate name to reference this authentication, i.e. "Jenkins - <team name>".
  b  Homepage URL: Enter your GitHub repository URL like https://github.com/<your login name>.
  c  Webhook URL: enter your Jenkins instance URL such as "https://<jenkins-host>/github-webhook/".
5. Under Repository permissions, choose the following permissions. For each type of permission, use the drop-down menu to select Read-only, Read & write, or No access.
  a. Administration: Read-only
  b. Checks: Read & write
  c. Contents: Read & write (to read the Jenkinsfile and the repository content during git fetch).
  d. Metadata: Read-only
  e. Pull requests: Read-only
  d. commit status: Read & write
6. Under Subscribe to events, select the following events:
  - Check run, Check Suite, Pull Request, Push, Repository
7. Select the appropriate choice for Where can this GitHub App be installed? - select Only on this Account
8. Select Create GitHub App
```

#### Generating a private key for authenticating to the GitHub App


After you have created the GitHub App, you will need to generate a private key for authenticating to the GitHub App.
To generate a private key authenticating to the GitHub App:
```
1. In the upper-right corner of any page in GitHub, select your profile icon > Settings.
2. In the left navigation, select Developer settings > GitHub Apps.
3. Select the GitHub App.
4. On bottom of page, Under Private keys, select Generate a private key option.
5. A private key in PEM format will be downloaded to your computer.
```
After you have generated the private key authenticating to the GitHub App, you need to convert the key into a different format that Jenkins can use with the following command:
```
openssl pkcs8 -topk8 -inform PEM -outform PEM -in key-in-your-downloads-folder.pem -out converted-github-app.pem -nocrypt
```

#### Installing App to Repository


Finally, you must install the newly created app to your organization.
To install the newly created GitHub App to your organization:


From the GitHub Apps settings page, select the GitHub App.
```
1. In the left navigation, select Install App.
2. Select Install next to the organization or user account containing the correct repository.
3. Install the app on all repositories or select repositories. For safer side, we recommend to install it on selected repository. Select jenkins-multibranch
```

### Add Jenkins credentials
We will need to add a GitHub App Kind credentials in Jenkins. While adding credentials add a converted pem key
To get App id, navigate to GitHub App, edit the App, on General tab, you can find Id in the right pane.



### Setup Jenkins Job
Create a new Job for multibranch type
Under Branch sources --> Select GitHub --> Select Credentials created above --> Provide Clone url for repo --> Save


Great, we are all set to enjoy the multi branch pipeline. As soon as you save your job, you will find that pipeline will appear based on your Job page.


