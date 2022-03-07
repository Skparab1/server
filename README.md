# server
Using Github as a database and server for my projects. Used for message sender [here](https://skparab1.github.io/server/frontend.html)

## How it works:
- to load content from github, the webpage will just to the json and parse its content
- to write to github, the webpage will use github api and actions to stage a commit and push changes
  - the webpage first uses github api to open an issue with data to be commited in issue body
  - a github actions workflow is triggered by the issue
  - the github actions extracts the body and title data of the issue and then commits the content into the json
- however, the average workflow takes about 20 seconds to complete, and given that the gh api request limit is 5000/hr, this probably can't be used as a live server

## applications
- act as a slow server
- act as a database
- specific uses
  - url shortener (to make redirects)
  - forum [here](https://skparab1.github.io/server/frontend.html)
  - make accounts for something

## limitations
- Slow 
  - Too slow to be used as a live server
- Overwrites 
  - First reads data, then appends and then sends
  - If two people send messages at the same time, or within a minute of each other, or basically if the second message sends before the first one commits, then.....
  - the first one will be deleted and overwritten by the second one RIP

# Acknowledgements
- Original idea (of using Github issues and actions) by @KentoNishi
- Massive thanks to @R2dev2 for advice
