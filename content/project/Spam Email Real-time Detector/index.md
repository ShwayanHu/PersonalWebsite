---
title: Spam Email Real-time Detector
date: 2023-04-28
# external_link: https://pypi.org/project/SpamEmailDetector/
tags:
  - Natural Language Processing
  - Machine Learning
---

This package is a spam email detector. This package can continuesly listen to the email address provided by the user, and print a message when a new email is received, indicating whether the email is spam or normal.

Note that the package works only (at least primarily) for Chinese context. When applied to English context, the performance may worsen greatly.

[PyPI HomePage](https://pypi.org/project/SpamEmailDetector/)

# How to use this package?

## Download this package

Download this package by running command line:

```shell
pip install SpamEmailDetector
```

## Continuesly listen to email

```python
from SpamEmailDetector.EmailListener import EmailListener

myEmail = EmailListener(email=yourEmailAddress,password=yourPasscode,detectorName=nameOfDetector)
myEmail.startListening(10)
```

Where, `yourPasscode` is the authorization password of your email address. If you don't know what `yourPasscode` is, then find it from the setting page of your email official website. **Note it's not your password for logging in.** Parameter `detectorName` is the name of spam/normal detector model. Two models are provided in this package: `"BOWSpamDetector"` and `"TfIdfSpamDetector"`. Enter either `String` to select the model in need.

The method `.startListening(10)` means the interval of listening, in seconds. That is to say, the program will check if there is a new email in your email box and judge if it is spam or normal email for every 10 seconds.

Once you start listening to your email box, bellow message will display on the screen. 

```
Start Listening:
-------------------------------------------------- 
Listening at  2023-04-28 11:39:51.582455
--------------------------------------------------
-------------------------------------------------- 
Listening at  2023-04-28 11:40:02.373163
--------------------------------------------------
-------------------------------------------------- 
Listening at  2023-04-28 11:40:12.511100
--------------------------------------------------
```

Once a new email has been detected, a message will note you whether it is a spam or normal email. For example, if it is a normal email:

```
-------------------------------------------------- 
Listening at  2023-04-28 11:40:22.642931
--------------------------------------------------
A new e-mail received!
This is a normal email!
```

To stop listening to your email box, just stop running the program. 

## Judge email from spam and normal

This package also includes two models that can determine whether a given text is more likely to be a spam email or a normal email. Two models included are `BOWSpamDetector` and `TfIdfSpamDetector` .

For example, if you want to use `BOWSpamDetector`, include code below into your python script:

```python
from SpamEmailDetector.BOWSpamDetector import BOWSpamDetector

# create your instance of the class
myInstance = BOWSpamDetector(normalTextFilePATH='./Dataset/normal.txt',spamTextFilePATH='./Dataset/spam.txt', stopWordsTextFilePATH='./Dataset/stopwords_master/baidu_stopwords.txt')
myInstance.preprocessEmails(ChineseOnly=False)

# For a given text TEXT(String)
TEXT = "..."
vectorizedText = myInstance.countVectorizerModel.transform(TEXT) # transform TEXT into vector
result = myInstance.naiveBayesModel.predict(vectorizedText) # result=0 or 1
```

If you want to use `TfIdfSpamDetector`, replace  `BOWSpamDetector` with the corresponding name.

## A short example: listening to my email

First, start listening to the given mail box by running code below:

```python
from SpamEmailDetector.EmailListener import EmailListener

myEmail = EmailListener(
  						email='xueyanhu******@*****', # hidden
  						password = '*********', # hidden
  						detectorName='BOWSpamDetector'
					)
myEmail.startListening(10) # Listen to my email box every 10 seconds
```

Second, send me myself ten emails with my own account every 10 seconds, by running the following code: (Sensitive info is hidden)

In the variable `testText`, only number 7 is spam email.

```python
import datetime
from pprint import pprint
from SpamEmailDetector.EmailListener import EmailListener
import zmail
import time

if __name__ == '__main__':
    testText= [
        "本期系列课特邀3位**布道师做客直播间，为您详解ArkTS和ArkUI基础知识，带你轻松学会ArkTS声明式语法的便捷使用，并手把手教你使用ArkUI搭建基础页面。最后通过一个健康生活案例实战带你掌握HarmonyoS数据管理方法、页面路由方法、弹窗与动画等特性，掌握基本的******应用开发能力。",
        "在大数据时代背景下，统计学作为大数据分析领域的基础显得尤为重要。为了帮助学生更好的学习和应用数据统计与分析的知识，促进统计、计算机、数学等相关专业的发展，培养具有数据分析与应用型人才，经研究决定，中国国际经济技术合作促进会教育发展工作委员会决定主办“第二届全国大学生数据统计与分析竞。竞赛目的在于为我国数据统计与分析行业提供人才支持，夯实人才队伍基础。不论是提高数据分析能力，还是提升自身就业竞争力，本次竞赛都是一个不错的选择！",
        "参加线上讲座的开发团队，可在讲座当天报名参与无障碍适配挑战活动，通过审核后我们将邀请你参加 5 月 18 日在上海设计与开发加速器举办的无障碍宣传日线下活动，在线下你将了解到更多无障碍开发技术，以及与其他开发者进行交流和互动。我们还将邀请使用无障碍功能的用户来分享他们的故事，了解 App 是如何赋能他们的日常生活；以及有经验的开发者来分享他们的工程实践，看如何在产品内部推进无障碍适配。你还可获得一对一咨询和深度辅导，获得针对你 App 的无障碍优化建议。",
        "为响应新文化建设的要求，积极探索“一精多会，一专多能”的国际化复合人才培养模式，提升高校学生的应用能力、跨文化沟通能力、实践能力、高质量就业能力，中国管理科学研究院教育标准化研究所决定主办2023年第二届全国大学生新媒体大赛。",
        "结果显示 ** 写入性能最大达到 ** 的 6.7 倍，InfluxDB 的 10.6 倍。此外，** 在写入过程中消耗了最少计算（CPU）资源和磁盘 IO 开销；相同落盘数据规模下，** 存储空间只有 InfluxDB 的 25%，只有 TimescaleDB 的 4%。此外，对于大多数查询类型，** 的性能均优于 InfluxDB 和 TimescaleDB，在 Complex queries 类型的查询中展现出巨大的优势——** 的 Complex queries 查询性能最高达到了 InfluxDB 的 37 倍、 TimescaleDB 的 28.6 倍。",
        "各位同学，教务处面向全校师生开展大创项目选题征集工作，有意申报的学生请认真阅读通知和附件1的指南，按要求填写附件3，并于4月25日下午五点之前发至邮箱**@163.com，谢谢。",
        "您已成功报名训练营，各赛道线上竞赛将于4月28日（本周五）起陆续开赛，我们将在开赛前通知您详细的参赛信息，请您注意查收短信及邮件。",
        "感谢你对****的关注！我们已经收到你的简历并会认真评估。通过评估后，我们会及时与你联系，请注意保持手机和邮件畅通。",
        "在过去一个月美国储户的“存款大迁徙”中，货币市场基金显然成为了大赢家。 面对银行业持续动荡，寻求更高收益率的投资者大批涌入了美国货币市场基金，这导致货币市场基金的资产规模一路飙升至了创纪录的水平。货币市场基金自身具有的避险吸引力和远远超过银行存款的收益率，吸引了大量的投资者。",
        "感谢您注册参加**线上外汇交易讲座，本期讲座时间为2023年4月25日北京时间晚 8:30-9:30 pm。 本次讲座将使用腾讯会议，建议您提前安装app。"
    ]
    testMail = [
        {'subject':"NO.{} Email".format(i),'content_text':content} for i,content in enumerate(testText)
    ]

    server = zmail.server('*******@*****','********')
    for i,mail in enumerate(testMail):
        pprint('sending Mail: \n{}'.format(mail))
        server.send_mail('*******@*****',mail)
        pprint('Mail sent at {}'.format(datetime.datetime.now()))
        if i==7: print('Mail sent is SPAM!')
        else: print('Mail sent is NORMAL!')
        time.sleep(10)
    print('ends')
```

The result is like: (red boxes are corresponding) an example of normal email

![](https://pic2.imgdb.cn/item/644bdc120d2dde57778ce401.png)

Another example of spam email.

![](https://pic2.imgdb.cn/item/644bdc610d2dde57778d3cac.png)

# Techniques applied

>   This part includes the details of the package.

Namely, Term Frequency Inversed Document Frequency(TfIdf) and Bag of Words (BOW) model of Chinese sentences are included in the package.

## Dataset

The dataset includes three parts:

-   `SpamEmailDetector/Dataset/normal.txt`: includes the normal emails, 5000 emails in total;
-   `SpamEmailDetector/Dataset/spam.txt`: includes the spam emails, 5000 emails in total;
-   `SpamEmailDetector/Dataset/stopwords_master`: a file folder containing four different the most frequently used stop words, both for Chinese and English. View the `SpamEmailDetector/Dataset/stopwords_master/README.md` for more detail of the four files.

## Model

We apply two methods of transforming text data into vectorized data, and use Naive Bayes model to do bi-classification.

As for the super parameters of the final model, we run a grid search of parameters to find the best fitter. We split the trainning and testing data by 66.66% to 33.33%, vectorize the trainining set accordingly, after tokenization with package `jieba` and deleting stop words. Then under pre-specified super parameters the model is trained. We use the test set to evaluate the performance of our model. 

>   Note that words appear in the testing set are not included when we test the performance of our model in test set. This is because in a training context, the model doesn't know which word will appear. This assumption also accords to reality.

For TfIdf-NaiveBayes model, the grid search result are as below:

![](https://pic2.imgdb.cn/item/644bde7b0d2dde57778fb3ae.jpg)

for BOW-NaiveBayes model, the grid search result are as below:

![](https://pic2.imgdb.cn/item/644bdcb20d2dde57778d9b2c.png)

For TfIdf-NaiveBayes, the model has tbe biggest F1 score when the $\alpha$ of Naive Bayes is 0.1 and the n-gram range from 1 and 2(uni-gram and bi-gram). For BOW-NaiveBayes, the biggest F1 score occurs when $\alpha$ of Naive Bayes is 0.1. 

After selecting the super parameters that best fit, we train the final model on the whole dataset. If you run `trainFinalModel` method, then the vectorizer and Naive Bayes model will be stored into two attributes: `self.countVectorizerModel` and `self.naiveBayesModel`, respectively.