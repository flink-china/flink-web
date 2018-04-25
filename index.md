---
title: "高可扩展的流与批统一数据处理引擎"
layout: base
---
<div class="row-fluid">

  <div class="col-sm-10 col-sm-offset-1 homecontent">
    <p class="lead" markdown="span">Apache Flink® 是一款为**分布式、高性能、高可用、高精确**的数据流应用而生的开源流式处理框架。</p>
    <a href="{{ site.baseurl }}/introduction.html" class="btn btn-default btn-intro">Apache Flink 介绍</a>
  </div>

<div class="col-sm-12">
  <hr />
</div>

</div>



<div class="row front-graphic">
  <img src="{{ site.baseurl }}/img/flink-home-graphic-update.svg" width="700px" />
</div>

<!-- Updates section -->



<!-- Powered by section -->

<div class="row-fluid">
  <div class="col-sm-12">


  <hr />
    <h2><a href="{{ site.baseurl }}/poweredby.html">由Flink提供支持</a></h2>



  <div class="jcarousel">
    <ul>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/alibaba-logo.png" width="175"  alt="Alibaba" /></div>
          <!--<span>Alibaba uses Flink for real-time search optimization.</span>-->

        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/bouygues-logo.jpg" width="175"  alt="Bouygues" /></div>
          <!-- <span>Bouygues Telecom uses Flink for network monitoring.</span> -->
        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/capital-one-logo.png" width="175"  alt="Capital One" /></div>
          <!-- <span>Capital One uses Flink for anomaly detection.</span> -->
        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/ericsson-logo.png" width="175"  alt="Ericsson" /></div>
          <!-- <span>Ericsson uses Flink for .</span> -->
        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/king-logo.png" width="175" alt="King" /></div>
          <!-- <span>King uses Flink to power real-time game analytics.</span> -->
        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/otto-group-logo.png" width="175" alt="Otto Group" /></div>
          <!-- <span>Otto Group uses Flink for.</span> -->
        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/researchgate-logo.png" width="175" alt="ResearchGate" /></div>
          <!-- <span>ResearchGate uses Flink for.</span>        -->
        </li>
        <li>
          <div><img src="{{ site.baseurl }}/img/poweredby/zalando-logo.jpg" width="175" alt="Zalando" /></div>
          <!-- <span>Zalando goes big with Flink.</span> -->
        </li>
    </ul>
  </div>

  <a href="#" class="jcarousel-control-prev" data-jcarouselcontrol="true"><span class="glyphicon glyphicon-chevron-left"></span></a>
  <a href="#" class="jcarousel-control-next" data-jcarouselcontrol="true"><span class="glyphicon glyphicon-chevron-right"></span></a>

  </div>

</div>

<script type="text/javascript" src="{{ site.baseurl }}/js/jquery.jcarousel.min.js"></script>

<script type="text/javascript">

  $(window).load(function(){
   $(function() {
        var jcarousel = $('.jcarousel');

        jcarousel
            .on('jcarousel:reload jcarousel:create', function () {
                var carousel = $(this),
                    width = carousel.innerWidth();

                if (width >= 600) {
                    width = width / 4;
                } else if (width >= 350) {
                    width = width / 3;
                }

                carousel.jcarousel('items').css('width', Math.ceil(width) + 'px');
            })
            .jcarousel({
                wrap: 'circular',
                autostart: true
            });

        $('.jcarousel-control-prev')
            .jcarouselControl({
                target: '-=1'
            });

        $('.jcarousel-control-next')
            .jcarouselControl({
                target: '+=1'
            });

        $('.jcarousel-pagination')
            .on('jcarouselpagination:active', 'a', function() {
                $(this).addClass('active');
            })
            .on('jcarouselpagination:inactive', 'a', function() {
                $(this).removeClass('active');
            })
            .on('click', function(e) {
                e.preventDefault();
            })
            .jcarouselPagination({
                perPage: 1,
                item: function(page) {
                    return '<a href="#' + page + '">' + page + '</a>';
                }
            });
    });
  });

</script>
