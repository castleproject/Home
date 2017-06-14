# How to submit a fix to any Castle Project

Now that all [Castle projects are hosted on GitHub](http://github.com/castleproject), it's even easier to start contributing fixes to the project.

Let's walk through the steps to make it easy for all of us!

Let's say you want to submit a patch for the [Windsor project](https://github.com/castleproject/Windsor).

**Step 1: Sign up for GitHub** (jump on it)

* [https://github.com/signup/free](https://github.com/signup/free)
* Enter Username, Email Address, Password, (ignore the SSH Public Key field)
* Click "I agree, sign me up!"

**Step 2: Fork the Windsor project repository** (become a committer of your very own copy of Windsor!)

* [http://github.com/castleproject/Windsor](https://github.com/castleproject/Windsor)
* Click "Fork"

**Step 3: Setup your local dev environment** (one time only)

* [First install git](http://help.github.com/git-installation-redirect)
* [Then generate an SSH keypair](http://help.github.com/msysgit-key-setup)
* [Finally set your local git config](http://help.github.com/git-email-settings)
* You are now ready to clone the repository you forked in the previous step

**Step 4: Clone and modify your fork locally**

* Open a command-line and `cd` to your code folder (where you keep all your other projects)
* Type: `git clone YourPrivateGitAddress` (you can find your private git address on your main github page, it should look something like git@github.com:YourGithubUsername/Windsor.git)
* It should now start pulling down your repository. It was most likely named 'Windsor', so it'll create a 'Windsor' folder under your code folder.
* `cd` to the Windsor folder.  Do a 'dir' and you should see the Visual Studio solution file for the Windsor project.
* Open the Windsor solution and do the fixes in Visual Studio! Remember the code has to comply with our [coding standards](coding-standards.md). Also, we strongly encourage that you include test cases.

**Step 5: When you're ready it's time to commit your changes to github**

* Open a command-line and `cd` to the Windsor folder
* Type: `git commit -am "my commit message"`
  * The commit message should include a reference to an issue if applicable
* Type: `git push origin HEAD`

**Step 6: Submit a Pull Request** (tell the Castle team why your change rocks)

* Click "Pull Request"
* Enter a message that will go with your commit to be reviewed by core committers
* Leave the default recipients ticked
* Click "Send Pull Request"

**Step 7: Add a link to your commit to the issue on the [Issue Tracker](https://github.com/castleproject) if applicable** (tell everyone you're on the case)

* Add a comment that includes a hyperlink to the url of your commit from previous step

**Step 8: Eat a cookie** (yum)

* You're done!

Now if you need to change multiple files as part of one commit, the web interface will not be the way to go. In that case you'll want to learn a little more about GitHub and git. You can start here:

* Learn GitHub:
  * [http://help.github.com/](http://help.github.com/)
* Learn Git:
  * [http://progit.org/book/](http://progit.org/book/)
  * [http://gitscm.org/](http://gitscm.org/)
  * [Git Magic](http://www-cs-students.stanford.edu/%7Eblynn/gitmagic/)
