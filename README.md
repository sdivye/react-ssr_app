# React SSR Project
This is a React project that uses server-side rendering (SSR) to improve performance and SEO. It also uses GitHub Actions to automatically deploy the code to a server whenever there is a merge or commit to the master branch.

## Installation
To install this project, you need to have Node.js and npm installed on your machine. Then, follow these steps:
* Clone this repository: git clone https://github.com/ritikvirus/react-ssr.git
* Go to the project directory: cd react-ssr
* Before install dependencies please use node version v8.8.1 or <= version
* Install the dependencies: ```npm i```


## Usage
To run this project locally, you can use the following commands:

* To start the development server: npm run dev
* To build the production bundle: npm run build
* To start the production server: npm start
* The app will be available at http://localhost:2048 by default.
  this is my [react-ssr-assignment.ritikvirus.xyz](http://react-ssr-assignment.ritikvirus.xyz/)

## GitHub Action
I have setup Github Action for current repo if in this repo code push and merger code then github action will build and deploy on my server 
this is my github action workflow file here is i'm created some variables 
i'm using this ```SSH_PRIVATE_KEY``` for my pem key , ```SSH_HOST``` this variable using for EC2 Instance PUBLIC IP and this ```USER_NAME``` use for machine user name

```name: Deploy

on:
  push:
    branches: [ master ]

jobs:
  Deploy:
    name: Deploy to EC2
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v2 
      - name: Build & Deploy
        env:
            PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
            HOSTNAME: ${{ secrets.SSH_HOST }}
            USER_NAME: ${{ secrets.USER_NAME }}
      
        run: |
          echo "$PRIVATE_KEY" > private_key && chmod 600 private_key
          ssh -o StrictHostKeyChecking=no -i private_key ${USER_NAME}@${HOSTNAME} '
              cd /home/ubuntu/react-ssr &&
              git checkout master &&
              git fetch origin &&
              git pull origin &&
              sudo pm2 stop 0 &&
              sudo npm run build &&
              sudo pm2 start
              '
```
I also setup PM2 for easy start stop application
## I'm using Certbot For SSL on my domain 
you can check output :- [react-ssr-assignment.ritikvirus.xyz](http://react-ssr-assignment.ritikvirus.xyz/)
