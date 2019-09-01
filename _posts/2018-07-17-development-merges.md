---
id: 1561
title: Development Merges
date: 2018-07-17T10:45:20+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1561
permalink: /2018/07/17/development-merges/
colormag_page_layout:
  - default_layout
nkweb_code_in_head:
  - default
nkweb_Use_Custom_js:
  - default
nkweb_Custom_js:
  - ""
nkweb_Use_Custom_Values:
  - default
nkweb_Custom_Values:
  - ""
nkweb_Use_Custom:
  - 'false'
nkweb_Custom_Code:
  - ""
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/07/merge-ahead.gif
categories:
  - Development
---
If you have been following along you will know that I&#8217;ve now transitioned all code repositories to BitBucket and using Visual Studio 2017 for code development.  In my last [post](http://masteringlync.com/2018/07/11/git-branches-development/) I detailed my plan to create branches, merge them back to the master and start the process all over again.  Well, it&#8217;s time to do my first merge so what does it all look like.

<img class="alignnone wp-image-1562 size-large" src="https://masteringlync.com/wp-content/uploads/2018/07/1-1.png?resize=800%2C238&#038;ssl=1" alt="" width="800" height="238" /> 

As we can see in our commits view, we can see the two different branches being actively being developed against: PhoneProvisioning and BugFixes04.

**Step #1:** Revisit Visual Studio and ensure there are no changes due to be committed.  To do this, go to Team Explorer | Changes

<img class="alignnone wp-image-1563 size-medium" src="https://masteringlync.com/wp-content/uploads/2018/07/3-1.png?resize=300%2C245&#038;ssl=1" alt="" width="300" height="245" /> 

Here we can see there are no changes to be published.

**Step #2:** So our next step is going to be to take the BugFixes04 branch and merge it into the Master.  To do this, we go to Pull Request | Create New Request.

<img class="alignnone wp-image-1564 size-large" src="https://masteringlync.com/wp-content/uploads/2018/07/2-2.png?resize=800%2C558&#038;ssl=1" alt="" width="800" height="558" /> 

I want to close and merge this branch.  So it&#8217;s critical that you select the &#8220;Close Branch&#8221; option at the bottom.  Now you can click Create Pull Request.

**Step #3:** Review code and merge.  The pull request will show you everything that is being updated in this merge.  You can see two files changes and below this (not included in the screen shot) is the actual code differences.  You can view it inline or side-by-side to see the changes.

<img class="alignnone wp-image-1565 size-large" src="https://masteringlync.com/wp-content/uploads/2018/07/4-2.png?resize=800%2C345&#038;ssl=1" alt="" width="800" height="345" /> 

Once everything looks good and you are happy with the code changes you can choose Merge.  You will then get this option:

<img class="alignnone wp-image-1566 size-large" src="https://masteringlync.com/wp-content/uploads/2018/07/5-1.png?resize=800%2C482&#038;ssl=1" alt="" width="800" height="482" /> 

So there are Merge Strategy options.  You can [read](https://developer.atlassian.com/blog/2014/12/pull-request-merge-strategies-the-great-debate/) all about them and how they impact your project.  I&#8217;m going to choose the default for BitBucket which is git merge &#8211;no-ff (or no fast forward).

If we go back to the Commits view, we can see the first merge is now complete.

<img class="alignnone wp-image-1567 size-large" src="https://masteringlync.com/wp-content/uploads/2018/07/6.png?resize=800%2C298&#038;ssl=1" alt="" width="800" height="298"  /> 

**Step #4:** We are going to repeat Steps 2 and 3 and merge the PhoneProvisioning branch into the master now.  I won&#8217;t take screenshots of this part but trust me that they look exactly the same as before.

If we take one more look at Commits after the final merge we will see everything is in the single master branch now.

&nbsp;

<img class="alignnone wp-image-1569 size-large" src="https://masteringlync.com/wp-content/uploads/2018/07/7-1.png?resize=800%2C327&#038;ssl=1" alt="" width="800" height="327" /> 

**Step #5:** Now we need to go clean up visual studio.  Because its going to think we still have branches left over.  So we head to Team Explorer | Branches.  We find our Master branch, right-click and choose check out.  Then right-click, choose Pull.

<img class="alignnone wp-image-1570 size-medium" src="https://masteringlync.com/wp-content/uploads/2018/07/8.png?resize=300%2C276&#038;ssl=1" alt="" width="300" height="276" /> 

This will pull down everything for the master branch that was merged and updated your local repository.

<img class="alignnone wp-image-1571 size-medium" src="https://masteringlync.com/wp-content/uploads/2018/07/9.png?resize=289%2C300&#038;ssl=1" alt="" width="289" height="300" /> 

Here, we can see that it says fetch and pull were complete.  Now my local copy of Visual Studio matches exactly what is up in BitBucket.  Now it appears you can right-click and choose delete to the BugFix04 and PhoneProvisioning in the local copy (that is the top ones in the images).  The bottom ones are still there but the branches don&#8217;t exist in BitBucket.  There is an option to Delete Branch from Remote but I&#8217;m going to hold off on that for now.

&nbsp;

&nbsp;

&nbsp;

&nbsp;