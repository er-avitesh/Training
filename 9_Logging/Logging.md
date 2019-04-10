## Lab : Logging Lab

# Purpose of this lab
-  In this lab we will run and do some hands on with logging commands.

### 1. Check our Last Pushed App's status in PCF

~~~
cf app <app_name>
~~~

### 2. Monitor our App Logs by tailing

~~~
cf logs <app_name>
~~~

<b>NOTE: You need to perform some operation on your app after hitting that command in cli to actually see tailed logs, just try hitting your app in browser.</b>

### 3. Lets see last 2000 lines of logs for our app.

~~~
cf logs <app_name> --recent
~~~

### 4. Lets See major events happened in our app.

~~~
cf events <app_name>
~~~

<b> NOTE : You can also see all the above things via app manager, try finding that options in app manager.</b>
