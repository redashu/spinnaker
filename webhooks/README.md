## adding github auth token to enable webhooks

### first create webhook in github repo 

### Note my webhook URL 

```
http://34.233.5.195:32741/webhooks/git/github/ # url of gate component 
```

### Now create github personal token and store in a file in spinnaker machine 

```
TFile=/home/spinnaker/git_token
```

## execute hal config 

### 1

```
bash-5.0$ hal config features edit  --artifacts true 
+ Get current deployment
  Success
+ Get features
  Success
- No changes supplied.
```

### 2 

```
bash-5.0$ hal config  artifact  github enable
+ Get current deployment
  Success
+ Edit the github provider
  Success
Validation in halconfig:
- WARNING There is a newer version of Halyard available (1.49.0),
  please update when possible
? Run 'sudo apt-get update && sudo apt-get install
  spinnaker-halyard -y' to upgrade

+ Successfully enabled github
```

### 3 

```
bash-5.0$ hal config  artifact  github account  add ashu-git-account --token-file $TFile 
+ Get current deployment
  Success
+ Add the ashu-git-account artifact account
  Success
Validation in halconfig:
- WARNING There is a newer version of Halyard available (1.49.0),
  please update when possible
? Run 'sudo apt-get update && sudo apt-get install
  spinnaker-halyard -y' to upgrade

+ Successfully added artifact account ashu-git-account for artifact
  provider github.

```

### 4 

```
hal deploy apply 
```
