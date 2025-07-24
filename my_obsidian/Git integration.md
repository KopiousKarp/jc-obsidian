### Installation 
Use the [community plugin](obsidian://show-plugin?id=obsidian-git)

initialize a git repository at the base of the vault 


### Settings Configuration 

This should publish to my github: 
I made a blank repository called jc-obsidian. 

added the remote repository with
`git remote add origin git@github.com:KopiousKarp/jc-obsidian.git`

then pushed using 
`git push -u origin master`

Now the git plugin can push to github


### Auto sync 
I used the plugin settings to commit and push 15 minutes after the last file is edited
I also enabled the option to pull on startup of obsidian. 


### Phone app
This might be a little trickier. The git plugin does not support ssh authentication. 
#todo
My Solution is to clone the repo with https from my work laptop onto the phone and then trying to use the app to interact with the repo. 



