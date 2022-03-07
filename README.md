# Server
## About
This is a template for using Github as a database and slow server. It works by using a json file for data storage, and writing back data using GitHub Issues and Actions. It useally processes writes in less than a minute, but can cause overwrite problems if multiple writes are requested at the same time. An example of using this method of data storage is [here](https://skparab1.github.io/server/frontend.html).

## How it works
To load content from GitHub, the webpage will just to the json and parse its content. To write to back to the database, the webpage will use GitHub API for opening an Issue and Actions to stage a Commit and push changes. First, the webpage first uses Github API to open an issue with data to be commited in issue title and body. Then, the issue triggers a Github Actions workflow that extracts the title and body data of the issue and then commits the content into the json. The entire process takes an average of 30 seconds to complete, and usually completes in less than a minute.

## Applications
This was intended to be used as a Database, and can be used as a server too, but with some limitations. Using as a server may invoke overwrite problems. This can specifically be used for a message forum, which has [already been implemented](https://skparab1.github.io/server/frontend.html). Further uses can include short url generator, as well as to make user accounts.

## Limitations
When used, this can present many limitations, which make it less reliable than a traditional server. For one, it's much slower, and because of that it can overwrite data. The way sending works is by first reading old data and then appending new data, and then sending it. If two people send messages at the same time, or within a minute of each other, or basically if the second message sends before the first one commits, then the first message will be overwritten, or effectively deleted by the second one.

## Acknowledgements
- Original idea (of using Github issues and actions) by @KentoNishi
- Massive thanks to @R2dev2 for advice

## Now let me explain how each part works...
### Sending an issue with Github API
- To send an issue, I used octokit. The docs are [here](https://docs.github.com/en/rest/reference/issues#create-an-issue), but the following snippet sends an issue to the repo.
````
(async () => {
  const octokit = new Octokit({ auth: "YOUR TOKEN" }); // ideally you shouldnt hardcode this
  
  octokit.request('GET /repos/:owner/:repo', {        // you dont really need this but its good for debugging and making sure that it finds the repo
    owner: "OWNER OF THE REPO",
    repo: "REPO NAME"
  }).then(response => console.log(response.data))
  
  async function start() {                            // this part actually sends the issue
    return await octokit.request('POST /repos/OWNER OF REPO/REPO NAME/issues', {
        owner: 'REPO OWNER',
        repo: 'REPO NAME',
        title: 'THE ISSUE TITLE CONTENT',
        body: 'THE ISSUE BODY CONTENT',
      })
  };

  start();
})();
````

### Make Github Actions push isssue data
- This snippet takes the issue data and adds it to two different arrays of the json
````
name: Issue data pusher

# Triggers the workflow when issue is opened
on:
  issues:
    types: [opened]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:
  
# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest
    
    steps:
      # checkout the repo so that you can access files
      - uses: actions/checkout@v2
        with:
          submodules: 'recursive'
      # yoink the contents of the issue and modify data.json
      - name: Update name in data.json
        env:
          issue_data: github.event.issue.title
        run: echo "`jq '.data[0].name="${{ github.event.issue.title }}"' data.json`" > data.json
      - name: Update message in data.json
        run: echo "`jq '.data[1].message="${{ github.event.issue.body }}"' data.json`" > data.json
      # close issue but this doesnt work rn
      - name: Close issue
        run: echo "github.event.issue.close"
      # read the file (not really needed but useful for debugging
      - name: read data.json
        run: cat data.json
      # commit changes
      - name: Commit
        uses: EndBug/add-and-commit@v7
        with:
          message: 'updating data'
          add: "data.json"
      # done!
````
- But you will need to have a file named data.json formatted like this
````
{
  "version": "1",
  "data": [
    {
      "name": "THIS IS WHERE ISSUE TITLE DATA GOES"
    },
    {
      "message": "THIS IS WHERE THE ISSUE BODY DATA GOES"
    }
  ],
  "secondArray": [
    {
      "secondId": "2"
    }
  ]
 }
````

### Reading the data
- reading the data is easy - just load it, yoink the content, and then parse it
````
(async () => {
  const data1 = await fetch('./data.json').then(r => r.json());
  oldusernames = data1.data[0];
  oldmessages = data1.data[1];

  oldusernames = JSON.stringify(oldusernames);
  oldmessages = JSON.stringify(oldmessages);
 })();
````

## Closing
Wow you have reached the end! Thanks for reading all the way though, and I hope you think this is cool. Please star it if you want! Don't forget to leave a message at https://skparab1.github.io/server/ui.html !
