#PM2 Deployment

- Host: Set up GIT repo for a miniature project
- Host: `pm2 ecosystem # create an ecosystem.json5 file (in the GIT repo)`
- The VB needs permission to clone the repo from Bitbucket
- http://stackoverflow.com/questions/2643502/git-permission-denied-publickey
- VBox: `cd ~/.ssh && ssh-keygen`
- VBox: `cat id_rsa.pub # copy this`
- Go to BitBucket page and add the deployment key
- Host: Go to dir with ecosystem.json5
- VBox: clone git repo via ssh to add bitbucket to known hosts
- Host: `pm2 deploy dev setup # setup deployment stuff`
- Host: `pm2 deploy dev`
- VBox: go to dir specified for deployment path -> >cd source
- VBox: `pm2 index.js # start up node process`

**Prepare env for demo**
- VBox: Make sure it’s “bridged” so it can get GIT files (must be wired unless having an AirPort wifi)
- VBox: Get IP from VBox
- Update ecosystem.json5 with correct IP
- commit and push git changes to the ecosystem file

**DEMO: Deploying a change**
- Open terminal in Host
- Nav to demo repo and open with >code .
- Show ecosystem.json5
 - user, host, ref, repo, path, post-deploy
- Show master branch and release branch
- Open server.js and checkout master/release to show diff
- SSH into VBox
- VBox SSH: navigate to deployment target (~/GIT etc.)
- `pm2 list # nothing running at the moment`
- navigate to dev deployment source
- `pm2 start index.js`
- Host: open browser and take a look at <vbox ip>:3000
- Edit the index.js file and commit + push
- `pm2 deploy dev`
- Host: open browser and take another look at <vbox ip>:3000

**DEMO: Rollback**
- `pm2 deploy dev list # see all deployments, find which one to revert to`
- `pm2 deploy dev reset <nr of commits back>`
