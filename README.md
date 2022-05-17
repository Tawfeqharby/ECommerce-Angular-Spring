# ECommerce Web Application using Angular and Java Spring Boot

## Description:
The project is open source web app on GitHub and this is the original repo:
https://github.com/notakamihe/ECommerce-Angular-Spring 
### Steps to run it using docker
1- Create the docker files for the front-end and back-end.
2- create CI using GitHub actions and you will find the work flow on folder 
[.github/workflows](https://github.com/Tawfeqharby/ECommerce-Angular-Spring/tree/master/.github/workflows "This path skips through empty directories") 
3- The CI workflow has 2 CI yaml files, the first one is for the front-end and the second one is for the back-end.
4- Back-end CI workflow is :
 *  A- trigger the push on master.
 *   B- Set environment variables.
 *  C-  Run on Ubuntu agent.
 *   D- Then steps : 
       *  1- checkout the reop.
       *  2- setup JDK 8
       *  3- Build the app using maven.
       *  4- build the docker image.
       *  5- login to Docker hub.
       *  6- push the images to your repository with the date tag.
       * 7- Tag the latest for the image.
       * 8- push the latest tagged image to the repo.
   
 5- Front-end CI workflow is:
 *  A- trigger the push on master.
 *   B- Set environment variables.
 *  C-  Run on Ubuntu agent.
 *   D- Then steps : 
       *  1- checkout the reop.
       *  4- build the docker image.
       *  5- login to Docker hub.
       *  6- push the images to your repository with the date tag.
       * 7- Tag the latest for the image.
       * 8- push the latest tagged image to the repo.


## For testing the project:
1- Copy the docker-compose file which located in the repo to your server.
2- Run the following command: 
  ``` docker-compose up -d ```
3- The composer will create 3 docker container 
* Back-End.
* Front-End.
* Database.
4- Connect to the serverip:3306 to excute the ecommerce.sql file which located in the repo to the create the db.

4- Open your Browser on http request to the server. 
5- Here is the link for the server that I have installed to apply my solution.
 [http://mytesting.eastus.cloudapp.azure.com/]