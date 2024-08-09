# ✏️ HW 1 Setting up CCS and Git

## 📜 Agenda
- Create a Bitbucket repository.
- Configure git repository.
- Install Code Composer Studio.
- Setup Code Composer Studio (CSS) integrated development environment (IDE).
- Import CCS project files.


```{note} 
Don’t worry if it doesn’t work right. If everything did, you’d be out of a job.
```

## 💻 Procedure


```{warning} 
The GIF animations provided on this page are intended to complement the main resources, rather than serve as the main source of information. It's important to thoroughly read the text instructions in order to understand and follow the guidance. If the instructions are unclear, you can refer to the supplementary GIF animations for further clarity. However, solely relying on the animations without reading the accompanying text may make it difficult to accurately follow the instructions.
```


### Create Bitbucket Repository

- Create a <a href="https://bitbucket.org/" target="_blank">Bitbucket</a> account if you don't have one.
- Log in <a href="https://bitbucket.org/" target="_blank">Bitbucket</a>.
- Browse to <a href="https://bitbucket.org/stanbaek2/ece382_wksp/src/master/" target="_blank">https://bitbucket.org/stanbaek2/ece382_wksp/src/master/</a>.
- Click on the three dots ($\cdots$) on the top of the right-hand navigation for more options. 
- Select `Fork this repository`. 
- Enter "ECE382" for `Project`.
- Name this new repository ECE382_LastName_FirstName.
- Ensure the access level is `Private repository`.
- Give your instructor, Capt Yarbrough, and Dr. Baek read access: Click `Invite` and then click `Add members`. Provide them **read access**. The instructors' Bitbucket email addresses are as follows:

    - Dr. Baek: ![baek](https://img.shields.io/badge/stanley.baek@afacademy.af.edu-red)  
    - LtCol Trimble: ![trimble](https://img.shields.io/badge/james.trimble@afacademy.af.edu-yellow)
    - Capt Yarbrough: ![yarbrough](https://img.shields.io/badge/bcynmelk@yahoo.com-blue)


    <!--  
    - Dr. York: ![york](https://img.shields.io/badge/george.york@usafa.edu-green)
    - Capt Yarbrough: ![yarbrough](https://img.shields.io/badge/bcynmelk@yahoo.com-blue)
    -->

<br>

```{image} ./figures/HW1_GitFork.gif
:width: 720
:align: center
```
<br>

```{important}
Please name your repository as ECE382_LastName_FirstName. This will help instructors find your repository easily.
```

- You may need to create a Bitbucket `app password` as follows.
- Click `Your Profile` on the top of the right-hand navigation and then click `Personal Settings`. 
- Click `App Passwords` and then click `Create app password`. 
- Write your preferred label and select permissions as needed.
- Click `Create`.
- Save the password as you cannot view it after you create it.

```{image} ./figures/HW1_BitbucketAppPassword2.gif 
:width: 720
:align: center
```
<br>

### Install Git

- Browse to  <a href="https://git-scm.com/download/win" target="_blank">git-scm.com</a> to download `64-bit Git for Windows setup`
- Install Git with the default settings. Git is already installed on Mac. 
- Create a folder named `workspace` under your home folder, e.g., C:\Users\stanley.baek\workspace. 
- Right-click the `workspace` folder and select `Git Bash Here` as shown below.   
- From your repository in Bitbucket, click Clone and copy the command that begins with _git clone_ by clicking on the copy button as shown below.  
- Paste it within the Bash terminal (middle-click, right-click > Paste, or `Shift+Ins` to paste) and add a space followed by a **_period_** as shown below. The **_period_** at the end means that the destination is the **_current folder_**. Hit Enter.
- If it asks for a password, enter the app password you saved in the previous step.
- Notice that you have `(master)` at the end of the folder name. 

```{image} ./figures/HW1_GitClone.gif
:width: 720
:align: center
```
<br>

- The figure below shows an example of a local `workspace` folder on your computer.

```{image} ./figures/HW1_Workspace.png
:width: 580
:align: center
```

<br>

- Go back to Git Bash. If you have already closed it, right-click on an empty space inside the `workspace` folder and select `Git Bash Here`.
- Type `git remote -v`.  It will return two lines showing that `origin` is your remote repository at bitbucket.org for both fetch and push. 
- Type `git remote add upstream https://stanbaek2@bitbucket.org/stanbaek2/ece382_wksp.git` (or copy & paste) and hit `Enter`.
- Type `git remote -v`.  It will now return four lines showing that `upstream`
is the original instructor's repository that you forked from.

```{image} ./figures/HW1_GitAddUpstream.gif
:width: 640
:align: center
```
<br>

- Whenever there are any updates on the original code, you will be asked to run `git fetch upstream` to update your local files.  
- Your default push and pull repository is `origin`, which is your Bitbucket repository. 

```{image} ./figures/HW1_FetchUpstream.png
:width: 320
:align: center
```
<center>
Image is sourced from <a href="https://stackoverflow.com/questions/9257533/what-is-the-difference-between-origin-and-upstream-on-github/9257901#9257901" target="_blank">Stakeoverflow</a>
</center>

<br>

### Install Code Composer Studio (CCS).

- Download `Code Compose Studio 12.7.x` using one of the following methods
    - Go to https://www.ti.com/tool/download/CCSTUDIO/12.7.1 to download `Windows on-demand installer for CCS IDE`. For Mac users, download `macOS on-demand installer for CCS IDE`.  <!--Do not download version 13 since it was recently released and there will be multiple patches next few months. -->
    - Go to Teams > General > Files > Class Materials > Software and download `ccs_setup_12.x.x.xxxxx.exe`.    
- Run the installer by double-clicking the `ccs_setup_12.x.x.xxxxx.exe` executable.
- Begin the installation process, by default it will ask you to install under the `C:\ti` folder, which is recommended.
- Select `Custom Installation` (Recommended)
- Select the processor support for `SimpleLink MSP432 MCUs`.
- Ensure the default Install debug probe is selected and leave the rest unselected.
- Click Next until installation begins.
- Click Finish and your installation should proceed to completion.

```{image} ./figures/HW1_CCS_Setup.gif
:width: 760
:align: center
```
<br>

### Import Project Files.

```{attention}
The following steps are critical. If you make a mistake, it may take hours to fix the problem later this semester. Please follow the instructions carefully.
```

- Open CCS.
- When asked to `Select a directory as workspace`, select `Browse` and browse to your `workspace` folder. Select the `Use this as the default and do not ask again` check box. Click `Launch`   
- Import all the projects into CSS. **File > Import...**  Choose **Code Composer Studio > CCS Projects** and click `Next`.
- Select `Search Directory` and click the `Browse...` option. Browse to the `workspace` folder.
- CCS should discover many projects inside the `workspace` folder.  Click `Select All` (**DO NOT** check `Automatically import...` or `Copy projects...` options). This will have CCS reference the project from the original location and preserve the original directory structure required to build. Click `Finish`.
- In `Project Explorer`, expand `HW01_Git` and double click `test.txt` to open. Edit the file and save. You can simply add "My name is ..." as shown below.

```{image} ./figures/HW1_CCS_Config.gif
:scale: 50%
:align: center
```
<br>

### Push Your Code.

- Go back to Git Bash. If you have already closed it, right-click on an empty space inside the `workspace` folder and select `Git Bash Here`.
- Type `git add -A` or `git add -all` and hit `Enter`.
- Type `git commit -m "Homework 1"` and hit `Enter`.
- Type `git push` as shown below.
- You can run `git status` to check the current status of your local repository.
- Enter your username and password if prompted.
- Refresh your Bitbucket repository and ensure your push has been made through. 

```{image} ./figures/HW1_GitPush.gif
:width: 540
:align: center
```
<br>

- In the future you will repeat these three steps when committing your changes:
    - git add -A
    - git commit -m "Comment"
    - git push

```{attention} 
It is your responsibility to check your files have been successfully pushed to your Bitbucket repository. Always visit your Bitbucket repository after you push your assignments to the repository.
```

```{tip}
CCS comes with built-in GIT, and it can be opened from CCS menu > View > Other > Git > Git Staging. You can commit and push at the same time. There are also many third-party graphic user interface (GUI) clients. Check out https://git-scm.com/downloads/guis.
```

### Unhide File Extensions

- Go to the `workspace` folder on your computer. Then, go to the `inc` folder. 
- Do you have files with the same names? If you have two files with the same name, one of them is a c source file (`*.c`) and the other is a header file (`*.h`). Windows hides file extensions unless you change its default setting.
- If your Windows File Explorer displays file extensions such as `.c` and `.h`, skip this section.
- Click `Options` and select `View` as shown in the gif anmination below.
- Uncheck `Hide extensions for known file types`. You can also change some of the Windows default settings shown in the animation.
- Click `OK`.  Now you can see file extensions.

```{image} ./figures/HW1_ShowFileExtensions.gif
:width: 720
:align: center
```
<br> 


### Convert Tabs to Spaces

- Open CCS if it is closed.
- Open Text Editors Settings.  **Window > Preferences > General > Editors > Text Editors**.
- Check `Insert spaces for tabs` and `Remove multiple spaces on backspace/delete` as shown below.
- Click `Apply and Close`.

```{image} ./figures/HW1_Tabs2Spaces.png
:width: 560
:align: center
```

<br>

### Hardware Diagnostics Tool

- Click 🙋 FAQ on the left sidebar of this website. If the sidebar is hidden, click the hamburger button (the triple bars ☰). 
- Go to _How to run the hardware diagnostics tool for hardware issues on my robot?_ section and complete the hardware diagnotics.
- While you are running the tool, take a picture of your robot. Your picture must show one of the tests on the LCD as shown below. Ensure you have a small AF symbol on each test screen.
- Submit the picture on Gradescope.

```{image} ./figures/HW1_HardwareDiagnostics.jpg
:width: 640
:align: center
```
<br>

## 🚚 Deliverables

Take screenshots of the following and submit them via Gradescope.  Use `Snip & Sketch` (Win+Shift+S) on Windows 10/11 or Shift+CMD+4 on MacOS to take a screenshot. Save it in `png` or `jpg`.  

```{warning}
Do NOT take pictures of your computer screen using your phone because (i) it can result in sampling aliasing, as explained in ECE215/ECE315, (ii) it will require more steps compared to a simple screen capture, and (iii) the resulting image will always be blurrier than a direct screen capture.
```

- **[5 Points]** Provide a screenshot of your Bitbucket repository as shown below

```{image} ./figures/HW1_Deliverable1.png
:width: 740
:align: center
```

<br>

- **[5 Points]** Go to Windows Settings > Apps.  Click on Code Composer Studio and take a screenshot as shown below.  For Mac users, it is in the Applications folder.

```{image} ./figures/HW1_Deliverable2.png
:width: 480
:align: center
```

<br>

- **[6 Points]** While you are running the hardware diagnostics tool, take a picture of your robot and submit it in Gradescope.

<br>

- **[-10 Points]** Take a picture of your screen with a mobile device or digital camera and submit it on Gradescope. Yes, I am serious...

```{Warning}
You will receive a grade of -10 everytime you submit a picture of computer screen taken by your phone or mobile device. 
```





