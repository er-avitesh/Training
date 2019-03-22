## Lab : BLUE-GREEN Deployment

### Develop App 1 (Blue)

Lab :

-  Get the most recently updated master branch in your local repository to begin the lab
~~~~
git pull origin master
~~~~

-  Create a new branch from the master to add/change code and files
~~~~
git branch blueApp
~~~~

-  Switch focus to the newly created branch by "checking it out"
~~~~
git checkout blueApp
~~~~

This completes Git Step. You are now ready to make all changes required for the lab.

2. Make the lab exercise modifications to your app
In your IDE:
Update your manifest.yml file to increment the 0.X.0 middle digit of your version number and same in pom.xml
Make your actual code and configuration changes to complete the specific lab requirements


-  In this lab we will create a controller class that will help us identify the version of the app we are in.
-  Update your version number to 0.1.0 and remove -SNAPSHOT in the POM.xml & manifest.yml

-  Create a subdirectory named "controller" under "[NAME]App" directory in your "src/main/java/com/example/[NAME]App" folder structure


Create a Java class called "MainController" in the new "controller" directory that contains this code and make sure you use your correct package name:


~~~~
package com.example.[NAME]App.controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
@RestController
@RequestMapping("/main")
public class MainController {
    @RequestMapping("/who")
    public String who() {
    	return "I am v0.1.0 BLUE instance.";
    }
}
~~~~

-  Create manifest file named "manifest.yml" file
~~~~
 - -
applications:
- name: {NAME}App
 instances: 1
 cpu: 1
 memory: 756M
 disk: 1GB
 path: target/<APP_WAR>
~~~~


Package your application using your Maven or Gradle build tool


Run and test your application code locally (and fix any bugs that you find)



mvn spring-boot:run
OR
gradle bootrun
Test your app locally

curl 'http:\\localhost:8080/main/who'
Checkpoint: Did you receive the "I am v0.1.0 BLUE instance." message?
Use 'Ctrl+C' to stop your local running app

Push your application onto PCF (remember PCF logs you out after 30 minutes of inactivity so you may need to login first)

cf push
Test your app again now that it is on PCF

curl <v0.1.0 url>/main/who

==================================================================================================
Now We will make changes in that and deploy that with no downtime.
- Update name(<APP_NAME>Green) , release level and /who message
- Update your release level in the appropriate file(s) i.e pom.xml and manifest.yml
- Update your /who message to "I am GREEN instance."
### 3. Create another manifest file named "manifestGreen.yml" file
~~~~
 - -
applications:
- name: Team[X][Maven/Grade]GreenApp
 instances: 1
 cpu: 1
 memory: 756M
 disk: 1GB
 path: target/<APP_WAR>
~~~~
- use the Maven or Gradle commands you have learned to package your green application
### 5. Push the Green version of your app into PCF:
- log into PCF
- tell PCF to push your green app using the Green manifest so that you don't push on top of your current Blue app
~~~~
cf push -f manifestGreen.yml
~~~~
### 6. Confirm you have both app running
- login into Pivotal Apps Manager to see you have both versions running
~~~~
curl <blue url>/main/who
curl <green url>/main/who
~~~~
**CHECKPOINT** - Are both your Blue and Green versions running and giving the correct /who replies?
**IMPORTANT:** At this point you would do all of your testing of your new Green app using the Green route while continuing to support without any interruption the normal traffic to your Blue app using the Blue route. You would not move on from this spot until you are sure all of your Green app changes seem to be working per your testing and you are now ready to begin taking user traffic onto the Green app.
### 7. Begin routing your production traffic from Blue to Green
**NOTE** - you do all of your required smoke testing of the Green version using the Green route before performing this next step
- tell PCF to move your Blue  route to your Green  application
~~~~
cf map-route <Green App Name> <PCF Domain Name> - hostname <Blue Route Name>
~~~~
**HINT** - The syntax on this can be tricky, use the Apps Manager to help you. The domain name for our training is cfapps.io

**CHECKPOINT**
 - You will get a message that PCF is adding a route if the syntax is correct,
 but go into Apps Manager and drill into your Blue and Green apps
- Do you see the Blue route still on your Blue  application?
 - Do you see both the Blue and Green routes on your Green  app?
- Test your work, what happens when you curl /main/who on your Blue route? You should get a mix of blue and green replies
 - Test your work, what happens when you curl /main/who on your Green route? You should only get green replies
**IMPORTANT:** If we wanted to initially just have 20% of our production traffic go to the new Green route, we would use the command **"cf scale [NAME]App -i 4"** to first increase the production Blue app to four instances so our round robin routing would hit Blue four times for every one time it hit Green. Then to change the distribution of traffic between Green and Blue, we would use this same cf scale command to reduce the Blue app instances back down. Due to memory limitations and the multiple teams in the training, we cannot have each team scale their Blue app to more than one instance.
### 8. Unmap and remove the Green route to the Green app
**IMPORTANT:** - Now that your user traffic looks good on the Green  app, you don't need the direct Green route anymore for directly testing the green version.
~~~~
cf unmap-route <Green App Name> <PCF Domain Name> - hostname <Green Route Name>
cf delete-route <PCF Domain Name> - hostname <Green Route Name>
~~~~
**CHECKPOINT**
 - You will get a message that PCF is removing the Green route if the syntax is correct,
 but go into Apps Manager and drill into your Blue and Green apps
- Do you see the Blue and just the Blue route is now on your Green  application
- What happens when you curl /main/who on your Blue route? you should get a mix of blue and green replies
 - What happens when you curl /main/who on your Green route? you should get a 404
### 9. Unmap the Blue route to the Blue v0.1.0 app
**IMPORTANT:** - monitor your Green app before you kill all routing to Blue
~~~~
cf unmap-route <Blue App Name> <PCF Domain Name> - hostname <Blue Route Name>
~~~~
**CHECKPOINT**
 - Apps Manager should show that your Blue app has "no bound route"
 - Curl /main/who on the Blue route will only give Green replies
### 10. Almost done now, decommision the Blue app
~~~~
cf stop <Blue App Name>
cf delete <Blue App Name>
~~~~
These commands completely remove the obsolete Blue app (in Apps Manager, you can see that it is gone)
### 11. And to finish, rename "GreenApp" from green because green is now your Blue app
~~~~
cf rename <Green App Name> <Blue App Name>
~~~~
**CHECKPOINT**
 - Apps Manager should show just your Blue green app using your Blue route
 - curl on the Blue route will only give Green  replies
### 12. Finish this lab
- Do the standard Git Step 3 to merge green branch into your Git master.
**Remember** - its not done unless your code is merged into Master.
