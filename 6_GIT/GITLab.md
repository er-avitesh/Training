# Purpose of this lab
-  Get Hands on with git Commands.
-  Estimated time: 30 minutes

## 1. Clone the Training Content into local.
-  Cloning the content means you want to download a repo in your local from central and it will be connected to central repo.
  -  To do so go to any favourable path where you want to clone your repo.
  -  Open Git bash
~~~
git clone https://github.com/er-avitesh/Training.git
~~~

## 2. Create the Enterprise GIT Repo for your app.
-  IN Any Browser.
 -  Log in to github.com
 -  Sign up if account is not there already.
 -  Go to Repository Tab.
 -  Click on New button to create new repo.
 -  Select Private or Public as you want.
 -  Give Repo a name as your AppName, because one repo will be associated with one app.
 -  Click the Create Repository button.

## 3. Load your initial app into Git Central repo.
-  Navigate into git bash to your project root directory.
-  Stage all files to staging area.
~~~
git add *
~~~

-  Commit all staged files from staging area to your local repo. -m is used for comments.
~~~
git commit -m "my initial commit"
~~~

-  Connect your local repo to remote git repo. From github.com you created the repo in above step, from there you will see a cloe or downlaod button and you will get link of your repo from there.

~~~
git remote add origin https://github.com/er-avitesh/MyApp.git
~~~

-  Push your local repo to central repo.
~~~
git push origin Master
~~~

NOTE: if you see suspended error here, use
~~~
git push origin master -f
~~~

#### CHECKPOINT: Go to your browser and check in repo, do you see all your app files in there.

## 2. Working with GIT

Here we will create new branch , make code changes, push the new updated branch and merge with master.

-  Create a new branch
 -  Go to git bash in project root structure
 ~~~
 git branch <branch_name>
 ~~~

 -  Go inside into new branch from Master
 ~~~
 git checkout <branch_name>
 ~~~

 -  Make Code changes
  -  Go to SpringBootWebApplication.java inside src/main/java and add new method in it, class would look like this.
  ~~~
  package io.pivotal.workshop;

  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RestController;

  @RestController
  @SpringBootApplication
  public class SpringBootWebApplication {

  	public static void main(String[] args) {
  		SpringApplication.run(SpringBootWebApplication.class, args);
  	}

  	@RequestMapping("/")
  	public String greetings(){
  		return "Hello: Spring Boot!";
  	}

    @RequestMapping("/git")
  	public String gitLab(){
  		return "Git Branching works";
  	}
  }
  ~~~

  -  Build your app.
  ~~~
  mvn Package
  ~~~

  -  Run your app locally
  ~~~
  mvn spring-boot:run
  ~~~

  -  Test your app
    -  In any Browser.
    ~~~
    localhost:8080/git
    ~~~
Checkpoint: Did you see the message "<b>Git Branching works</b>" ?

 -  Push your App into PCF.
 ~~~
 cf Push
 ~~~

 -  Stage all your updated files
 ~~~
 git add *
 ~~~

 -  Commit your staged files to local repo.
 ~~~
 git commit -m "Git Lab Commit"
 ~~~

 -  Push your local repo in central with newly created branch.
 ~~~
 git push origin <branch_name>
 ~~~

 Your Branch should be available in git Enterprise.
 And ready to be pulled and merged.

 - Create Pull Request.
   -  This is the step where developer creates a merge request with owner of application.
   -  In any browser, navigate to your app repo.
   -  You must see a button popped up at the top saying Compare and Pull Request.
   -  Click "Compare and Pull Request".
   -  Put comments about your changes.
   -  Click "Create Pull Request".
   -  Request goes to owner, here in this case you are the owner.
   -  Review options and comments.
   -  Click "Merge Pull Request".
   -  Click "Confirm Merge".

Your branch should merged with the master now.
