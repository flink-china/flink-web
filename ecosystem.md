---
title: "生态系统"
---
<br>
Apache Flink支持广泛的生态系统，并可与其无缝对接许多其他数据处理项目和框架。
<br>
{% toc %}

## 连接器

<p>连接器提供了与各种第三方系统接口的代码</p>

<p>目前支持这些系统：</p>

<ul>
  <li><a href="{{site.docs-stable}}/dev/connectors/kafka.html" target="_blank">Apache Kafka</a> (sink/source)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/elasticsearch.html" target="_blank">Elasticsearch</a> (sink)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/elasticsearch2.html" target="_blank">Elasticsearch 2.x</a> (sink)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/filesystem_sink.html" target="_blank">HDFS</a> (sink)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/rabbitmq.html" target="_blank">RabbitMQ</a> (sink/source)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/kinesis.html" target="_blank">Amazon Kinesis Streams</a> (sink/source)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/twitter.html" target="_blank">Twitter</a> (source)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/nifi.html" target="_blank">Apache NiFi</a> (sink/source)</li>
  <li><a href="{{site.docs-stable}}/dev/connectors/cassandra.html" target="_blank">Apache Cassandra</a> (sink)</li>
  <li><a href="https://github.com/apache/bahir-flink" target="_blank">Redis, Flume, and ActiveMQ (via Apache Bahir)</a> (sink)</li>
</ul>

要使用其中一个连接器运行应用程序，通常需要安装和启动额外的第三方组件，例如消息队列的服务器。关于这些的进一步说明可以在相应的小节中找到。


## 第三方项目

这是建立在Flink之上的第三方包列表（即库，系统扩展或示例）。
Flink社区收集这些软件包的链接，但不保留它们。
因此，他们不属于Apache Flink项目，社区不能给予他们任何支持。
**你的项目丢失了么?**
请在 [用户/开发邮件列表]（#邮件列表）告知我们。

**Apache Zeppelin**

[Apache Zeppelin](https://zeppelin.incubator.apache.org/) 是一款基于Web的notebook开发工具，支持交互式数据分析，
[Flink as an execution engine](https://zeppelin.incubator.apache.org/docs/interpreter/flink.html) (next to others engines).
详情请参阅 Jim Dowling的 [Flink Forward 话题](http://www.slideshare.net/FlinkForward/jim-dowling-interactive-flink-analytics-with-hopsworks-and-zeppelin) 关于 在Flink运行Zeppelin.

**Apache Mahout**

[Apache Mahout](https://mahout.apache.org/) 是在机器学习库中使用，Flink将他收录为底层执行引擎。
详情请参阅 Schelter的 [Flink Forward 话题](http://www.slideshare.net/FlinkForward/sebastian-schelter-distributed-machine-learing-with-the-samsara-dsl) 关于 Mahout-Samsara DSL.

**Cascading**

[Cascading](http://www.cascading.org/cascading-flink/) 级联使用户能够轻松地在Flink和其他执行引擎上构建复杂的工作流程。
[Cascading on Flink](https://github.com/dataArtisans/cascading-flink)是建设在 [dataArtisans](http://data-artisans.com/) 和[Driven, Inc](http://www.driven.io/).
详情请参阅Fabian Hueske的[Flink Forward 话题](http://www.slideshare.net/FlinkForward/fabian-hueske-training-cascading-on-flink) for more details.

**Apache Beam (incubating)**

[Apache Beam (incubating)](http://beam.incubator.apache.org/) 是一种开源的统一编程模型，您可以使用它来创建数据处理管道。 Flink是Beam编程模型支持的后端之一。


**GRADOOP**

[GRADOOP](http://dbs.uni-leipzig.de/en/research/projects/gradoop) 在Flink之上实现可扩展的图形分析，是由莱比锡大学开发。 更多详情请查看 [Martin Junghanns’ Flink Forward talk](http://www.slideshare.net/FlinkForward/martin-junghans-gradoop-scalable-graph-analytics-with-apache-flink).

**BigPetStore**

[BigPetStore](https://github.com/apache/bigtop/tree/master/bigtop-bigpetstore) 是一个包含数据生成器的基准测试套件，Flink将它收入其中。 详情请参阅
See Suneel Marthi的[Flink Forward talk](http://www.slideshare.net/FlinkForward/suneel-marthi-bigpetstore-flink-a-comprehensive-blueprint-for-apache-flink?ref=http://flink-forward.org/?session=tbd-3)预览 .

**FastR**

[FastR](https://bitbucket.org/allr/fastr-flink) 在Java中快速实现了R语言。 [FastR Flink](https://bitbucket.org/allr/fastr-flink/src/3535a9b7c7f208508d6afbcdaf1de7d04fa2bf79/README_FASTR_FLINK.md?at=default&fileviewer=file-view-default) 可以在Flink之上用R语言开发工作。

**Apache SAMOA**

[Apache SAMOA (incubating)](https://samoa.incubator.apache.org/) 即将推出一款基于Flink为特色的流媒体ML库。 Albert Bifet 介绍了如何运行 Flink在 SAMOA，请参阅他的 [Flink Forward talk](http://www.slideshare.net/FlinkForward/albert-bifet-apache-samoa-mining-big-data-streams-with-apache-flink?ref=http://flink-forward.org/?session=apache-samoa-mining-big-data-streams-with-apache-flink).

**Python Examples on Flink**

[collection of examples](https://github.com/wdm0006/flink-python-examples) Apache Flink的Python API的示例集合。

**WordCount Example in Clojure**

[WordCount example](https://github.com/mjsax/flink-external/tree/master/flink-clojure) 在Clojure中编写Flink程序的WordCount小示例。


**Anomaly Detection and Prediction in Flink**

[flink-htm](https://github.com/nupic-community/flink-htm) flink-htm是Apache Flink中用于异常检测和预测的库。 该算法基于由Numenta智能计算平台（NuPIC）实施的分层时间存储器（HTM）。


**Apache Ignite**

[Apache Ignite](https://ignite.apache.org) 是一个高性能，集成和分布式内存平台，用于实时计算和处理大规模数据集。 详情请参阅 [Flink接收器流式传输连接器](https://github.com/apache/ignite/tree/master/modules/flink) 将数据注入Ignite缓存。
