Atomist uses `handlers` to listen to changes in your projects and then to execute a plan of action. The handler will be triggered by a certain type of activity that Atomist can detect, in this example you will notify the team when an issue labeled 'bug' gets fixed.

### Create a Rug Handlers Project

The first thing you need is a project for your code. Although you can add Atomist Rug code to any existing project by simply adding a `.atomist` directory along with a few support files in this case we'll create and publish a new project for your first Atomist Rug development automation.

Ask the Bot to list out the project generator that you need by typing `@atomist generator rug` and select the `NewHandlersProject` generator:

<div class="ss-container">
  <img src="../images/new-rug-handlers-project-button.png" alt="NewHandlersProject generator" class="ss-small">
</div>

Click on the `Create project` button and enter the following information to create your new project:

<div class="ss-container">
  <img src="../images/create-handlers-project-flow.png" alt="Creating the handlers project" class="ss-small">
</div>

Once done you can click on `Generate project` in the project generation summary:

<div class="ss-container">
  <img src="../images/create-handlers-project-summary.png" alt="New handlers project summary" class="ss-small">
</div>

Then `@atomist` will create the new project in your GitHub organization:

<div class="ss-container">
  <img src="../images/new-handlers-project-created.png" alt="New handlers project created message" class="ss-small">
</div>

Also the Atomist Bot will have created a new `#handlers` channel in Slack that's tied to that project that shows some initial setup commits made by the project generator:

<div class="ss-container">
  <img src="../images/handlers-slack-channel-created.png" alt="Handlers slack channel initialised" class="ss-small">
</div>

Clone the new handlers project locally and, because we're using TypeScript, change directory into the `.atomist` directory of the local handlers project and enter `npm install` to get the TypeScript library dependencies installed:

!!! note ""
    To develop your rugs in TypeScript you will need [`node.js` and `npm`](https://nodejs.org) installed locally.

```console
$ npm install
/.../handlers/.atomist
├─┬ @atomist/github@0.2.0
│ └── @atomist/rug@0.10.0
├── @atomist/rug@0.12.0
└─┬ @atomist/travis@0.6.1
  └── @atomist/rug@0.10.0
```

Now it's time to write your new handler.

### Writing Your Handler

The handlers project you just generated contains a number of pre-existing handlers that you can take inspiration from. For our purposes we only want one new handler and the closest example in the handlers project is `.atomist/handlers/IssueHandler.ts`.

The generated handlers project contains a whole host of sample handlers and executors, including all the default handlers that you've seen in action already. You need to keep those handlers in this project as when you publish this Rug archive you will effectively override the default handlers, and we don't want to lose that functionality we've already seen.

In a new `.atomist/handlers/CloseIssueThanks.ts` file enter the following handler code:

```typescript
import {Atomist} from '@atomist/rug/operations/Handler'
import {TreeNode} from '@atomist/rug/tree/PathExpression'
declare var atomist: Atomist

atomist.on<TreeNode, TreeNode>("/Issue()/belongsTo::Repo()/channel::ChatChannel()", m => {
   let issue = m.root() as any

   if (issue.state() != "closed") {
     return
   }

   atomist.messageBuilder()
    .say("Thanks for closing this issue on " +
         issue.belongsTo().name())
    .on(issue.belongsTo().channel().id()).send()
})
```

Now it's time to publish and test your new handler.

!!! note ""
    At the moment you can't test handlers locally and instead you need to publish and test your handler is invoked from it's results in Atomist.

### Publish Your Handler

To make Atomist aware of your new handler you need to publish the handler project. This is often done via continuous integration but for our purposes here you're going to see how it's done manually using the [Rug CLI][rug-cli].

[rug-cli]: /user-guide/interfaces/cli/index.md

#### Install and Configure the Rug CLI

Firstly make sure you've [installed the latest Rug CLI][cli-install]
for your platform.  To publish new Rugs you need to
configure the Rug CLI with the credentials it needs to push
your rugs to Atomists.  To see this problem you can try to run the
command `rug repositories configure` and you should see:

[cli-install]: /user-guide/interfaces/cli/install.md

```console
$ rug repositories configure

No token configured. Please run repositories login before running this command.

Run the following command for usage help:
  rug repositories configure --help
```

To configure the Rug CLI you need to execute the `rug repositories login` command providing your GitHub credentials:

```console
$ rug repositories login


The Rug CLI needs your GitHub login to identify you.

The command will create a GitHub Personal Access Token with scope 'read:org'
which you can revoke any time on https://github.com/settings/tokens.  Your
password will not be displayed or stored. Your sensitive information will not
be sent to Atomist; only to api.github.com.

  → Username : .....
  → Password : .....

  Please provide a MFA code
  → MFA code : .....

Successfully logged in to GitHub and stored token in ~/.atomist/cli.yml

```

!!! note ""
    No passwords are stored, only a unique personal access token with org/read scope. Also if you are using 2 Factor Authentication with GitHub you will be prompted for the `MFA code` as shown above.

Now when you execute `rug repositories configure` you should see the Rug CLI configured with your team's unique repository:

```console
$ rug repositories configure

→ Repositories
  <unique identifier of your team's repository>
    <your unique team's repository url here>
```

You're now all set to publish your handlers project.

#### Publishing

The Rug CLI has the `publish` command to publish a Rug archive from your local copy. Execute the `rug publish` command from inside your handlers project directory:

```console
$ rug publish
Resolving dependencies for yourorg:handlers:0.1.0 ← local completed
Invoking TypeScript Compiler on ts script sources
  Created .atomist/handlers/CloseIssueThanksHandler.js.map
  Created .atomist/handlers/CloseIssueThanksHandler.js
Processing script sources completed
Loading yourorg:handlers:0.1.0 ← local into runtime completed
  Created META-INF/maven/yourorg/handlers/pom.xml
  Created .atomist/manifest.yml
  Created .atomist/metadata.json
Generating archive metadata completed
  Uploading yourorg/handlers/0.1.0/handlers-0.1.0.zip → t3v0s7ss2 (186 kb) succeeded
  Uploading yourorg/handlers/0.1.0/handlers-0.1.0.pom → t3v0s7ss2 (927 bytes) succeeded
  Uploading yourorg/handlers/0.1.0/handlers-0.1.0-metadata.json → t3v0s7ss2 (762 bytes) succeeded
  Downloading yourorg/handlers/maven-metadata.xml ← t3v0s7ss2 (382 bytes) succeeded
  Uploading yourorg/handlers/maven-metadata.xml → t3v0s7ss2 (334 bytes) succeeded
Publishing archive into remote repository completed

→ Archive
  ~/workspace/handlers/.atomist/target/handlers-0.1.0.zip (186 kb in 165 files)

→ URL
  https://atomist.jfrog.io/atomist/T3V0S7SS2/yourorg/handlers/0.1.0/handlers-0.1.0.zip

Successfully published archive for yourorg:handlers:0.1.0
```

Your new handlers rugs are now ready for action in your Atomist environment. To test that everything has published correctly you can execute `rug search` and you should see your Rug archive listed just for you:

```console
$ rug search
Searching https://api.atomist.com/catalog
Searching catalogs completed

→ Remote Archives (38 archives found)
yourorg:handlers [private] (0.1.0)
...
```

### Your Handler in Action

Now you can give your new `CloseIssueThanksHandler` handler a spin! Head back to the `#sprocket` channel and you should see a button to `Close` the issue we created earlier:

<div class="ss-container">
  <img src="../images/close-button-on-issue.png" alt="Issue ready to be closed" class="ss-small">
</div>

Click on `Close` (you could also close the issue in GitHub if you prefer) and several things will happen. Firstly the Atomist Bot will indicate with a message that the issue has been edited...

<div class="ss-container">
  <img src="../images/close-issue-edited-response.png" alt="Issue edited response" class="ss-small">
</div>

... then the panel for the issue in Slack will be updated with a `Reopen` button ...

<div class="ss-container">
  <img src="../images/reopen-button-displayed.png" alt="Reopen issue displayed" class="ss-small">
</div>

... and finally your new handler will be invoked and the "thanks" message will be displayed!

<div class="ss-container">
  <img src="../images/new-handler-message-displayed.png" alt="New handler thank-you message displayed" class="ss-small">
</div>

There, you did it! You just created a new automation, and taught the Bot to listen for events and run that automation!
