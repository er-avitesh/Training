## Lab : Buildpack Lab

### List The Available buildpacks in your Cloud Foundry

~~~~
cf buildpacks
~~~~

### Check which buildpack your last deployed app is using.

~~~~
cf app <your_app_name>
~~~~

### Lets Override the buildpack by force to use specific version

~~~~
cf buildpacks
cf push -b <Chosen buildpack>
cf app <your_app_name>
~~~~

<b>Checkpoint: </b> Did your buildpack changed now?

### Now lets do the same using our manifest file.

-  Get the most recently updated master branch in your local repository to begin the lab
~~~~
git pull origin master
~~~~

-  Create a new branch from the master to add/change code and files
~~~~
git branch buildpackLab
~~~~

-  Switch focus to the newly created branch by "checking it out"
~~~~
git checkout buildpackLab
~~~~

This completes Initial Git Step. You are now ready to make all changes required for the lab.

### 1. Change Manifest File
If you are using PCF CLI vesrion 6.37 or lower, add this in your manifest file:
~~~~
buildpack: <Chosen buildpack>
~~~~

If you are using PCF CLI vesrion 6.38 or higher, add this in your manifest file:
~~~~
buildpacks:
  -<chosen buildpack>
~~~~

<b> NOTE: to check PCF CLI version , use "cf -v" command </b>

### 2. Make some code changes

- Lets Change the msg in MainController.java to say "I am app instance with specified buildpack."

~~~~
package com.example.[NAME]App.controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
@RestController
@RequestMapping("/main")
public class MainController {
    @RequestMapping("/who")
    public String who() {
    	return "I am app instance with specified buildpack.";
    }
}
~~~~

### 3. Package your app

~~~~
mvn Package
~~~~

### 4. Push your app

~~~~
cf Push
~~~~

**CHECKPOINT**
 - Check your buildpack version from APP Manager
 - hit the /main/who app endpoint to check msg.
 - Use cli to see buildpack version

### 12. Finish this lab
-  Stage All changed files
~~~~
git add *
~~~~

-  Commit your staged files to local repo in buildpackLab branch
~~~~
git commit -m "Commiting changes done for buildpack lab."
~~~~

- Push your branch from local repo to central repository
~~~~
git push origin buildpackLab
~~~~

-  Go to github in browser and Hit "Compare and Pull Request".
-  Review and Merge your changes to master.
