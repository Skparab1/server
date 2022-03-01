# server
Using Github as a database and server for my projects. Sample project here: 

## How it works:
- to load content from github, the webpage will just to the json and parse its content
- to write to github, the webpage will use github api and actions to stage a commit and push changes
  - the webpage first uses github api to open an issue with data to be commited in issue body
  - a github actions workflow is triggered by the issue
  - the github actions extracts the body metadata of the issue and then commits the content into the json
- however, the average workflow takes about 20 seconds to complete, and given that the gh api request limit is 5000/hr, this probably can't be used as a live server

- applications
  - act as a server (a relatevely mediocre one)
  - act as a database
  - specific uses
    - url shortener (to make redirects)
    - chat app (but idk)
