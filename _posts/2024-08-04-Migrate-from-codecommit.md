---
layout: post
title: The Lights Have Come On in the AWS CodeCommit Club: Where to Go Now?
---

[Imagine you're at your local bar.](https://www.youtube.com/watch?v=xGytDsqkQY8) It's not the best, but it's convenient, close to your other regular spots, so you still go there despite its mediocrity.

Suddenly, the lights snap on unexpectedly, jarring you into a blinding alert state. The bar's closed? When did that happen? You notice a few people dancing in the corner, oblivious to the signals to get out. You decide it's best to grab your stuff and stumble out onto the street, hurriedly thinking over your options for where to keep the party going.

That's been my experience with AWS CodeCommit. It's not great, it'll do, but the most attractive bit is that it's on the same platform as the other AWS services I use. Then suddenly, [people are saying that it's not allowing access, but you can still get in](https://github.com/SummitRoute/aws_breaking_changes/?utm_source=substack&utm_medium=email). Then you see on a news site that [AWS is now communicating through their VP via X instead of official blogs](https://x.com/jeffbarr/status/1818461689920344321).

![Git_R_Done]({{ site.baseurl }}/images/no_codecommit.png)

So as far as I'm concerned, the writing's on the wall. Let's go somewhere else.

# Some Options

Please do your research, but here are a couple of well-known options out there today.

### GitHub
   - **Overview:** GitHub is a leading software development platform, offering powerful collaboration, code review, and project management features.
   - **Key Features:** Integrated CI/CD with GitHub Actions, extensive marketplace for tools and integrations, robust issue tracking, and an active community for support and collaboration. Free static site hosting (you're reading this on one right now).
   - **Why Choose GitHub:** Its user-friendly interface, strong community support, and extensive integration capabilities make it a top choice for developers and teams of all sizes.
   - **Cons:**
     - Limited free private repositories (especially for larger teams).
     - Can become costly with large numbers of private repositories.
     - Less flexible in terms of hosting options compared to self-hosted solutions.

### GitLab
   - **Overview:** GitLab is a complete DevOps platform that provides source code management, CI/CD, and monitoring all in one place.
   - **Key Features:** Built-in CI/CD, comprehensive security features, powerful collaboration tools, and robust integration with other DevOps tools.
   - **Why Choose GitLab:** Its end-to-end DevOps capabilities, strong security features, and ability to self-host make it an excellent choice for teams looking to streamline their development processes.
   - **Cons:**
     - Self-hosting can be resource-intensive and complex to manage.
     - The user interface can be overwhelming for new users.
     - The free tier offers fewer features compared to GitHub's free tier.

### Bitbucket
   - **Overview:** Bitbucket is a Git repository management solution designed for professional teams, offering a full set of features for code collaboration and CI/CD.
   - **Key Features:** Deep integration with Atlassian products (like Jira and Confluence), built-in CI/CD with Bitbucket Pipelines, and a focus on team collaboration and project management.
   - **Why Choose Bitbucket:** Its seamless integration with the Atlassian ecosystem and strong project management tools make it ideal for teams already using other Atlassian products.
   - **Cons:**
     - CI/CD features are not as robust as dedicated solutions like GitLab CI or GitHub Actions.
     - Interface can be less intuitive compared to competitors.
     - Limited free tier for private repositories, potentially leading to higher costs for larger teams.

# How to Get Up and Go

### Migration Prep Items to Consider

1. **Access to source repository:** Make sure you've got the right logins/permission to access the AWS CodeCommit repository you want to migrate.
2. **Where are you going?:** Make sure you've set up your new repository. For this example, I'm using GitHub, but thanks to git's interface, the method doesn't change as long as your destination is using git.
3. **Installed software:**
    - **Git:** If it's not already on your machine, [it's not that difficult](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
    - **AWS CLI:** Less likely to be pre-installed, but [Bezos has you covered here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

### This is Our Setup

* **Current repo:** `https://git-codecommit.us-east-1.amazonaws.com/v1/repos/my-code-commit-repo`
* **To be migrating to:** `https://github.com/test-user/my-github-repo.git`
* **GitHub user:** test-user

# Let's Do This!

![Git_R_Done]({{ site.baseurl }}/images/git_r_done.jpg)

<center>See what I did there?</center>

### Quick Rundown

1. Clone the CodeCommit repo.
1. Tell the local copy about your shiny new provider.
1. Send the repo onward to its new home.
1. Check the new repo location to make sure it all arrived.
1. Update your existing CI/CD automation to the new location
1. Clean up the old CodeCommit references from git.

### Actually Useful Steps

1. **Clone the CodeCommit Repository**

    Clone your existing CodeCommit repository to your local machine. Use the following command:

    ```sh
    git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/my-code-commit-repo
    cd my-code-commit-repo
    ```

2. **Add a Remote for the New Repository**

    Add a remote to your new repository. This remote will point to the repository you just created.

    ```sh
    git remote add github-origin https://github.com/test-user/my-github-repo.git
    ```

3. **Push to the New Repository**

    Push your local repository's contents to the new GitHub remote. This will migrate all branches and tags.

    ```sh
    # Push all branches
    git push github-origin --all

    # Push all tags
    git push github-origin --tags
    ```

4. **Verify the Migration**

    It's crucial to verify that all data has been correctly migrated. Check your new repository to ensure all branches, commits, and tags are present and correct. This is likely easiest via its web browser console.

5. **Update CI/CD Pipelines and Webhooks**

    If you have any CI/CD pipelines or webhooks set up with your CodeCommit repository, update them to point to your new GitHub repository. This ensures that your automated processes continue to function seamlessly.

6. **Clean Up**

    After pushing your repository, you might want to update your local Git configuration to point to the GitHub remote by default. This can be done by renaming the remote:

    ```sh
    git remote rename origin codecommit-origin
    git remote rename github-origin origin
    ```

    After verifying the migration and updating your configurations, you can remove the old CodeCommit repository if it's no longer needed. This step is optional and should be done with caution.

    ```sh
    # Delete the old remote reference (if desired)
    git remote remove codecommit-origin
    ```

### Additional Tips

* **Backup:** Always take a backup of your repository before starting the migration process.
* **Permissions:** Ensure that permissions and access controls are correctly set up in the new Git provider.
* **Documentation:** Document the migration process and any changes made for future reference.

# All Done?

As I mentioned earlier, AWS hasn't said they're decommissioning CodeCommit, but they are blocking all new repository creation if your account or AWS Organization hasn't already created one. That says plenty to me.

If you've followed these steps and have moved to your new digs, congrats! If you've been solely in CodeCommit, you'll probably spend the next few months realizing the basic features other git repository providers bake in by default.

*P.S. If you didn't click the youtube link at the start, you're missing out on what was rolling around in my head while writing this :D*
