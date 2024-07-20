---
title: "Image Recognition Functionn"
description: 
date: 2024-07-20T13:31:01+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---

## 前言
今年发现一个本地使用纯前端就能实现物品识别代码，之前自己测试过yolo相关的图片检测需要配置各种驱动，环境非常麻烦，当然yolo的功能比较强大，性能也非常好，跟纯前端实现差别较大。



## 具体代码

```html

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>图片识别</title>
  <!-- CSS 样式 -->
  <style>
    .container {
      margin: 40px auto;
      width: max(50vw, 400px);
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .custom-file-upload {
      display: flex;
      align-items: center;
      cursor: pointer;
      gap: 10px;
      border: 2px solid black;
      padding: 8px 16px;
      border-radius: 6px;
    }

    #file-upload {
      display: none;
    }

    #image-container {
      width: 100%;
      margin-top: 20px;
      position: relative;
    }

    #image-container>img {
      width: 100%;
    }

    .bounding-box {
      position: absolute;
      box-sizing: border-box;
    }

    .bounding-box-label {
      position: absolute;
      color: white;
      font-size: 12px;
    }
  </style>
</head>

<body>
  <!-- 页面主体内容 -->
  <main class="container">
    <label for="file-upload" class="custom-file-upload">
      <input type="file" accept="image/*" id="file-upload">
      上传图片
    </label>
    <div id="image-container"></div>
    <p id="status"></p>
  </main>

  <!-- JavaScript 代码 -->
  <script type="module">
    // 导入transformers nlp任务的pipeline和env对象
    import { pipeline, env } from "https://cdn.jsdelivr.net/npm/@xenova/transformers@2.6.0"
    // 允许本地模型
    env.allowLocalModels = false;

    // 获取文件上传和图片容器元素
    const fileUpload = document.getElementById('file-upload');
    const imageContainer = document.getElementById('image-container')

    // 监听文件上传事件
    fileUpload.addEventListener('change', function (e) {
      const file = e.target.files[0];
      const reader = new FileReader();
      reader.onload = function (e2) {
        const image = document.createElement('img');
        image.src = e2.target.result;
        imageContainer.appendChild(image)
        detect(image)
      }
      reader.readAsDataURL(file)
    })

    // 获取状态信息元素
    const status = document.getElementById('status');

    // 检测图片的AI任务
    const detect = async (image) => {
      status.textContent = "分析中..."
      const detector = await pipeline("object-detection", "Xenova/detr-resnet-50")
      const output = await detector(image.src, {
        threshold: 0.1,
        percentage: true
      })
      output.forEach(renderBox)
    }

    // 渲染检测框函数
    function renderBox({ box, label }) {
      const { xmax, xmin, ymax, ymin } = box
      const boxElement = document.createElement("div");
      boxElement.className = "bounding-box"
      Object.assign(boxElement.style, {
        borderColor: '#123123',
        borderWidth: '2px',
        borderStyle: 'solid',
        left: 100 * xmin + '%',
        top: 100 * ymin + '%',
        width: 100 * (xmax - xmin) + "%",
        height: 100 * (ymax - ymin) + "%"
      })

      const labelElement = document.createElement('span');
      labelElement.textContent = label;
      labelElement.className = "bounding-box-label"
      labelElement.style.backgroundColor = '#000000'

      boxElement.appendChild(labelElement);
      imageContainer.appendChild(boxElement);
    }
![](https://fastly.jsdelivr.net/gh/filess/img13@main/2024/07/20/1721452050971-d50a17f8-e4ca-46f0-81bd-7c35c5917acf.png)

  </script>
</body>

</html>

```
在本地直接使用浏览器打开这个html文件就能实现效果了




# 部署
之前的文章分享过使用Vercel 部署ChatGPT网站，今天也使用Vercel 部署测试一下，非常简单，最主要是不会使用自己的任何的服务器资源，Vercel是一个云服务平台，支持静态网站和动态网站的应用部署、预览和上线，现在GitHub上面有很多主流项目都支持Vercel一键部署

部署单文件html 实测可以忽略掉Vercel.json 这个配置文件，把上面的代码复制到新建的html文件中上传到GitHub 上，使用GitHub登录Vercel ,直接导入（import）这个项目

![](C:/Users/15549/Pictures/文章图片/图片识别/Snipaste_2024-07-20_14-06-13.png)
![](C:/Users/15549/Pictures/文章图片/图片识别/Snipaste_2024-07-20_14-06-30.png)


然后点击部署（deploy） 不到一分钟就部署成功了，然后使用官方自带的二级域名就可以访问了
![](C:/Users/15549/Pictures/文章图片/图片识别/Snipaste_2024-07-20_14-08-20.png)

![](C:/Users/15549/Pictures/文章图片/图片识别/Snipaste_2024-07-20_14-10-07.png)

# 最后


Xenova/detr-resnet-50 是基于 Facebook 开发的 DETR（DEtection TRansformer）模型的改进版本，该模型在 COCO 2017 数据集上进行端到端训练，用于目标检测任务。

Xenova/detr-resnet-50 模型可以通过 Transformers.js 库在前端实现。
Transformers.js 是一个用于处理 Transformer 模型的 JavaScript 库，它提供了丰富的 API，可以方便地加载、使用和部署 Transformer 模型。

可以在huggingface 找到Transformers.js 库的说明（https://huggingface.co/docs/transformers.js/index），还有其他很多的任务的能力，有兴趣的同学可以看看，例如：

📝 自然语言处理：文本分类、命名实体识别、问题解答、语言建模、摘要、翻译、多项选择和文本生成。
🖼️ 计算机视觉：图像分类、物体检测和分割。
🗣️ 音频：自动语音识别和音频分类。
🐙 多模态：零样本图像分类。






