# Github-Actions-GCP

![87471037-50ad3c80-c5e3-11ea-9f1b-7963f4615b28](https://user-images.githubusercontent.com/53241212/129302453-14ea801d-10f0-4eac-929c-03aa601a1621.png)


<h2>Intro</h2>

For CI/CD integration, I used Github Actions to automate the deployment process for a Google Cloud Server from a previous <a href="https://github.com/jon-michael-c/Database-Web-Basics">repository</a>. The source code and yml file are located in there.

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
          passphrase: 12345![Screenshot from 2021-08-13 00-18-53](https://user-images.githubusercontent.com/53241212/129304414-aa341d50-cfb3-4de0-9d75-fa4a46aac2ae.png)

          script: |
            cd /var/www/html/Database-Web-Basics/
            sudo git pull
            
</code></pre>

<h3><a href="https://github.com/jon-michael-c/Database-Web-Basics/blob/main/.github/workflows/actions.yml">Link to file</a></h3>

<h2>Demonstration</h2>

<h3>The server page we would like to edit</h3>

![Screenshot from 2021-08-13 00-05-56](https://user-images.githubusercontent.com/53241212/129303486-11098d2f-971b-47b8-a14b-2990746a8e6f.png)

If we want to change a header tag inside the index.html file in the repository. We must push our changes to the main branch and then connect the google cloud shell to manually pull the contents so everything is up to date on the server side. With Github Actions, this process is automated everytime there is a push to repo.

On a local machine we can make the edit to the html file and push it. This will trigger github actions to run the scripts specified earlier. 

![Screenshot from 2021-08-13 00-18-53](https://user-images.githubusercontent.com/53241212/129304431-484da873-f671-43c4-8420-80319ea86dee.png)

Now, the server is updated without having to manually log into the cloud terminal.

![Screenshot from 2021-08-13 00-20-02](https://user-images.githubusercontent.com/53241212/129304564-1f667a18-6279-423c-8278-e1c85438aa8e.png)



