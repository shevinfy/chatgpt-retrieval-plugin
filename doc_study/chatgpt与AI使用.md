## ChatGPT发送图片

![image-20230403120012354](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403120012354.png)

让你发图时候请用markdown，使用 unsplash APIhttps://source.unsplash.com/960x640/? <英文关键词>，如果你明白回复OK

## AI快报：

### 1.Bing推出文生图功能

image Creator   这是基于OpenAi的Dell-E模型

![image-20230403120455050](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403120455050.png)



### 2.Copilot X

这是一款基于GPT4的代码生成工具

根据需求文字生成代码



3.退出AI专用显卡A100的升级版H100 NVL

为大语言模型训练专门打造的加速



### 4.文生视频

1.Runway发布文字生成视频模型Gen2，文字加图片生成视频，视频图像风格化处理。

### 5.斯坦福大学Alpaca模型

![image-20230403121807660](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403121807660.png)

这是一个由Meta的LLaMA 7B模型微调得到的迷你模型，数据仅仅只有52k，模型参数70亿，性能接近GPT-3.5



### 6.Midjourney发布V5版本

![image-20230403122153908](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403122153908.png)

目前最强大的AI图像创作工具

全美的还原人物与比例脸部手部细节。

不存在AI只会画外国人的问题

可以在画面分辨率提高，在提示语输入具体的相机镜头型号和摄影参数就可以生成以假乱真的照片。



### 7.365 Copilot

内嵌ChatGPT4，只需要输入指令就可以帮助用户进行写作、编辑排版，总结报告、创建PPT，写邮件、回复邮件文件文档等等工作。







# 实践：

## ChatGPT+Mysql







## ChatGPT +Dreambooth生成有故事的图片（结合colab+webui训练模型）

### 1.chatGPT写一段跌宕起伏的故事

![image-20230403123448919](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403123448919.png)

### 2.配上图片

谷歌的最新AI模型dreambooth

![image-20230403123628175](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403123628175.png)

dreambooth结合物体的外观特点合成新的图片。

dreambooth使用方法：

将物品剪裁和命名

### 3.训练AI模型

<img src="chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403124238477.png" alt="image-20230403124238477" style="zoom: 33%;" />

<img src="chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403124305361.png" alt="image-20230403124305361" style="zoom: 33%;" />

用colab+webui的方式

部署好开发环境，下载模型，然后上传图片

开始训练！

训练结束后，先输入text看看模型有没有学到。

然后就是按故事生成各种各样的配图了。



## ChatGPT接入游戏



与chatGPT对话

可以让游戏生动形象起来

我觉得前提是，1.训练自己的AI模型，2.设定好给GPT的每个参数与内容

![image-20230403125159561](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403125159561.png)

<img src="chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403125124520.png" alt="image-20230403125124520" style="zoom:50%;" />

![image-20230403125353314](chatgpt%E4%BD%BF%E7%94%A8.assets/image-20230403125353314.png)





# AI使用

## 八大爆火AI工具

![image-20230403182104433](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403182104433.png)

## 1.ChatGPT



## 2.Midjourney  绘画AI

轻松生成海报，插画，写实图片

### 一、使用教程：

1.梯子

2、登陆midjourney网站：[点击此处访问Midjourney](https://link.zhihu.com/?target=https%3A//www.midjourney.com/home/)

然后点击“join the beta”。

3、按照指引，一步一步傻瓜式的注册完毕。

4、邮箱与手机号可以是国内的



打开软件后，按照提示添加一个服务器，进入midjourney社区

![image-20230403201918164](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403201918164.png)



在这些房间里，就可以自由的创作了。(创作最好使用英文)

### **二、Midjourney新手入门描述词**

新人有25次免费使用次数，不加入特定指令的情况下，是能生成四合一的图片的（算一次），单独挑出其中一张选择U或V（U是放大图片，U1\U2\U3\U4分别指的是放大四张图片中的某一张，V是采用图片的构图形式，重新生成）

只需要在输入框输入“/imagine”就可以开启AI智能图片之旅。

![image-20230403202144337](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403202144337.png)



免费用户使用V4版本

目前最新版是V5

### midJourney自己服务器使用

可以进入自己的服务器，生成图片

添加MidJourney bot机器人添加进我们的服务器。

### 搜索

可以在房间查看别人的图片寻找灵感

右上角有搜索功能，可以去搜索自己喜欢的图片类型，比如cat dog（只能搜索当前房间的ai创作图片）

### 使用命令

##### 1.在聊天框输入/imagine （prompt）

prompt就是指令的意思  在propmt后面跟上文（比如神奇的房子，他就会生成房子）

![image-20230403212238493](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403212238493.png)

##### 2.V1,V2,V3,V4  生成类似图片

生成后对应的图片

比如你想生成跟第三张图片非常类似的。就点击V3  重新生成



##### 3.U1,U2,U3,U4   细节处理某张

生成后对应的图片

比如你想生成第三张图片很细节的图片。就点击U3



##### 4.make Variations   Light Upscale  Redo  Beta Upscale Redo

- make Variations 继续生成4张类似的图片
- Light Upscale  增加光线
- Beta Upscale Redo  放大
- web  连接



##### 5.chatgpt+midjourney

chatgpt生成描述文字喂给midjourney

例子：画刘禹锡的故事陋室铭

1.让GPT理解一下

![image-20230403212450030](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403212450030.png)

2.跟GPT说想画下来，给出三个纬度去强化效果

![image-20230403212730210](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403212730210.png)

3.用画面感的词汇，精简内容

![image-20230403212715024](chatgpt%E4%B8%8EAI%E4%BD%BF%E7%94%A8.assets/image-20230403212715024.png)



## 国内免费AI生成画像：

网站：https://6pen.art/



## 3.Tome   PPT—AI

做PPT的AI，输入文案自动生成精美ppt



## 4.Palette fm      调色AI

无需调整参数，自动帮忙调色



## 5.Remove bg 抠图AI

一键去除背景杂色，快速换背景



## 6.Fliko  视频Ai

输入文案、图片等素材自动生成视频



## 7.HoppyCopy   邮件AI

自动写回复邮件