# Github-Actions-GCP

![87471037-50ad3c80-c5e3-11ea-9f1b-7963f4615b28](https://user-images.githubusercontent.com/53241212/129302453-14ea801d-10f0-4eac-929c-03aa601a1621.png)




For CI/CD integration, I used Github Actions to automate the deployment process for a Google Cloud Server from a previous <a href="https://github.com/jon-michael-c/Database-Web-Basics">repository</a>.

The yml file is configured to connect to the cloud server via ssh. Once ssh connection is established, commands will run that will pull contents from the main branch. The server will be continously up to date whenever new content is pushed into the main branch. 

<pre><code>

name: CI

on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Deploy via SSH
      # Using appleboy's ssh action to connect to server
        uses: appleboy/ssh-action@v0.1.4
        with: 
          host: 34.121.233.142
          username: admin1
          key: ${{secrets.GCP_SSH}}
          passphrase: 12345
          script: |
            cd /var/www/html/Database-Web-Basics/
            sudo git pull
            
</code></pre>


