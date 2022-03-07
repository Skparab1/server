# server
## About
This is a template for using Github as a database and slow server. It works by using a json file for data storage, and writing back data using GitHub Issues and Actions. It useally processes writes in less than a minute, but can cause overwrite problems if multiple writes are requested at the same time. An example of using this method of data storage is [here](https://skparab1.github.io/server/frontend.html).

## How it works
To load content from GitHub, the webpage will just to the json and parse its content. To write to back to the database, the webpage will use GitHub API for opening an Issue and Actions to stage a Commit and push changes. First, the webpage first uses Github API to open an issue with data to be commited in issue title and body. Then, the issue triggers a Github Actions workflow that extracts the title and body data of the issue and then commits the content into the json. The entire process takes an average of 30 seconds to complete, and usually completes in less than a minute.

## Applications
This was intended to be used as a Database, and can be used as a server too, but with some limitations. Using as a server may invoke overwrite problems. This can specifically be used for a message forum, which has [already been implemented](https://skparab1.github.io/server/frontend.html). Further uses can include short url generator, as well as to make user accounts.

## Limitations
When used, this can present many limitations, which make it less reliable than a traditional server. For one, it's much slower, and because of that it can overwrite data. The way sending works is by first reading old data and then appending new data, and then sending it. If two people send messages at the same time, or within a minute of each other, or basically if the second message sends before the first one commits, then the first message will be overwritten, or effectively deleted by the second one.

# Acknowledgements
- Original idea (of using Github issues and actions) by @KentoNishi
- Massive thanks to @R2dev2 for advice

# Now let me explain how each part works...
