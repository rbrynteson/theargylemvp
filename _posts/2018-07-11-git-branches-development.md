---
id: 1538
title: Git + Branches + Development
date: 2018-07-11T13:33:07+00:00
author: Richard Brynteson
layout: post
guid: http://masteringlync.com/?p=1538
permalink: /2018/07/11/git-branches-development/
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
image: https://masteringlync.com/wp-content/uploads/sites/2/2018/07/1.png
categories:
  - Development
---
I&#8217;m not a developer.  Maybe someday I will feel like I&#8217;ve graduated from person who writes code to a developer but I see what developers do and it blows me away.  So this is my attempt to write out how I&#8217;ve started to use Git with Branches for the purposes of managing bugs and development features.  This might feel like random thoughts and I&#8217;ll be honest, they are to some degree.

**What I&#8217;m Trying To Do**

So what am I trying to do in Visual Studio + Git (well in this case BitBucket)?  I&#8217;m trying to have a place where I can have two parallel tracks of code.  One that is for bug fixes and the other is to allow me to complete larger &#8220;feature&#8221; requests.  As major features are completed, I would want the tracks to merge with each other and then we would start the process again.  I even made a Visio of what I was trying to do.

<img class="alignnone size-full wp-image-1539" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/Branches.png?resize=487%2C693&#038;ssl=1" alt="" width="487" height="693" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/Branches.png?w=487&ssl=1 487w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/Branches.png?resize=211%2C300&ssl=1 211w" sizes="(max-width: 487px) 100vw, 487px" data-recalc-dims="1" /> 

So I&#8217;ve been using BitBucket + Jira for about two years to keep track of projects, features, etc. and I figured now was the time to figure out how to do the above.  If you haven&#8217;t used Git or BitBucket to save your projects you really should.  If you write code and it&#8217;s not in a shared repository you should change and maybe I&#8217;ll blog about that some time.  Moving a project to a Git but I&#8217;m sure there are lots of those out there.

**Solution**

If you have never used Git/BitBucket there is a concept called a branch.  We branch our code to allow us to work in parallel on several items and then at the end we merge everything together.  The reason this is important is I might be working on amazing feature A but it&#8217;s half-way done and suddenly high-impact bug B might be discovered.  If I&#8217;m just working on the code in a single place then I need to either take steps to hide feature A or rush it to completely so I can get the build out.

So we start by creating two branches.  I&#8217;m going to call it BugFix and FeatureName.

<img class="alignnone wp-image-1540" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2.png?resize=600%2C152&#038;ssl=1" alt="" width="600" height="152" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2.png?w=931&ssl=1 931w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2.png?resize=300%2C76&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/2.png?resize=768%2C195&ssl=1 768w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /> 

All a branch does is creates a symbolic link between the master and changes that would go into the branch.  So once I&#8217;ve created my two branches I can go to Visual Studio and do a fetch to retrieve back those branches.  NOTE: This assumes you have already committed to the master and have setup that part.  Please make sure you commit first.

So after our fetch of the master when we look under branches now we see this:

<img class="alignnone size-full wp-image-1542" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/3.png?resize=311%2C227&#038;ssl=1" alt="" width="311" height="227" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/3.png?w=311&ssl=1 311w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/3.png?resize=300%2C219&ssl=1 300w" sizes="(max-width: 311px) 100vw, 311px" data-recalc-dims="1" /> 

From here, I can right-click on each branch and choose to &#8220;Checkout&#8221;.  When I do that, the code of Visual Studio changes from the existing branch to the other.  Essentially, I can now flip between the two worlds as I need to.  I can do some work on a bug, flip back to my feature request.  It&#8217;s pretty darn neat.  If we look at the commits we can see two very different paths now.

<img class="alignnone wp-image-1544" src="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/4-1.png?resize=600%2C112&#038;ssl=1" alt="" width="600" height="112" srcset="https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/4-1.png?w=1212&ssl=1 1212w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/4-1.png?resize=300%2C56&ssl=1 300w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/4-1.png?resize=768%2C144&ssl=1 768w, https://i2.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/4-1.png?resize=1024%2C192&ssl=1 1024w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /> 

Once we have completed that sprint of bugs fixes and the feature is complete, we can merge everything back to the master.  So from BitBucket I can choose a new Pull Request to merge BugFix04 to master.  The important thing is I want to close this branch so I can get the master completely cleaned up and ready to branch again.

<img class="alignnone wp-image-1545" src="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/5.png?resize=600%2C398&#038;ssl=1" alt="" width="600" height="398" srcset="https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/5.png?w=856&ssl=1 856w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/5.png?resize=300%2C199&ssl=1 300w, https://i1.wp.com/masteringlync.com/wp-content/uploads/sites/2/2018/07/5.png?resize=768%2C510&ssl=1 768w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /> 

Once I&#8217;ve closed and merge the bug fix branch, I would do the same with my feature request branch.  Once both are merged, we would want to go back and make sure we didn&#8217;t create any new bug via regression.  But if everything looks good in the master I could branch again and repeat the process over.  It&#8217;s pretty neat.

Again, I&#8217;m not a developer and I&#8217;m sure I could do something easier, faster, better in the above.  If any real programmers happen to read this please feel free to share.

&nbsp;

&nbsp;