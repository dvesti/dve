---
title: How to use blogdown in 2024
author: Zihang Wang
date: '2024-03-01'
slug: []
categories: []
tags: []
subtitle: 'Make your own personal website/blog with RStudio using blogdown'
lastmod: '2024-08-20T17:35:57+03:00'
draft: yes
authors: []
description: ''
series: []
hiddenFromHomePage: no
hiddenFromSearch: no
featuredImage: ''
featuredImagePreview: ''
toc:
  enable: yes
math:
  enable: no
lightgallery: no
license: ''
---


Build your own fancy personal website/blog with RStudio using blogdown in 2024!

<!--more-->

## Overview

Creating a fancy personal website or blog is a dream for many people. However, it is not easy to make it happen. Well, it's at least what I thought one month ago, and thinking it as a tough problem is probably one of the reasons why I'm not a big fan of making website/blog. 

Motivated by a [post](https://www.shilaan.com/post/building-your-website-using-r-blogdown/) from Shilaan Alzahawi, I realized that people can build websiet with RStudio and RMarkdown, which is super cool since I'm using RStudio all the time and quite familiar with it. So I decided to give it a try and it's much simpler than I expect! 

The existing tutorials regarding `Blogdown` are typically written in 2022 or earlier, so that there may be some outdated information. I tested their instructions and summarized into a doable solution. Following these steps, everyone should be able to create a personal website/blog with `blogdown` in 2024!

![The overall workflow](https://zihangwang.org/2024-02-12-how-to-use-blogdown-in-2024/overflow.png)

## Create Github repo

1. Go to your Github account, create a new public repository. Initialize with a README, but don't add `.gitignore`. Unlike creating webpage based on `Github Pages`, we can name this repository whatever we like if you use Netlify in following steps. Choose an appropriate license, e.g. MIT License.

2. Go to the main page, click the blue `Code` button, copy the `HTTPS` link under `Clone` section. We will need to paste this link into RStudio in the next section.

3. Now we have a remote repository on Github! It's used to store the source code and related files of our website. Next, we need to create an R project on our local computer to manage our website.


## Create RStudio project
1. We work on the local copy on our computer by cloning the remote Github repository into a new RStudio project, so that we can sync between remote repository and local desktop.

![create git](https://zihangwang.org/2024-02-12-how-to-use-blogdown-in-2024/rstudio_git.png)

2. Open RStudio, click `File > New Project > Version Control > Git`.

3. Paste the `HTTPS` link in step 1 to `Repository URL`, name this project.

4. Create this project in the folder where you would like to work on.

5. Click `Create Project`.


## Create Site from Templates

1. Install the `blogdown` package: 

```r
install.packages('blogdown')
```

2. We are about to use our first `blogdown` function to create a website with Hugo. Here is a list of [Hogo themes](https://themes.gohugo.io/) which can be chosen to serve as the template of your website. However, not all themes are supported perfectly by `blogdown`. It may be a wise choice to choose the theme that is actively maintained, or is documented to be supported by `blogdown`, see [these examples](https://bookdown.org/yihui/blogdown/other-themes.html). 
![hugo themes](https://zihangwang.org/2024-02-12-how-to-use-blogdown-in-2024/hugo.png)

3. The template I choose to use is [`DoIt`](https://github.com/HEIGE-PCloud/DoIt). Luckily, this template is well-supported by `blogdown`. The command I use to initialize my website is

```r
new_site(theme = "HEIGE-PCloud/DoIt")
```
Note that the theme name is the same as the repository name on Github. If you use a different theme, you should replace `HEIGE-PCloud/DoIt` with the theme name your choose.

![dolt](https://zihangwang.org/2024-02-12-how-to-use-blogdown-in-2024/dolt.png)

4. We can now see some message showing `blogdown` is building the site. Take a moment to read through those messages, and take necessary actions. Importantly, we can use 

```r
blogdown::serve_site()
```
to preview it locally in RStudio `viewer`, and

```r
blogdown::stop_server()
```
to stop a local preview. It's especially convenient when we modify our website in RStudio before publishing in the future.


## Create your own content

1. The website is now created, but it's not personalized yet. We need to add our own content to the website. Take some time to read through the instructions in the `README` file of the theme you choose. It should contain some basic information about how to customize the website. Modify your template in the way you want.

{{< admonition tip >}}
:(far fa-bookmark fa-fw): In most cases, the `config.toml` file or some other `.toml` files in the `config` folder are the most important files to modify. 
{{< /admonition >}}

2. Let's use `Rmarkdown` to create our new blog! That's the main advantage of using `blogdown` to create a website, at least for me using `Rmakrdown` is within my "comfort zone" :) We can use console to initialize a post template:

```r
blogdown::new_post(title="How to use blogdown in 2024")
```
This command will create a new Rmarkdown file in the `content/post` folder. The path of post folders may vary depending on the theme you choose. In some cases, the name is "post" instead.

3. Modify the content of the Rmarkdown file, like authors and date. We can add an option to our `.Rprofile` file so that we don't have to type the same information every time we create a new post. 

```r
blogdown::config_Rprofile() 
```
Then modify the `.Rprofile` file content, for example

```r
options(
  # to automatically serve the site on RStudio startup, set this option to TRUE
  blogdown.serve_site.startup = FALSE,
  # to disable knitting Rmd files on save, set this option to FALSE
  blogdown.knit.on_save = FALSE,
  blogdown.author = "Zihang Wang",
  blogdown.ext = ".Rmarkdown",
  blogdown.subdir = "posts"
)
```
Restart R session to make the changes take effect.

4. After we finish the content, we need to knit the `index.Rmarkdown` file to generate a `index.markdown` file, so that it can be published as a `.html` page. We can use the `Knit` button in RStudio, or use `Cmd+Shift+K` (Mac) or `Ctrl+Shift+K` (Windows/Linux).

5. Let's then config GitHub settings. We need to make a `.gitignore` file:

```r
file.edit(".gitignore")
```
Then add these contents:

```r
.Rproj.user
.Rhistory
.RData
.Ruserdata
.DS_Store
Thumbs.db
```

Use this command to check our `.gitignore` file:

```r
blogdown::check_gitignore()
```
Follow the instructions showing in the console, especially those one starting with `[TODO]`.

Finally, check our content:

```r
blogdown::check_content()
```
Pay attention to the items that are flagged in the printing message, and take necessary actions to fix them. After that, we're free to add/commit/push our changes to the remote repository.

6. The overall workflow every time I want to modify my website is :

- [x] Open RStudio, open my website project.
- [x] Pull the latest changes from the remote repository. I personally use the `Git` pane in RStudio to do this.
- [x] Start Hugo server by `blogdown::serve_site()`, then view site in RStudio `viewer`.
- [x] Edit existing files or add a new post.
- [x] The RStudio viewer pane will automatically update when saving the file. Take a look at the changes to make sure everything looks good.
- [x] Commit and push changes to GitHub (can use the `Git` pane).


## Use Netlify to publish the website
So far, we have a website on our local computer, and we can preview it in RStudio. However, it's not published on the internet yet. We can use Netlify to publish our website (It's free!). 

1. Go to [Netlify](https://www.netlify.com/), sign up using your GitHub account.
2. Click `Add new site`, then choose `Import an existing project`, then `Deploy with GitHub`. Select the repository we created in the first step.
3. Allow necessary permissions for Netlify to access our repository. Complete the process and then we're free to go! 
4. We'll have a website with a URL containing `netlify` domin, like `https://zihangwang.netlify.app`. In the free plan, we can customize the domain name before `netlify.app`. We can use our own domain name if we have one, or buy one from Netlify (I bought `zihangwang.org` from Netlify :).
5. Then back to our local blogdown project, config our Netlify setting:

```r
blogdown::config_netlify()
```
Follow the instructions and take necessary actions. After that, we can check our config:

```r
blogdown::check_config()
```
Importantly, remember to change `baseURL` to the actural URL of our website.


## We made it!
Congratulations! We have created our own website/blog with `blogdown` in 2024! We can now personalize it, share our website with others, and write our own blog posts. It's a great way to share our thoughts and knowledge with others!


## References
- The `blogdown` project github: https://github.com/rstudio/blogdown.
- The official book for `blogdown`: https://bookdown.org/yihui/blogdown/.
- Up & running with blogdown in 2021, by Alison Hill: https://www.apreshill.com/blog/2020-12-new-year-new-blogdown/.
- Building your website using R {blogdown}, by Shilaan Alzahawi: https://www.shilaan.com/post/building-your-website-using-r-blogdown/.



