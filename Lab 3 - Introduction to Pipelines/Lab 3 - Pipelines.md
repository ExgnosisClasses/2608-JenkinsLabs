# Lab 2 - SCM and Build Tools

The objective of this lab work git repositories and to use the Maven plug that integrates Maven with Jenkins.

Check to see that Jenkins is running and that you are logged in before starting the project

## Part 1: Create and Run the Project

Go to the main screen and select the `New Item` option. At the project definition screen, select the `Maven Project' option. If you don't see it, then the Maven plugin needs to be installed. THe instructor can show you how to do this.

<img src="images/1.maven.png" />

You also need to have the Maven Tool Definition entered in the Jenkins tool section. This should be done already in the Tools section of Jenkins as shown below

<img src="images/2.Maven.png" />

If this is not defined, the instructor can guide you on how to define it.

### SCM

Open the configuration screen for the project. The first step is to provide Jenkins with the location of the remote repository.

The repository we will be using is `https://github.com/ExgnosisClasses/MavenReferenceProject.git`

Under the source code management section of the project, enter the repository location as shown.

You also need to change the branch name from `master` to `main`. Jenkins defaults to using the older branch naming convention.


<img src="images/3.maven.png" />

### Build Step and Run

Because this is a Maven project, the plugin has added some Maven specific configuration items. You can leave the POM file specification as it is, but under the `Goals and Options` section, we specify the specific actions we want Maven to execute. In this case `clean, validate and package`

<img src="images/4.mvn.png" />

Once this is done, save your configuration and run the project. The first time you run the project, it may take a long time if it needs to retrieve the dependencies from Maven Central.

<img src="images/6.mvenrun.png" />

### Output

Go to the build output. First look at the console output to see that the project cloned the repository you specified.

<img src="images/7.maven.png" />

Scroll down and confirm that Maven executed

<img src="images/8.build.png" />


### Artifacts

The output of a Maven build is a jar file. If you go the project page, not the build page there is an option to see the artifacts from the last build by opening the `Workspace` which shows the output of the build. This includes the built jar file.

<img src="images/9.artif.png" />

The project also has unit tests that are automatically run because we specified `package` as a build option. The raw output from the test is in the `surefire-reports` folder in the workspace.

These can also be seen in the `tests` option in the project screen.

<img src="images/10test.png" />

## Part 2: Polling the SCM

in the project configuration, go to the Build Triggers section select Build
Periodically. Enter the cron string */2 * * * * which tells Jenkins to run a build every two
minutes.

<img src="images/11.poll1.png" />

Wait a few minutes to confirm the build is triggered.  Once you have done this, go back and unselect the `Build Periodically` option.

### More Tests

If the main project window, you can see a cummulative report on how many of the tests have passed by build.

<img src="images/12test.png" />

## Part 2: Using a Local SCM

For this section, clone the remote repository to your lab machine. In these screenshots the repository was cloned int `c:\repos`.


<img src="images/13repos.png" />

Alter the trigger sections of the project configuration to use this local repository instead of the remote repository.


<img src="images/14config.png" />


Save the changes and run the repository. The build should succeed.


## Part 3: Polling the SCM

### Setting up the Trigger

For this section, go back to the triggers section and select the `Poll SCM` option and enter the same cron spec you entered earlier. This will poll your local repository every two minutes, and only if it has detected a new commit, then it will run a new build.

<img src="images/24Poll.png" />

You can wait a few minutes to confirm the builds are not being done. You can also check the `git polling log` on the project page.

<img src="images/15log.png" />

### New Commit

Open the project in Visual Studio Code and make one change to the `AppTest.java` file.  Change the file from this

<img src="images/16java1.png" />

To this

<img src="images/16java2.png" />

*Note: If you don't want to use VSCode, then just open the file in Notepad and make the one line change there.*

Save the chances and commit the changes to your local repository.

<img src="images/17%20Commit.png" />

Wait for a while, and then you will see the build start and finish in an `unstable` state.

Unstable means the build finished successfully but the unit test failed. This is marked with an orange exclamation mark. Also notice the display test results show the last build failed the unit tests.

<img src="images/18Result.png" />

#### Optional

Go back and correct the error so that the next polled build is successful and stable.

## Clean up

Go back into the configuration and uncheck the `Poll SCM` option in `Triggers`


## End Lab

