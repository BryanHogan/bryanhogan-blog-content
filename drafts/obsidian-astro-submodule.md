---
title: Title
description: Description.
emoji: 🖋️
pubDate: 2025-02-13T21:04:11Z
lastUpdate: 2025-02-13T21:04:11Z
tags:
  - astro
  - obsidian
  - blog
---

My blog uses markdown files. I love using Obsidian to write markdown. So how do I (and you) easily write markdown based blog posts for framework such as Astro using Obsidian?

Since my website uses Astro I will mention Astro, but this approach works just as well for other static site generators that use markdown like Hugo, Jekyll, Eleventy, Next.js and more.

The best way is: GitHub & Submodules  
Submodules might sound complicated at first, but it really isn't. It just means that our markdown content is another repository and we add that repository to our main repository.

I had previously tried using Symlinks, but that approach does not feel reliable and is gimmicky.

## How to use Obsidian to write Astro markdown content

A quick overview of all the steps:

1. Create folder with markdown content
2. Make this folder into a repository
3. Open folder with Obsidian and add `Git` plugin
4. Add this repository to the main repository
5. Adjust build step so that the new submodule repository is actually used

That's it!

### Step 1: Content folder

First we want to create the folder that has the markdown content. If you also use Astro this is what would be in the `src/content`/ folder.

If you use images or other assets, which you probably do, you want to create another folder for that.

I have two folders:
- `/blog` which contains all the posts written in markdown.
- `/blog-assets` which contains things like images that I show within my blog posts.

### Step 2: Create repository

Now we want to create a repository for this content.

This means we will have one repository where our all files for the Astro project live, such as all the components, pages, layouts, styles files and everything else you need for your website, and another repository with just the two folders we created earlier.

<details><summary>How to use Git to do that</summary>

Make sure you have Git installed on your device. You can use the visual interface in something like Visual Studio Code which should make this easier for you. But to make it short you will do these steps in your command line interface: `git init` -> `git add *` -> `git commit -m "commit message"` -> `git push`.

</details>

### Step 3: Obsidian and `Git` plugin

Within Obsidian open the file as a new vault that contains the two folders.

Add the community plugin `Git`. Since we have already created and linked the content to a repository the plugin will already know where to pull and push the content to.

### Step 4: Submodule magic

Now we will be adding the smaller repository that only has two folders to the larger repository that has our front-end stuff as a submodule.

When using Astro we want our markdown content within a folder of the content folder. Use `git submodule add small-repository-link src/content` to add the two folders.

Add, commit and push that this new submodule has been added. 

![Screenshot of Visual Studio Code interface when it has a submodule](../blog-assets/images/Obsidian-Astro-Visual-Studio-Code-With-Submodules.png)

### Step 5: Adjusting building steps

Now the step I struggled the most with. This might also depend on how you host your static website, I have tried DigitalOcean so far and will add how I did this with Cloudflare in the future.

Since submodule are not automatically included when we do a pull of this repository we have to specifically state that we do want to include the submodule.

#### DigitalOcean

A simple way to do this would be to just add `--recursive` for the pull command, but I couldn't find a way to do this within DigitalOcean so I had to get a bit more creative.

In DigitalOcean:

1. Add this to the build command: `git submodule update --init --recursive && npm install && npm run build`
2. Use https instead of ssh for the submodule, to do this adjust the `.gitmodules` file which was created when we added the submodule. Use this command within the main repository to change the submodule to https: `git config --file .gitmodules submodule.src/content.url https://github.com/your-username/your-submodule-repo.git` and then `git submodule sync`.

---

That's all! 🎉

I want to hear your feedback!
Was something confusing? Did you get stuck? Did you find this post amazingly helpful? Was it all very mid? Let me know per ==e-mail== or at any of my social media channels like ==Instagram== or ==BlueSky==!