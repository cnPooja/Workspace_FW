This is a project to understand Buildroot in Linux

Below are the steps used to make changes in repo
1. Clone buildroot, specify the tag version in readme and commit to your own remote project repo in github or 
   Create new project folder,
   git init my-new-repo                     //Initializes a new Git repository in the directory my-new-repo
   cd my-new-repo                           //Moves into the newly created repository folder
   echo "# My New Repository" > README.md   //Creates a README file with initial content. Replace this with your desired initial files
   git add .                                //Stages all changes (new files, modifications) for commit
   git commit -m "Initial commit"           //Saves the changes in the repository with a message describing the commit
   git submodule add https://git.buildroot.net/buildroot <path where submodule should reside> // clones the submodule and keeps the directory name as is unless specified and
                                             updates .gitmodules. This gets the latest code from the original remote(buidroot developer eg) to parent repo(this repo)
                                             This is same as staging. You can commit and push the changes to the parents repo(this repo).

   Future collaborator who clones this repo has to submodule init and update in order to update the empty submodule. Commands are as below
    git clone <your-repo-url> 
    git submodule init                        //update the .gitmodule of the collaborator. Only used once if the submodule has not been initialised.
                                                                If the parent repo is updated with latest commits from the original repo, then the collaborator has to just use
                                                                submodule update.
                                                                If he uses submodule fetch then he is fetching from the original repo and is not tracked by parent repo   
   git submodule update                       // clones the contens from the parents repo. 
   **Note** that Submodules are often in a detached HEAD state to lock the parent repository to a specific submodule version. Even if the upstream Buildroot repository updates,
   you won't automatically get those updates until you explicitly fetch them to parent
   
   To fetch latest content to parent(not required unless you want to have latest content)
   git fetch                                       //fetch from original repo
   git checkout origin/main                         // refers to the head in the original repo. 
   git add <submodule-path>
   git commit -m "update submoduel to latest commit" // so that the parent submodule points to the latest head that has been updated now
2. cd buildroot
3. git checkout 2023.08  # Or any specific version you want. With git add submodule, the latest commit was fetched.
   You might get head detached message meaning you are working on particular commit and not a branch. Detached head occurs when you are working on a commit and not a branch
    As long as you don’t want to commit to original codebase of build root this is okay. If not checkout the branch and start developing. 
    If you just want to work on build root for your own repo, create a branch and push the changes to your remote repo.
4. Note that as build root is a submodule reference, it is checked in as part of gitmodules file and the new files or modified files are checked in.
   Procedure as below
    git checkout -b my-buildroot-branch
    Make changes
    git add . 
    git commit -m "Custom changes to Buildroot"
    Git remote -v
    Git remote remove origin
    Git remote add origin url
    git remote add origin <your-repo-url> 
    Git remote -v
    Git add . // make changes and stage
    Git status // check staged files
    git commit -m "Updated submodule Buildroot to latest custom changes”
    git push -u origin my-buildroot-branch // push branch to remote



Menuconfig
To make changes in menuconfig, stay in build root folder and make changes. Changes are saved in .config file
To include in commit or to preserve configuration, git add path/to/buildroot/.config
Your controller config will be present inside configs inside build root folder. This config for no of boards and chips help you build a linux image. 
If your controller is not present, check online for your board configuration. 
Make configfilename // builds the config and creates .config file in build root. Defconfig is default config. Run this in build root directory. Using Raspberry pi as STM nucleo has only controller and not processor to run Linux. 
Though bluez is present inside emnuconfig, you need to use menuconfig to include it
Make menuconfig // to see the .config file in pictorial form and change the configuration from default. Bluez present under Target packages -> Network Applications -> BlueZ
Make // build linux image
