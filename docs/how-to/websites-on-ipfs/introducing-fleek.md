---
title: Fleek简介
descripton: Fleek is a service that lets you host a website on IPFS without needing to install anything on your computer or run command-line scripts.
---

# Fleek简介

本系列教程的大部分步骤都需要手动执行，是否有一个服务，能够把这些繁杂的工作都完成了，使得我们可以专注于在IPFS中托管出色的网站？这就是Fleek的用武之地！


![The Fleek homepage, showing a "Build on the New Internet" slogan at the top.](./images/introducing-fleek/fleek-homepage.png)

Fleek作为一个服务，能够让你无需在电脑上安装任何程序，或者执行任何命令行命令，即可把网站托管到IPFS中。

我们已经知道在IPFS中，文件夹和文件都是基于内容寻址的，也就是说通过内容的hash可以访问到对应的内容。如果内容发生了变化，它的hash也相应变化了。正如之前教程显示的，这会成为更新网站时的问题。一个HTML文件即便只是修改了一个字符，也会创建一个全新的hash！

Fleek提供了一个简单的工作流。当你把更新推送到Git时，Fleek能够构建、固定和更新你的网站。此服务也能够与React, Next.js, Gatsby, Jekyll, Hugo和 [许多其他流行的开发框架](https://docs.fleek.co/hosting/site-deployment/#common-frameworks)很好的进行集成。还可以通过Fleek来管理域名，并通过和传统web开发类似的方式来监控网站。

Fleek是一个很好的选择，用于在IPFS中快速托管网站！很多信息可以查看[Fleek.co](https://fleek.co)和[文档](https://docs.fleek.co/).

## 托管站点

If you've never used a service like Fleek, or just need a refresher, this quick guide walks through adding a website to a GitHub repository and linking that repo to your Fleek account.

We're going to re-use the Random Planet Facts site we created in a previous tutorial. If you've been following this tutorial series, you should already have this project ready to go! If you don't, just download the [project `.zip` here](https://github.com/johnnymatthews/random-planet-facts/archive/master.zip) or [clone this repository](https://github.com/johnnymatthews/random-planet-facts).

### Upload to GitHub

If you cloned the Random Planet Facts repo above, you don't need to follow this section.

1. Log into [GitHub](https://github.com).
1. Create a new repository and upload the Random Planet Facts project.
1. Your project repository should look something like this:

   ![A GitHub repository showing an index.html file, a style.css file, and an image file.](./images/introducing-fleek/github-repo-showing-a-few-files.png)

### Add a repository to Fleek

1. Go to [Fleek.co](https://fleek.co/) and sign in using your GitHub account. You may need to allow Fleek to access your GitHub profile.
1. Once logged in, click **Add new site**.
1. Select **Connect with GitHub** and find the site that you want to host on IPFS.
1. Leave all the options with their default settings. Since we're not dealing with a special framework or a repository with lots of branches we don't have to change anything here.

   ![Fleek showing the website repository options page.](./images/introducing-fleek/fleek-showing-the-website-repo-options.png)

1. Click **Deploy site**. Fleek will add your site into the build queue. Once it's done you can click **Verify on IPFS** to view your site!

   ![Deployment information window within Fleek.](./images/introducing-fleek/deployment-information-window.png)

## Domain names

Fleek allows you to configure your domain names with your sites on IPFS! No more wrangling with DNSlink or IPNS. You can even buy domains directly through Fleek. Click **Add or Buy Domain** to get started. [Check out the Fleek documentation for more information on how to get your domain linked up →](https://docs.fleek.co/domain-management/overview/)

![A black button leading to the domain section of Fleek](./images/introducing-fleek/add-or-buy-domain.png)

## Up next

For the final tutorial in this series, we're going to take a quick look at [static-site generators, and how to host them on IPFS](../static-site-generators).
