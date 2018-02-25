# Backstage: a server + CLI tool for deploying individual branches

<pre class="terminal">
awesome-app$ git checkout <b>feature-branch-1</b>
awesome-app$ backstage deploy
Your deploy can now be viewed at
http://my-backstage-instance/__backstage/go/awesome-app/<b>feature-branch-1</b>

awesome-app$ git checkout <b>feature-branch-2</b>
awesome-app$ backstage deploy
Your deploy can now be viewed at
http://my-backstage-instance/__backstage/go/awesome-app/<b>feature-branch-2</b>
</pre>

Backstage was made to solve the problem of allowing team members to QA new features of a front-end app before those features are merged into the main branch of the app's repo. With Backstage, you can deploy multiple branches to the same server, and they'll each be available at their own, unique URL.

Interested? Read the [getting started guide](./getting-started.md)!
