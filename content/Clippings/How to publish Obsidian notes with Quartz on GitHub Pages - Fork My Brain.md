---
title: "How to publish Obsidian notes with Quartz on GitHub Pages - Fork My Brain"
source: "https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages"
author:
  - "[[Fork My Brain]]"
published:
created: 2025-03-29
description: "How to publish Obsidian notes with Quartz on GitHub Pages - Fork My Brain"
tags:
  - "clippings"
---
On this page, I describe how I publish [Plain text](https://notes.nicolevanderhoeven.com/Plain+text) notes online using:

- [Obsidian](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/01+First+steps+with+Obsidian/Obsidian) as an interface to create and edit notes in [Markdown](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/02+Making+Notes+in+Obsidian/Markdown)
- [Quartz](https://notes.nicolevanderhoeven.com/Quartz) as a [Static site generator](https://notes.nicolevanderhoeven.com/Static+site+generator) to turn the Markdown into HTML
- [GitHub Pages](https://notes.nicolevanderhoeven.com/GitHub+Pages) as a hosting provider

![How to publish your notes for free with Quartz 2024-01-22 19.17.08.excalidraw.dark.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/plugins/Excalidraw/How%20to%20publish%20your%20notes%20for%20free%20with%20Quartz%202024-01-22%2019.17.08.excalidraw.dark.png)

This setup is free, with the exception of the last, optional, step. I'm also doing all of this on a [Mac](https://notes.nicolevanderhoeven.com/macOS). You may have to tailor terminal commands if you're using a different operating system.

Here's the end result: [Doing It in Public](https://doingitinpublic.com/). Now let's rewind and go through the steps to get there.

Much of this was taken from different pages of the [Quartz docs](https://quartz.jzhao.xyz/), but I'm reposting the steps to get this exact setup and combination of tools here.

Is this page I'm reading hosted on Quartz?

No. This is hosted on Obsidian Publish. I haven't (yet) decided to move this repository over to Quartz. [Here's my Quartz-generated site](https://doingitinpublic.com/).

## Step 0. Prerequisites

You'll need the following installed before you continue:

- [NodeJS](https://notes.nicolevanderhoeven.com/NodeJS) v18.14+ (check your version using `node -v`)
- [NPM](https://notes.nicolevanderhoeven.com/Node+Package+Manager) v9.3.1+ (check your version using `npm -v`)
- [Git](https://notes.nicolevanderhoeven.com/Git) (check your version using `git --version`)
- [Obsidian](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/01+First+steps+with+Obsidian/Obsidian)

## Step 1. Download and install Quartz

### Clone the Quartz repository

Open up your terminal and run this command:

```
git clone https://github.com/jackyzha0/quartz.git my-notes
```

This downloads a copy of the Quartz repository (within a folder called `quartz`) and stores it locally on your computer, in a folder called `my-notes`.

You can change `my-notes` to something more descriptive or appropriate for your use case.

### Install Quartz dependencies

Quartz is a Node project, and it requires other libraries to run.

Change the directory to the new folder:

Now, use NPM to install those dependencies:

### Initialize Quartz

Time to create a new Quartz project! Run:

You are then asked to choose between three different methods of initializing the content in your directory:

```
- Empty Quartz
- Copy an existing folder
- Symlink an existing folder
```

Use the arrow keys to select `Empty Quartz`, and then hit Enter. You should then see something like this:

```
Choose how Quartz should resolve links in your content. You can change this later in \`quartz.config.ts\`.
- Treat links as absolute path
- Treat links as shortest path
- Treat links as relative paths
```

What you select here is dependent on how you prefer to handle links in Obsidian. By default, Obsidian uses the shortest path where possible, so unless you know you'll want to change this behaviour later, use the arrow keys to select the second option, `Treat links as the shortest path`, and then hit Enter.

If you already have an Obsidian vault, and you want to check which one you're using...

In Obsidian, go to Settings > Files and links and look for the option *New link format*:  
![obsidian-new-link-format.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/obsidian-new-link-format.png)  
Choose the Quartz option that matches whatever you've got selected from the dropdown menu.

You should see something like this:

```
You're all set! Not sure what to try next? Try:
- Customizing Quartz a bit more by editing \`quartz.config.ts\`
- Running \`npx quartz build --serve\` to preview your Quartz locally
- Hosting your Quartz online (see: https://quartz.jzhao.xyz/hosting)
```

## Step 2. Set up a GitHub repository

### Create a GitHub repository

When logged into GitHub, click on the *New* button to create a new repository.

In *Repository name:*, type `my-notes` (or whatever you chose in [Step 1](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages#Step%201.%20Download%20and%20install%20Quartz)).

For *Initialize this repository with:* Make sure that *Add a README file* is NOT ticked.

Click *Create repository* at the end.

### Change the `origin` remote

A remote is a name for a repository hosted on GitHub that you can refer to later. There are already two remotes that are set up, but you need to change one of them.

On GitHub, after creating your repository, you should see a screen like this:

![github-quick-setup.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/github-quick-setup.png)

In the screenshot, I chose `SSH`. [SSH](https://notes.nicolevanderhoeven.com/SSH) is more secure, but it has its disadvantages. Unless you have a preference, I recommend you choose [HTTPS](https://notes.nicolevanderhoeven.com/HTTPS).

Click on the Copy button to copy what's in the text field.

Then, on your terminal, run:

You should see something like:

```
origin  https://github.com/jackyzha0/quartz.git (fetch)
origin  https://github.com/jackyzha0/quartz.git (push)
upstream        https://github.com/jackyzha0/quartz.git (fetch)
upstream        https://github.com/jackyzha0/quartz.git (push)
```

which shows two remotes, `origin` and `upstream`, each of which has a `fetch` and a `push`.

Now run:

This command removes the `origin` remote.

Now add the remote back, but this time with the correct repository:

```
git remote add origin https://github.com/yourusername/my-notes.git
```

where `yourusername` is the username you selected for GitHub and `my-notes` is the name of the vault you chose in [Step 1](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages#Step%201.%20Download%20and%20install%20Quartz).

Check the remotes again:

You should see something like:

```
origin  https://github.com/yourusername/my-notes.git (fetch)
origin  https://github.com/yourusername/my-notes.git (push)
upstream        https://github.com/jackyzha0/quartz.git (fetch)
upstream        https://github.com/jackyzha0/quartz.git (push)
```

Note the changes in the first two lines.

### Sync your changes

In this section, you are pushing the changes you made to your local repository (`my-notes` on your computer) to your remote repository (`my-notes` on GitHub).

Run:

```
npx quartz sync --no-pull
```

You should get some text back with a nice green `Done!` at the end.

To verify that it's worked, go back to GitHub and refresh that page you were on where you previously copied that text string containing your repository's address. You should see something like this there instead:

![github-quartz-sync.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/github-quartz-sync.png)

Your repositories are synced! From now on, every time you run that last command, any changes you've made locally will be sent to your GitHub repository.

## Step 3. Create an Obsidian vault

### Opening your Quartz repository in Obsidian

Time to start actually creating content! You're going to do that in Obsidian. So fire up Obsidian, and when asked which vault to open, click on *Open* next to *Open folder as vault.*

A Finder dialog window pops up. Navigate to the folder you created (`my-notes`) and then click *Open*.

### (Optional) Setting up Obsidian

At this point, you can spend some time to set up Obsidian the way you like-- or you can skip this step entirely. To give you an idea, here are the things I did:

- I copied over the `.obsidian/hotkeys.json` from my main vault to my new vault, so that all my keyboard shortcuts will work in the new one too.
- Selected a theme.
- Installed and set up plugins I knew I'd want in the new vault.

### Create a note template in Obsidian

Quartz needs certain [properties](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Core+Plugins/Properties+in+Obsidian) in the [YAML Frontmatter](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/03+Linking+and+organizing/YAML+Frontmatter) to work. You *could* just type these out each time, but I highly suggest using a template for this. You can use either the [core plugin](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Core+Plugins/Core+plugins) [Templates](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Core+Plugins/Templates), or you could use the community plugin [Templater](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Community+Plugins/Templater+plugin). I chose Templater, so install and enable that before continuing.

In Obsidian, create a note in `templates` called `note` (or whatever you want it to be called). In that note, copy this:

```
---
title: "How to publish Obsidian notes with Quartz on GitHub Pages"
draft: false
tags:
  - 
---
 
```

Go to Settings > Templater. In *Template folder location*, type `templates`.

### Create content

In Obsidian, create a new note or two with your new template.

## Step 4. Link your local files to GitHub

### Build your site locally

Before you publish your site online, you should verify that it all works on your computer. This requires "building" the site by running a Quartz server that will convert the Markdown to HTML so that you can see it on your web browser.

Then, in your terminal, run this command:

You should see a block of text that includes this line:

```
Started a Quartz server listening at http://localhost:8080
```

Open up your web browser and type in `http://localhost:8080`.

You should see your site rendered in your browser:

![quartz-local-build.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/quartz-local-build.png)

The page shown may look different if you changed its contents, but the important thing is that you see your changes reflected in your browser.

### Sync to GitHub

Now that you've verified that your site renders correctly locally, it's time to sync your changes to GitHub.

Run this command:

This command sends your changes to your online GitHub repository.

## Step 5. Host your vault online

At this point, you are able to view a site (the rendered HTML version of your Markdown notes) locally, but when you sync those changes to GitHub, they're still just in the repository. In this step, you'll set your repository up so that those Markdown files get generated and published online.

### Create a `deploy.yml` file

From your terminal, run:

```
touch .github/workflows/deploy.yml
```

Open up that file that you just created in Finder.

Make sure hidden files are visible

`.github` is a hidden file, so if you don't see it, hit `CMD + OPT + .` so that you do see it.

The blank file should have opened in your default text editor. Now copy and paste this into it:

```
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v3
        with:
          node-version: 18.14
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

(*Note: This was updated on 2024-01-25. If you're reading this much later than that, get the latest version of this from [the Quartz documentation](https://quartz.jzhao.xyz/hosting#github-pages).*)

Save the file.

Why can't you see this file in Obsidian?

By default, Obsidian only shows you some types of files and not others. Don't worry! That's why you had to use a different text editor to create this file. You won't need to modify this file again in the future.

### Create a GitHub Action

In GitHub, go to Settings > Pages.

Under *Source*, select *GitHub Actions*.

Then, go back to your terminal and sync. Remember, that's:

### Behold your new site!

Verify that this worked by visiting `https://yourusername.github.io/my-notes` from your browser.

You should see your site displayed, just as you saw it when you built it locally.

Wait... is this really free?

Yes. Yes it is. *However*, there are some restrictions. Hosting via GitHub pages isn't really suited for an ecommerce platform where people are buying your products, for example. Since you're publishing your notes, that shouldn't be a problem. But [check out the usage restrictions on GitHub Pages here](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#usage-limits).  
If you decide GitHub Pages isn't enough, you can also use [Cloudflare Pages](https://quartz.jzhao.xyz/hosting#cloudflare-pages), [Vercel](https://quartz.jzhao.xyz/hosting#vercel), or [Netlify](https://quartz.jzhao.xyz/hosting#netlify) instead.

You can stop here if you're happy with the new URL above. But what if you already have a nice shiny new domain and you want your site to be hosted there instead?

## (Optional) Use a custom domain for your new site

This is the only part that is not free. You'll need to pay for your domain. You can buy one at a domain registrar. I like [Porkbun](https://notes.nicolevanderhoeven.com/Porkbun).

### Create A records for your domain

On your domain registrar, navigate to the DNS records section for your domain. On Porkbun, [go to the Domain Management page](https://porkbun.com/account/domainsSpeedy), hover over your domain, and then click the *DNS* link that appears below it.

Select *A - Address record* under *Type*.

In *Answer*, paste this:

Here's what that looks like in Porkbun:

![quartz-porkbun-add-a-records.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/quartz-porkbun-add-a-records.png)

Click Add.

Do this three more times, adding these three more A records for your domain, one by one:

```
185.199.109.153
185.199.110.153
185.199.111.153
```

This step makes it so that when someone goes to your domain, they're served up content from the GitHub servers (at those IP addresses).

### Set up the custom domain on GitHub Pages

Go back to GitHub and navigate to your repository.

Go to Settings > Pages.

In *Custom domain*, enter your domain name, like `supercooldomain.com`. Click *Save*.

### Wait

Actually, don't wait. Go for a walk, make a hot beverage of your choice, sleep. Sometimes, DNS changes take ages to propagate.

You can check whether the changes have propagated by going to `supercooldomain.com`. You should see your Quartz site there.

Go back to Settings > Pages on GitHub and under the *Custom domain* section, tick the option *Enforce HTTPS.* Now you should be able to go to `https://supercooldomain.com`, too!

## References

- [Quartz documentation](https://quartz.jzhao.xyz/)
- [GitHub documentation](https://docs.github.com/)
- [GitHub Pages documentation](https://docs.github.com/en/pages)
- [Obsidian documentation](https://help.obsidian.md/Home)