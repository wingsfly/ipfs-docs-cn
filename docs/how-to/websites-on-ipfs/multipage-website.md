---
title: 多页面网站
description: Learn how to host a website with multiple pages and external assets on IPFS.
---

# 多页面网站

本教程中，会说明如何在IPFS中托管多页面，且包含外部资源的网站。此教程是面向网站开发者，说明如何在IPFS中构建网站和应用的第二部分。前一部分的教程不是必须了解的，但是如果你刚开始接触IPFS生态，建议开始本章节前阅读[单页面网站教程](/how-to/websites-on-ipfs/single-page-website)，它会为你提供一个坚实的基础。

## 先决条件

如果已经阅读前一教程，应该已经安装了IPFS Desktop应用，如若没有，可以从此获取[IPFS Shipyard](https://github.com/ipfs-shipyard/ipfs-desktop)。

## 项目配置

在深入了解IPFS之前，我们需要首先创建这个小型项目所需要的相关文件。

1. 创建一个目录，名为`multi-page-first-step`。
1. 在此新目录中，创建一个名为`index.html`的文件，将以下代码粘贴进去。我们还将继续使用前一教程的[Random Planet Facts](http://randomplanetfacts.xyz)网站，同时添加一个链接，指向 _About_ 页面：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Random Planet Facts</title>
    <meta
      name="description"
      content="Get a random fact about a planet in our solar system."
    />
    <meta name="author" content="The IPFS Docs team." />
    <style>
      body {
        margin: 15px auto;
        max-width: 650px;
        line-height: 1.2;
        font-family: sans-serif;
        font-size: 2em;
        color: #fff;
        background: #444;
      }
      a {
        color: yellowgreen;
      }
    </style>
  </head>
  <body onload="main()">
    <h1>Random Planet Facts</h1>
    <img src="moon-logo.png" />
    <p id="output_p"></p>
    <h2><a href="about.html">About this website</a></h2>
    <script>
      function main() {
        const facts = [
          'Mars is home to the tallest mountain in our solar system.',
          'Only 18 out of 40 missions to Mars have been successful.',
          'Pieces of Mars have fallen to Earth.',
          'One year on Mars is 687 Earth days.',
          'The temperature on Mars ranges from -153 to 20 °C.',
          'One year on Mercury is about 88 Earth days.',
          'The surface temperature of Mercury ranges from -173 to 427°C.',
          'Mercury was first discovered in 14th century by Assyrian astronomers.',
          'Your weight on Mercury would be 38% of your weight on Earth.',
          'A day on the surface of Mercury lasts 176 Earth days.',
          'The surface temperature of Venus is about 462 °C.',
          'It takes Venus 225 days to orbit the sun.',
          'Venus was first discovered by 17th century Babylonian astronomers.',
          'Venus is nearly as big as the Earth with a diameter of 12,104 km.',
          'The Earth's rotation is gradually slowing.',
          'There is only one natural satellite of the planet Earth, the moon.',
          'Earth is the only planet in our solar system not named after a god.',
          'The Earth is the densest planet in the solar system.',
          'A year on Jupiter lasts around 4333 earth days.',
          'The surface temperature of Jupiter is around -108°C.',
          'Jupiter was first discovered by 7th or 8th century Babylonian astronomers.',
          'Jupiter has 4 ring.',
          'A day on Jupiter lasts 9 hours and 55 minutes.',
          'Saturn was first discovered by 8th century Assyrians.',
          'Saturn takes 10756 days to orbit the Sun.',
          'Saturn can be seen with the naked eye.',
          'Saturn is the flattest planet.',
          'Saturn is made mostly of hydrogen.',
          'Four spacecraft have visited Saturn.',
          'Uranus was discovered by William Herschel in 1781.',
          'A year on Uranus takes 30687 earth days.',
          'Uranus turns on its axis once every 17 hours, 14 minutes.',
          'With minimum atmospheric temperature of -224°C Uranus is nearly coldest planet in the solar system.',
          'Only one spacecraft has flown by Uranus, the Voyager 2.',
          'Neptune was discovered in 1846 by Urbain Le Verrier and Johann Galle.',
          'Neptune has 14 moons.',
          'The average temperatue of Neptune is about -201 °C.',
          'There is a 1:20 million scale model of the solar system in Sweden.',
          'The gap between the Earth and our moon is bigger than the diameters of all the planets combined.',
          'The first accurate calculation of the speed of light was using Jupiter's moons',
          'Jupiter's magnetic field is believed to be a result of rapidly spinning metallic hydrogen at the core, and is ~10x stronger than the Earth's.',
          'Venus spins backwards.',
          'Uranus spins sideways, relative to the ecliptic plane of the solar system.',
          'It is easier to reach Pluto or escape the solar system from Earth than being able to <i>land</i> on the Sun.'
        ]
        document.querySelector('#output_p').innerHTML =
          facts[Math.floor(Math.random() * facts.length)]
      }
    </script>
  </body>
</html>
```

1. 创建另一个文件，命名为`about.html`，并粘贴以下代码：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>About | Random Planet Facts</title>
    <meta
      name="description"
      content="Get a random fact about a planet in our solar system."
    />
    <meta name="author" content="The IPFS Docs team." />
    <style>
      body {
        margin: 15px auto;
        max-width: 650px;
        line-height: 1.2;
        font-family: sans-serif;
        font-size: 2em;
        color: #fff;
        background: #444;
      }
      a {
        color: yellowgreen;
      }
    </style>
  </head>
  <body>
    <h1>Random Planet Facts</h1>
    <p>
      This website gives you random facts about the <i>planets</i> our solar
      system! Refresh the homepage to see a new fact!
    </p>
    <h2><a href="index.html">Go back home</a></h2>
    <footer>
      <hr />
      Created by ___.
    </footer>
  </body>
</html>
```

1. 在`Created by ___.`行加入你的名字，不然每个学习此教程的人都复制/粘贴了相同的代码，结果都得到了完全相同的CID！这本身没有什么问题，但对你的项目来说，可以生成一个唯一的CID显得更有意思一些。
1. 最后，下载此图片，将其保存到目录中，并命名为`moon-logo.png`：

   ![Icon of the moon with stars and circles in the background.](./images/multipage-website/moon-logo.png)

现在整个目录的结构应该如下：

```
  multi-page-first-step/
  ├── about.html
  ├── index.html
  └── moon-logo.png
```

## 添加文件到IPFS

现在整个项目已经就绪了，我们可以通过IPFS Desktop把它们添加到IPFS中。这里我们不会单独添加每个文件，而是添加整个项目的目录，IPFS Desktop会帮我们处理好后续的事情！

1. 打开IPFS Desktop应用，然后选择**Add** > **Folder**。
1. 选择`multi-page-website`目录，一旦添加完成，你应该在应用中可以看到这个文件夹：

   ![The IPFS Desktop application with the multi-page project folder showing.](./images/multipage-website/ipfs-desktop-with-multi-page-folder-showing.png)

1. 点击右侧的`...`菜单，然后选择**Share link**。
1. 点击**Copy**，然后将链接粘贴到浏览器地址栏中，你应该可以看到这个网站及其Logo!

   ![Random space facts open in a Firefox browser window.](./images/multipage-website/website-open-in-firefox.png)

   尝试点击about页面的链接，你应该可以正常的访问这些页面。

## 发布到IPNS

:::tip 这一步是可选的
并不必须完成这一步，然而，它提供了一些关于IPNS和IPFS如何协作的有用信息。
:::

通过CID来获取内容是很好的方案，这可以确保用户总能获得他们所期望的内容。但是当用户只想获取某个内容的最新版本，但并不知道所对应的具体内容时该怎么办呢？这时可以通过IPNS来轻松满足。

在这种情况下，你无需分享你网站的CID，而是发布它的根CID到IPNS中，然后分享你从IPNS中获得的 _key_。

1. 打开一个终端控制台，进入你的multi-page项目所在的目录：

    ```shell
    cd ~/Code/random-planet-facts
    ```

1. 通过运行`ipfs add -r .`，再次确认此项目已经添加到IPFS中：

    ```shell
    ipfs add -r .

    > added QmP4KNjSaVCR3jTxi8nsMq3DDqGyVUXyc5vfij31J3B3vr multi-page-first-step/about.html
    > added QmYp2jy5t7knzwhkqPJ68amuAqLYJ3DG5vvxgJW6bFdQwN multi-page-first-step/index.html
    > added QmW8U3NEHx3p73Nj9645sGnGa8XzR43rQh3Kd52UKncWMo multi-page-first-step/moon-logo.png
    > added QmchJPQNLE5EUSYTzfzUsNFyPozXyANiZHFDSFKWdLNdRR multi-page-first-step
    > 12.65 KiB / 12.65 KiB [=====================================================] 100.00%
    ```

1. 复制`ipfs add`命令所输出的最后的一个CID`QmchJPQN...`。
1. 用`ipfs name publish /ipfs/QMchJPQN...`命令来发布你的项目到IPNS中，把`QMchJPQN...`替换为上一步所对应的CID：

    ```shell
    ipfs name publish /ipfs/QmchJPQNLE5EUSYTzfzUsNFyPozXyANiZHFDSFKWdLNdRR

    > Published to k51qzi5uqu5dh9gnl66grpnpuhj245ha1xq9ajtmuf7swe847zovdg1t9a0xiz: /ipfs/QmchJPQNLE5EUSYTzfzUsNFyPozXyANiZHFDSFKWdLNdRR
    ```

    这里`k51qzi...`形式的是你的IPFS安装程序所对应的key！通过这个key可以让其他人访问到你的数据内容。

1. 现在你可以通过访问`https://gateway.ipfs.io/ipns/k51qzi...`来查看你的项目，需要把`k51qzi...`替换为上一步所输出的key。
1. 然后当你对项目进行了任何修改的时候，只要简单的重新添加到IPFS中，然后把它再次发布到IPNS中：

    ```shell
    ipfs add -r .

    > ...
    > added QmchJPQNLE5EUSYTzfzUsNFyPozXyANiZHFDSFKWdLNdRR multi-page-first-step
    12.65 KiB / 12.65 KiB [=====================================================] 100.00%

    ipfs name publish QmchJPQNLE5EUSYTzfzUsNFyPozXyANiZHFDSFKWdLNdRR

    > Published to k51qzi5uqu5dh9gnl66grpnpuhj245ha1xq9ajtmuf7swe847zovdg1t9a0xiz: /ipfs/QmchJPQNLE5EUSYTzfzUsNFyPozXyANiZHFDSFKWdLNdRR
    ```

    现在可以重新打开`https://gateway.ipfs.io/ipns/k51qzi...`，以获取你的更新内容。

这里只是IPNS的冰山一角。[查看IPNS页面以了解更多 →](../../concepts/ipns)

## 下一步

在下一教程中，我们会了解如何[为网站绑定域名](/how-to/websites-on-ipfs/link-a-domain)

