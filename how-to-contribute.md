---
title: "如何贡献"
---
Apache Flink的社区非常开放和友好。欢迎大家加入Flink社区并为Apache Flink做贡献。与社区互动，并未Flink做贡献，包括提问，提交错误报告，提出新功能，加入邮件列表进行讨论，贡献代码或文档，改进网站，或称为发布候选人，有以下几种方式：

{% toc %}

## 提问

Apache Flink社区非常希望帮助到您，并回答你的问题。您可以加入我们的[用户邮件组](http://flink-china.org/community.html#mailing-lists)，[加入IRC Channel](http://flink-china.org/community.html#irc)，或者在Stack Overflow上关注[apache-flink](https://stackoverflow.com/questions/tagged/apache-flink)标签。

-----

## 提交缺陷报告

如果您遇到了Flink相关的问题，请打开Flink的JIRA，点击上方蓝色‘创建按钮’，提交缺陷报告。请在缺陷报告中填写问题的详细信息，如果可能的话，请添加重现问题的描述。非常感谢。

-----

## 提出改进或者新功能

社区一直在努力寻找能够完善Apache Flink的反馈。如果您有能够提高Flink的新想法或者能够使Flink用户受益的新功能，请在[Flink JIRA](https://issues.apache.org/jira/projects/FLINK/issues)中提交问题。请尽量详细的描述这个新功能，包括其依赖及范围。详细信息非常重要，因为：

- 它能保证在开发新功能的过程中，所有前置条件都能满足。

- 它能帮助我们评估实现你需求的所需的人力及方案。

- 它能够激发关于此问题的建设性的讨论。

如果你打算自己实现这个新功能或新改进，也需要尽量详细的信息，同时请读一下[贡献代码](http://flink-china.org/contribute-code.html)规范。

我们建议你再动手写代码之前，先在社区讨论是否需要这个新功能以及如何实现新功能，并在社区就此问题达成一致。一些功能可能会超过Flink项目的范围，最好尽早讨论。

对一些会改动Flink基础的大功能，我们有另一个渠道：[Flink改进建议](https://cwiki.apache.org/confluence/display/FLINK/Flink+Improvement+Proposals)。如果感兴趣的话，你可以在那里提一个新功能或者关注一些已有的讨论。


-----

## 帮助他人并加入讨论

大多数Apache Flink社区的问题都在以下两个邮件组中进行讨论：

* 用户邮件组user@flink.apache.org：用户在该邮件组中提问，并寻求帮助或建议。加入用户邮件组来帮助他人，是非常好的贡献Flink社区的方式。同时，在Stack Overflow上，有[apache-flink](https://stackoverflow.com/questions/tagged/apache-flink)标签，你可以在那里帮助其他Flink用户并且收获一些知识。

* 开发邮件组dev@flink.apache.org：Flink开发人员使用该邮件组来交换想法，讨论新功能，发布以及整体开发进度。如果你想为Flink贡献代码，可以加入这个邮件组。

非常欢迎用户加入这两个[邮件组](http://flink-china.org/community.html#mailing-lists)。除了用户邮件组，你还可以加入[Flink IRC Channel](https://flink.apache.org/community.html#irc)来跟其他用户或贡献者交流问题。

-----

## 测试待发布版本

由于社区的积极推动，Apache Flink一直在持续改进。每隔几周，我们会发布一个包含缺陷修复，功能改进和新功能的版本。发发布新版本包括以下几个步骤：

1. 选择一个待发布版本并进行投票（投票通常会持续72小时）

2. 测试待发布版本并且投票（如果发布包没有问题，就+1，如果有问题，就-1）

3. 如果待发布版本有问题，我们就回到第一步，如果没有问题，就发布该版本

我们的wiki中，有一页专门介绍了[待发布版本的测试流程](https://cwiki.apache.org/confluence/display/FLINK/Releasing)。如果参与发布的人很少，那么发布测试就会是一项很繁重的工作。但是其实可以有更多的人加入到版本测试过程中。Flink社区鼓励大家积极参与版本发布的测试工作。通过测试发布版本，你也可以保证Flink下个发布版本在你的环境可以正常运行，同时可以帮助提高发布质量。

-----

## 贡献代码

志愿者贡献的代码在不断维护，改进和拓展Apache Flink的技术能力。Apache Flink社区鼓励大家都来贡献源代码。为了保证代码贡献者有愉快的贡献体验，同时保证高质量的代码库，我们在[代码贡献指南](http://flink-china.org/contribute-code.html)中拟定了一份代码贡献流程。指南中也包括了如何设置开发环境，编码规范和代码风格，同时也解释了如何提交代码。

请在贡献代码前，先读**[代码贡献指南](http://flink-china.org/contribute-code.html)**
也请读一下本章提交贡献者许可证协议部分。

### 寻找待解决问题?
{:.no_toc}

我们在[Flink's JIRA](https://issues.apache.org/jira/projects/FLINK/issues)中维护了一份已知问题，改进建议和功能建议的列表。对于那些适合新贡献者解决的问题，都标上了'starter'标签。这些任务解决起来都非常简单，并且能够帮助你熟悉项目和贡献流程。

如果你在寻找适合解决的问题，请关注[初学者问题列表](https://issues.apache.org/jira/issues/?jql=project%20%3D%20FLINK%20AND%20resolution%20%3D%20Unresolved%20AND%20labels%20%3D%20starter%20ORDER%20BY%20priority%20DESC)。当然你也可以选择解决[其他问题](https://issues.apache.org/jira/issues/?jql=project%20%3D%20FLINK%20AND%20resolution%20%3D%20Unresolved%20ORDER%20BY%20priority%20DESC)。对于你有兴趣解决的问题，可以随时提问。

-----

## 贡献文档

对于任何软件，好的文档都是非常重要的。对Apache Flink这种复杂的分布式数据处理引擎来说，更是如此。Apache Flink社区致力于提供简洁清晰完整的文档，并且欢迎大家以各种形式为Apache Flink贡献文档。

* 对于缺失，错误或者过时的文档，请提交[JARA issue](https://issues.apache.org/jira/projects/FLINK/issues/FLINK-8955?filter=allopenissues)。

* Flink文档以Markdown形式写成。在[Flink源代码库](http://flink-china.org/community.html#main-source-repositories)的docs目录。关于如何更新、改进以及贡献文档，请参阅[贡献文档指南](http://flink-china.org/contribute-documentation.html)获得更详细的信息。

-----

## 改进网站

Apache Flink的网站代表了Apache Flink及其整个社区。网页有几个用途，包括：

* Apache Flink以及其功能说明

* 鼓励访问者下载并使用Flink

* 鼓励访问者与投入社区建设

我们欢迎任何改进网站的行为。

* 如果你认为网站可以更好，请开一个[JIRA issue](issues.apache.org/jira/browse/FLINK)

* 如果你想更新或改进网站，请遵守[网站改进指南](https://flink.apache.org/improve-website.html)

-----

## 更多贡献方式

还有很多其他的贡献Flink社区的方式。例如，你可以

* 做一个关于Flink的演讲，告诉其他人你是怎么用Flink的

* 组织线下交流会或用户组

* 跟其他人讨论Flink 等等

-----

## 提交贡献者许可协议

如果你想贡献Flink，请向Apache软件基金会(ASF)提交贡献者许可协议。以下引用摘http://www.apache.org/licenses ,介绍了ICLA和CCLA，以及他们的作用。

> ASF希望所有为Apache项目贡献想法、代码或文档的贡献者，都通过邮件，信件或传真，签名并提交[个人贡献者许可协议](http://www.apache.org/licenses/icla.txt)（CLA）。该协议中清楚的定义了知识产权相关条目已经被捐献给了ASF。因此未来如果有对该软件法律上的争议，ASF会使用该部分知识产权相关条目。
如果公司允许其员工参与Apache项目，公司可以通过[公司贡献者许可协议](http://www.apache.org/licenses/cla-corporate.txt)来捐赠相关的只是产权，改部分内容应该已经包括在了员工雇佣合同的内容中。注意：公司贡献者许可协议与个人贡献者许可协议并不冲突，员工仍需要签署CLA来捐赠不包含在CCLA内容中的知识产权条目。
>

-----

## 如何成为提交者

提交者是拥有项目代码库写权限的人。比如，他们可以更改代码，文档和网站，也可以接受别人的贡献。
关于如何成为提交者，并没有很严格的约定。新提交者的候选人一般都是社区中比较活跃的贡献者。

成为社区活跃用户，意味着你需要参与到邮件组的讨论中，能够回答问题，测试发布版本，尊重他人，并且遵守社区任人唯贤的原则。“Apache方式”聚焦社区建设，这对社区建设是非常重要的。

当然，对社区贡献代码和文档也非常重要。非常好的融入方式是贡献改进建议，新功能以及修复缺陷。你需要对你贡献的代码负责，增加测试及文档，并帮助共同维护相关代码。

新提交者候选人通常由当前提交者或者PMC成员提名，并在内部投票产生。

如果你想成为提交者，你需积极参加社区活动，并开始按照以上方式为Apache Flink做贡献。你也可以与其他提交者讨论或者寻求他们的建议和指导。
