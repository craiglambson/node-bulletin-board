## Vue Events Bulletin Board

My experiences have included a number of challenges which I have either overcome or worked around.
First:  In order to install DOCKER, I had to have a more recent build of Windows10/64.  Since my currrent build of Windows HOME was updated, windows feedback told me
I needed to install Windows 11 Professional.  I was able to acquire that build from a retailer at a much lower price than the $99 figure microsoft wanted to charge.

Next I had to configure an editor for windows/powershell.  I've had experience with several different editors but opted to install notepad++.  I didn't have time to set it up witha logical called Edit, so I  just did a PS start notepad++ and it seemed to work well enough to get the job done.

With the upgraded OS, I was able to install DOCKER.

I followed the amazingly clear instructions written by Evan Porter, using the step-wise tutorial for Docker, I was able to complete it. Docker was completely new to me so I took advantage to read everything I could find so that I felt comfortable using it.  I completed that exercise some time Friday afternoon.  I had kept a running log file of all of my successes, failures, and reworks.  I apologize that it was a bit messy.

I spent Saturday digging through GitHub.  Though I had experience using it, it has been a couple of years.  I knew the concepts but needed to learn the syntax again. My previous experiences were simplified because the development managers had already set up the masters and branches.  This time, I was creating my own and parsed the help files repeatedly until I was confident in what I was doing.

Tonight, Sunday, I wanted to do one more run through of cloning a repository from GitHub to a local destination, make changes, then repost everything to the GitHub master again.
I have now done that with much less anxiety and more confidence in using the new tools from scratch.  I even created a new set of SSH keys for myself, though they weren't necessary for my tasks.

Thank you for the opportunity of using this software again.  I look forward to being able to use it more agressively in the future.

Kindest regards,
Craig

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
below follows my log file:  Sunday, Sept. 20, 2020  11:45pm
----------------------------------------------------------------------------------------------------------------------------------------------------------------------


Authenticating to GitHub Packages
You need an access token to publish, install, and delete packages. You can use a personal access token to authenticate with your username directly to GitHub Packages or the GitHub API. When you create a personal access token, you can assign the token different scopes depending on your needs.
To authenticate using a GitHub Actions workflow:
•	For package registries (PACKAGE-REGISTRY.pkg.github.com/OWNER/REPOSITORY/IMAGE-NAME), you can use a GITHUB_TOKEN.
•	For the container registry (ghcr.io/OWNER/IMAGE-NAME), you must use a personal access token.

Authenticating with a personal access token
You must use a personal access token with the appropriate scopes to publish and install packages in GitHub Packages. For more information, see "About GitHub Packages."
You can authenticate to GitHub Packages with Docker using the docker login command.
To keep your credentials secure, we recommend you save your personal access token in a local file on your computer and use Docker's --password-stdin flag, which reads your token from a local file.
$ cat ~/TOKEN.txt | docker login https://docker.pkg.github.com -u USERNAME --password-stdin
To use this example login command, replace USERNAME with your GitHub username and ~/TOKEN.txt with the file path to your personal access token for GitHub.
For more information, see "Docker login."
To authenticate using a GitHub Actions workflow:
•	For package registries (PACKAGE-REGISTRY.pkg.github.com/OWNER/REPOSITORY/IMAGE-NAME), you can use a GITHUB_TOKEN.
•	For the container registry (ghcr.io/OWNER/IMAGE-NAME), you must use a personal access token.
SSH keys
New SSH key
This is a list of SSH keys associated with your account. Remove any keys that you do not recognize.
•	Delete
SSH
ssh key for githubMD5:fb:38:ba:a5:51:84:45:25:56:df:3a:32:71:ff:eb:04 SHA256:LQSumCq4bKZjLuC9lnK/aVGfHSjThD1XRmnR/CN5/HQAdded on Sep 20, 2020Never used — Read/write
C:\Users\micro\node-bulletin-board>git remote -v
origin  https://github.com/dockersamples/node-bulletin-board (fetch)
origin  https://github.com/dockersamples/node-bulletin-board (push)

Change the pointing to GitHub
$ git remote set-url origin ssh://git@<github-url>/<project>.git


Now create a test.txt file to test that my local repository is connected to my remote repository on github.


C:\Users\micro\node-bulletin-board\bulletin-board-app>start notepad++ test.txt

C:\Users\micro\node-bulletin-board\bulletin-board-app>dir
 Volume in drive C is Windows
 Volume Serial Number is 143D-A4D6

 Directory of C:\Users\micro\node-bulletin-board\bulletin-board-app

09/20/2020  10:32 PM    <DIR>          .
09/20/2020  10:32 PM    <DIR>          ..
09/17/2020  08:32 PM                23 .gitignore
09/17/2020  08:32 PM             1,291 app.js
09/17/2020  08:32 PM    <DIR>          backend
09/18/2020  01:48 PM               137 Dockerfile
09/17/2020  08:32 PM    <DIR>          fonts
09/18/2020  11:34 AM             1,938 index.html
09/17/2020  08:32 PM             1,152 LICENSE
09/19/2020  09:13 PM    <DIR>          node-bulletin-board
09/17/2020  08:32 PM               544 package.json
09/19/2020  08:50 PM             1,009 readme.md
09/18/2020  02:15 PM            18,613 READMEcrl.md
09/17/2020  08:32 PM             1,107 server.js
09/17/2020  08:32 PM             1,277 site.css
09/20/2020  10:33 PM               129 test.txt
              11 File(s)         27,220 bytes
               5 Dir(s)  416,674,074,624 bytes free

C:\Users\micro\node-bulletin-board\bulletin-board-app>git add test.txt

[master (root-commit) ef5c0b4] commit message
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt

C:\Users\micro\node-bulletin-board\bulletin-board-app>

Add the modified file, test.txt to the staging area.
:\Users\micro\node-bulletin-board\bulletin-board-app>git add test.txt
Now we need to commit those changes:
C:\Users\micro\node-bulletin-board\bulletin-board-app>git commit -m "commit message"

These changes have only been recorded in the local repository.  I need to push these changes to the remote repository on the GitHub server.
Lets integrate any changes that have been made to files on the GitHub server by doing a pull and integrating.
Git pull

C:\Users\micro>cd clonedfromgithub

C:\Users\micro\clonedfromgithub>git clone https://github.com/craiglambson/node-bulletin-board
Cloning into 'node-bulletin-board'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (20/20), done.
remote: Total 22 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (22/22), 35.13 KiB | 467.00 KiB/s, done.

Make sure test.txt is now in the clonedfromgithub/node-bulletin-board directory
C:\Users\micro\clonedfromgithub>dir node-bulletin-board
 Volume in drive C is Windows
 Volume Serial Number is 143D-A4D6

 Directory of C:\Users\micro\clonedfromgithub\node-bulletin-board

09/20/2020  10:56 PM    <DIR>          .
09/20/2020  10:56 PM    <DIR>          ..
09/20/2020  10:51 PM                23 .gitignore
09/20/2020  10:51 PM             1,291 app.js
09/20/2020  10:51 PM    <DIR>          backend
09/20/2020  10:51 PM            18,893 CraigLambsonREADME.md.txt
09/20/2020  10:51 PM               137 Dockerfile
09/20/2020  10:51 PM    <DIR>          fonts
09/20/2020  10:51 PM             1,938 index.html
09/20/2020  10:51 PM             1,152 LICENSE
09/20/2020  10:51 PM               544 package.json
09/20/2020  10:51 PM             1,009 readme.md
09/20/2020  10:51 PM             1,107 server.js
09/20/2020  10:51 PM             1,277 site.css
09/20/2020  10:54 PM               155 test.txt
              11 File(s)         27,526 bytes
               4 Dir(s)  416,679,645,184 bytes free

When you run git clone, the following actions occur:
•	A new folder called repo is made
•	It is initialized as a Git repository
•	A remote named origin is created, pointing to the URL you cloned from
•	All of the repository's files and commits are downloaded there
•	The default branch is checked out
  C:\Users\micro\clonedfromgithub\node-bulletin-board>dir
 Volume in drive C is Windows
 Volume Serial Number is 143D-A4D6

 Directory of C:\Users\micro\clonedfromgithub\node-bulletin-board

09/20/2020  10:56 PM    <DIR>          .
09/20/2020  10:56 PM    <DIR>          ..
09/20/2020  10:51 PM                23 .gitignore
09/20/2020  10:51 PM             1,291 app.js
09/20/2020  10:51 PM    <DIR>          backend
09/20/2020  10:51 PM            18,893 CraigLambsonREADME.md.txt
09/20/2020  10:51 PM               137 Dockerfile
09/20/2020  10:51 PM    <DIR>          fonts
09/20/2020  10:51 PM             1,938 index.html
09/20/2020  10:51 PM             1,152 LICENSE
09/20/2020  10:51 PM               544 package.json
09/20/2020  10:51 PM             1,009 readme.md
09/20/2020  10:51 PM             1,107 server.js
09/20/2020  10:51 PM             1,277 site.css
09/20/2020  10:54 PM               155 test.txt
              11 File(s)         27,526 bytes
               4 Dir(s)  416,761,409,536 bytes free

C:\Users\micro\clonedfromgithub\node-bulletin-board>git add test.txt
C:\Users\micro\clonedfromgithub\node-bulletin-board>git commit -m "first commit"
[master 984698b] first commit
 1 file changed, 2 insertions(+)
 create mode 100644 test.txt
set remote repository
C:\Users\micro\clonedfromgithub\node-bulletin-board>git remote -v
origin  https://github.com/craiglambson/node-bulletin-board (fetch)
origin  https://github.com/craiglambson/node-bulletin-board (push)

Push the changes in my local repository to GitHub

C:\Users\micro\clonedfromgithub\node-bulletin-board>git push origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 380 bytes | 190.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/craiglambson/node-bulletin-board
   669b8d2..984698b  master -> master


On Github we see that test.txt is now in the GitHub repository.

 

I think I have completed the exercise.
