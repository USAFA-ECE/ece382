# ✏️ HW 1 Setting up CCS and Git

## 📜 Agenda
- Create a Bitbucket repository.
- Configure git repository.
- Install Code Composer Studio.
- Setup Code Composer Studio (CSS) integrated development environment (IDE).
- Import CCS project files.

:::{note}
Don’t worry if it doesn’t work right. If everything did, you’d be out of a job.
:::

## 💻 Procedure

### Create a Bitbucket Repository

- Create a <a href="https://bitbucket.org/" target="_blank">Bitbucket</a> account if you do not already have one.
- Create a new repository in Bitbucket as shown below.
- Name it ECE382\_LastName\_FirstName.
- Give your own instructor and the course director (Dr. Baek) read access: Click `Invite` on the top of the right-hand navigation and then click `Add members`. Provide them **read access**. The instructors' email addresses are as follows:

    - Dr. Baek: stanley.baek@afacademy.af.edu
    - Dr. York: george.york@usafa.edu
    - Maj Mike Seery: 
    - Capt Brian Yarbrough: 
    
    
```{image} ./figures/HW1_BitBucketConfig.gif
:width: 680
:align: center
```

```{Note}
Your repository name must be ECE382\_LastName\_FirstName. Otherwise, instructors may not be able to find your repository.
```

- You may need to create a Bitbucket `app password` as shown below.
- Click `Your Profile` on the top of the right-hand navigation and then click `Personal Settings`. 
- Click `App Passwords` and then click `Create app password`. 
- Write your preferred label and select permissions as shown below.
- Click `Create`.
- Save the password as you cannot view it after you create it.

```{image} ./figures/HW1_BitBucketAppPassword.gif
:width: 720
:align: center
```

### Install Git

- Go to https://git-scm.com/download/win to download `64-bit Git for Windows setup`
- Install Git with the default settings. Git is already installed on Mac. 
- Create a folder named `workspace` under your home folder, e.g., C:\Users\stanley.baek\workspace. 
- Right-click the `workspace` folder and select `Git Bash Here` as shown below.   
- From your repository in Bitbucket, click Clone and copy the command that begins with _git clone_ by clicking on the copy button as shown below.  
- Paste it within the Bash terminal (middle-click, right-click > Paste, or `Shift+Ins` to paste) and add a space followed by a _period_ as shown below. The _period_ at the end means that the destination is the _current folder_. Hit Enter.
- If it asks for a password, enter the app password you saved in the previous step.
- Notice that you have `(master)` at the end of the folder name. If you type `ls`, it should return nothing.

```{image} ./figures/HW1_GitClone.gif
:width: 720
:align: center
```
<br>

- Go to Teams > General > Files > Class Materials. Download the `workspace_XXXX` file and unzip everything in `workspace_XXXX` into the `workspace` folder in your home directory. Do not copy the `workspace_XXXX` folder itself into the `workspace` folder.  The figure below shows an example of a local `workspace` folder on your computer.


```{image} ./figures/HW1_Workspace.png
:width: 640
:align: center
```

<br>

- Go back to Git Bash. If you have already closed it, right click on an empty space inside the `workspace` folder and select `Git Bash Here`.
- Type `git add -A` or `git add -all` and hit `Enter`.
- Type `git commit -m "Initial commit."` and hit `Enter`.
- Type `git push` as shown below
- Enter your username and password if prompted.

```{image} ./figures/HW1_GitPush.gif
:width: 720
:align: center
```

<br>

- In the future you will repeat these three steps when committing your changes:
    - git add -A
    - git commit -m "Comment"
    - git push

- Refresh your Bitbucket repository to observe the new files as shown below

```{image} ./figures/HW1_BitbucketPushed.PNG
:width: 720
:align: center
```
<br>

```{Attention}
After you push your assignments to Git, it is your responsibility to check your code has been successfully pushed to Bitbucket.
```

```{tip}
CCS comes with built-in GIT, and it can be opened from CCS menu > View > Other > Git > Git Staging. You can commit and push at the same time. There are also many third-party graphic user interface (GUI) clients. Check out https://git-scm.com/downloads/guis.
```


- Go to the `workspace` folder on your computer. Then, go to the `inc` folder. 
- Do you have files with the same names? If you have two files with the same name, one of them is a c source file (`*.c`) and the other one is a header file (`*.h`). Windows hides file extensions unless you change its default setting.
- If your files display the file extensions such as `.c` and `.h`, skip the following instruction and go to `Install Code Composer Studio (CCS)`.
- Click `Options` and select `View` as shown the anmination below.
- Uncheck `Hide extensions for known file types`. You can also change some of the Windows default settings shown in the animation.
- Click `OK`.  Now you can see file extensions.

```{image} ./figures/HW1_ShowFileExtensions.gif
:width: 720
:align: center
```
<br> 

### Install Code Composer Studio (CCS).

- Go to https://www.ti.com/tool/download/CCSTUDIO/12.3.0 to download `Windows on-demand installer for CCS IDE`. For Mac users, download `macOS on-demand installer for CCS IDE`.  Do not download version 13 since it was recently released and there will be multiple patches next few months.   
- Run the installer by double-clicking the `ccs_setup_12.X.X.XXXXX.exe` executable.
- Begin the installation process, by default it will ask you to install under a `ti` folder, which is recommended.
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

- Open CCS.
- When asked to `Select a directory as workspace`, select `Browse` and browse to your `workspace` folder. Select the `Use this as the default and do not ask again` check box. Click `Launch`   
- Import all the projects into CSS. **File > Import...**  Choose **Code Composer Studio > CCS Projects** and click `Next`.
- Select `Search Directory` and click the `Browse...` option. Browse to the `workspace` folder.
- CCS should discover many projects inside the `workspace`.  Click `Select All` (**DO NOT** check `Automatically import...` or `Copy projects...` options). This will have CCS reference the project from the original location and preserve the original directory structure required to build. Click `Finish`.
- In Project Explorer, expand `HW01_Git` and double click test.txt to open. Edit the file and save.
- Commit and push to Bitbucket. Enter your password if prompted.
- Refresh your Bitbucket repository to observe the changes. 


```{image} ./figures/HW1_CCS_Config.gif
:scale: 50%
:align: center
```

<br>

```{Attention} 
It is your responsibility to check your files have been successfully pushed to your Bitbucket repository. Always visit your Bitbucket repository after you push your assignments to the repository.
```

### Convert tabs to spaces

- Open CCS if it is closed.
- Open Text Editors Settings.  **Window > Preferences > General > Editors > Text Editors**.
- Check `Insert spaces for tabs` and `Remove multiple spaces on backspace/delete` as shown below.
- Click `Apply and Close`.

```{image} ./figures/HW1_Tabs2Spaces.png
:width: 560
:align: center
```



### Hardware Diagnostics Tool

- Click 🙋 FAQ on the left sidebar of this website. If the sidebar is hidden, click the hamburger button (the triple bar ☰). 
- Go to _How to run the hardware diagnostics tool for hardware issues on my robot?_ section and complete the hardware diagnotics.
- While you are running the tool, take a picture of your robot. Your picture must show one of the tests on the LCD as shown below. 
- Submit the picture in Gradescope.

```{image} ./figures/HW1_HardwareDiagnostics.jpg
:width: 640
:align: center
```

## 🚚 Deliverables

Take screenshots of the following and submit them via Gradescope.  Use `Snip & Sketch` (Win+Shift+S) on Windows 10/11 or Shift+CMD+4 on MacOS to take a screenshot. Save it in `png` or `jpg`.  

```{Warning}
Do NOT take a picture of computer screen with your phone because (i) it will introduce sampling aliasing (more details in ECE215/ECE315) (ii) it will take more steps than screen capture if you want use it on your computer, and (iii) it will always be more blurry than screen capture. 
```

- **[5 Points]** Provide a screenshot of your Bitbucket repository as shown below

```{image} ./figures/HW1_Deliverable1.png
:width: 740
:align: center
```

<br>

- **[6 Points]** Go to Windows Settings > Apps.  Click on Code Composer Studio and take a screenshot as shown below.  For Mac users, it is in the Applications folder.

```{image} ./figures/HW1_Deliverable2.png
:width: 480
:align: center
```

<br>

- **[6 Points]** While you are running the hardware diagnostics tool, take a picture of your robot and submit it in Gradescope.

<br>

- **[-10 Points]** Take a picture of your screen with a mobile device or digital camera and submit it in Gradescope. Yes, I am serious...

```{Warning}
You will receive a grade of -10 everytime you submit a picture of a computer screen taken by your phone or mobile device. 
```





