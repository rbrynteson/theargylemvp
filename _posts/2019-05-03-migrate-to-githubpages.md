---
title: Migrate from WordPress to GitHub Pages
date: 2019-05-03T13:34:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/05/03/migrate-to-githubpages/
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/11/code2.png
categories:
  - Development
---

It was time to follow the crowd!  Everyone seemed to be moving away from WordPress to something easier, less costly and simply a lower chance of going wrong.  This week alone, another WordPress site has been down for one reason or another three times.  So it's time to move.

## Options

There are so many options out there.  If you want a good look at another journey check out [Michael](https://realtimeuc.com/2018/05/wordpress-static-site-azure/index.html)'s journey from WordPress to Azure.

In my case, I knew that I wanted to end my journey in GitHub Pages.  Why?  Because it's free to use, includes https support, uses mark down, easy to deploy, etc.  Basically it was an absolute no brainer for me.  So that meant for me, the choice was going to be [Jekyll](https://jekyllrb.com/) for the actual website, etc.  So how did we get from WordPress to here.

## The Migration

I'll be honest, this was way easier than I thought it was going to be.  There are lots of great guides out there that describe how to export and migrate.  So here were the basics for me.

1. Create a GitHub Repo.  I went ahead and went public because why not.
2. Installed Visual Studio Code and GitHub Desktop.  Both are going to be needed.
3. Export your data from WordPress.  I used [Girliemac](https://girliemac.com/blog/2013/12/27/wordpress-to-jekyll/)'s article as an aboslute starting point.
- Exported comments to Disqus
- Installed Jekyll
- Imported WordPress Contents
- Setup Disqus
- Found a theme I liked.

The theme was BY FAR the process that took the longest to figure out.  It took a bit but I found [Yummy Jekyll](https://github.com/DONGChuan/Yummy-Jekyll) and decided it was for me.  Now I spent some time customizing it and of course you can head over to [GitHub](https://github.com/rbrynteson/theargylemvp) to see everything that is on the site.

Once you have the theme and site it was just a matter of uploading the content to GitHub.  There are a few things I've found along the way that are broken and I know that I'm going to need to update all of the images to the new domain.  They are still pulling from the old site.  So before I can put my forward in that is my next step.

A few other links I used along the way that were helpful for building and testing Jekyll locally.  The cool thing is GitHub does the actual builds so I don't have to upload site files at all.

- [GitHub](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll) - Local Testing
- [GitHub](https://help.github.com/en/articles/page-build-failed-config-file-error) - Page Failures
- [Markdown](https://www.markdownguide.org/basic-syntax/) - Basics on Markdown