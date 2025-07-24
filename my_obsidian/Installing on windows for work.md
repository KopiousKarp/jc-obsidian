As expected, git will need to be installed on the machine

Generate an ssh key with the following 
```
ssh-keygen  -t ed25519 -C "jcristia@udel.edu" #Will prompt for location and password
cat C:\Users\jcristiano/.ssh/id_ed25519.pub #prints key to paste into github
```

Then go to github and add the key to the settings 
`ssh -T git@github.com #Confirm the key works`

You can use git links from github from this point forward

