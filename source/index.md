---
title: Profile
date: 2024-02-28 10:51:19
layout: profile
type: index
---
{% raw %}
<!DOCTYPE html>
<html>

<head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/jquery.fancybox@2.1.5/source/jquery.fancybox.css">
    <style>
      * {
        box-sizing: border-box;
      }
      html {
        height: 100%;
      }
      body {
        position: relative;
        min-height: 100%;
        background: #eee;
        color: #4c4948;
        font-size: 14px;
        font-family: Lato, Helvetica Neue For Number, -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, PingFang SC, Hiragino Sans GB, "Microsoft JhengHei", "MicrMicrosoft YaHei", Helvetica Neue, Helvetica, Arial, sans-serif;
        line-height: 2;
      }
      body {
        margin: 0;
      }
      h1, h2, h3, h4, h5, h6 {
        position: relative;
        margin: 1rem 0 .7rem;
        color: #344c67;
        font-weight: 700
      }
      hr:before {
        position: absolute;
        top: -10px;
        left: 5%;
        z-index: 1;
        color: #49b1f5;
        content: '\f0c4';
        font: normal normal normal 14px/1 FontAwesome;
        font-size: 20px;
        transition: all 1s ease-in-out;
      }
      hr {
        position: relative;
        margin: 2rem auto;
        width: calc(100% - 4px);
        border: 2px dashed #a4d8fa;
        background: #fff;
      }
      hr {
        box-sizing: content-box;
        height: 0;
        overflow: visible;
      }
      body, li, span, p {
        cursor: url(https://cdn.jsdelivr.net/gh/try0929/cdn/img/newText1.cur),auto;
      }
      a {
        background-color: transparent;
        cursor: url(https://cdn.jsdelivr.net/gh/try0929/cdn/img/newLoadCur.cur),auto;
      }
      strong{
        color: #FF0066;
      }
      #body-wrap {
        position: relative;
        display: box;
        display: flex;
        box-flex: 1;
        flex: 1 auto;
        -webkit-box-orient: vertical;
        flex-direction: column;
        transition: all .5s;
      }
      #nav.not_index_bg {
        height: 20rem;
      }
      #nav {
        position: relative;
        margin-top: -2rem;
        width: 100%;
        background-color: #49b1f5;
        background-attachment: local;
        background-position: center;
        background-size: cover;
      }
      #page_site-info {
        position: absolute;
        top: 10rem;
        width: 100%;
      }
      #site-title {
        color: #eee;
        text-align: center;
        text-shadow: 0.1rem 0.1rem 0.2rem rgba(0, 0, 0, .15);
        line-height: 1.5;
        font-weight: 700;
        font-size: 1.3rem;
        animation: titlescale 1s;
      }
      main {
        display: block;
      }

      #content-outer {
        -webkit-box-flex: 1;
        -moz-box-flex: 1;
        -o-box-flex: 1;
        box-flex: 1;
        -webkit-flex: 1 auto;
        -ms-flex: 1 auto;
        flex: 1 auto;
      }
      h1 {
        font-size: 2em;
        margin: .67em 0;
      }
      .layout_page {
        display: flex;
        -webkit-box-align: start;
        align-items: flex-start;
        margin: 0 auto;
        <!-- padding: 0 5px; -->
        max-width: 1400px;
      }

      #page{
        margin-top: 10px;
        width: 75%;
        border-radius: 3px;
        background: #fff;
        box-shadow: 0 4px 8px 6px rgba(7, 17, 27, .06);
        transition: all .3s;
      }
      #page{
        padding: 20px 44px 44px;
      }

      #aside_content {
        width: 25%;
      }
      #aside_content .card-widget {
        overflow: hidden;
        margin: 10px 0;
        border-radius: 3px;
        background: #fff;
        -webkit-box-shadow: 0 4px 8px 6px rgba(7, 17, 27, .06);
        box-shadow: 0 4px 8px 6px rgba(7, 17, 27, .06);
        -webkit-transition: all .3s;
        -moz-transition: all .3s;
        -o-transition: all .3s;
        -ms-transition: all .3s;
        transition: all .3s;
      }
      #aside_content .card-content {
        padding: 1rem 1.2rem;
      }

      .skillbar {
        position: relative;
        display: block;
        max-width: 360px;
        margin: 15px 10px;
        background: #eee;
        height: 30px;
        border-radius: 35px;
        -moz-border-radius: 35px;
        -webkit-border-radius: 35px;
        -webkit-transition: 0.4s linear;
        -moz-transition: 0.4s linear;
        -o-transition: 0.4s linear;
        transition: 0.4s linear;
        -webkit-transition-property: width, background-color;
        -moz-transition-property: width, background-color;
        -o-transition-property: width, background-color;
        transition-property: width, background-color;
      }

      .skillbar .skillbar-title {
        position: absolute;
        top: 0;
        left: 0;
        width: 110px;
        font-size: 0.9rem;
        color: #ffffff;
        border-radius: 35px;
        -webkit-border-radius: 35px;
        -moz-border-radius: 35px;
      }
      .skillbar .skillbar-title span {
        display: block;
        background: rgba(0, 0, 0, 0.15);
        padding: 0 20px;
        height: 30px;
        line-height: 30px;
        border-radius: 35px;
        -webkit-border-radius: 35px;
        -moz-border-radius: 35px;
      }
      .skillbar .skill-bar-percent {
        position: absolute;
        right: 10px;
        top: 0;
        font-size: 12px;
        height: 30px;
        line-height: 30px;
        color: #ffffff;
        color: rgba(0, 0, 0, 0.5);
      }

      .tool a {
        color: #FF0066;
      }

      .is-center {
        text-align: center;
      }

      #aside_content .card-info img {
        display: inline-block;
        padding: 5px;
        width: 120px;
        height: 120px;
        border-radius: 70px;
        vertical-align: top;
        transition: all .3s;
      }
      img {
        max-width: 100%;
        transition: all .2s;
      }
      .avatar-img {
        animation: avatar_turn_around 2s linear infinite;
      }
      #aside_content .card-info .author-info__name {
        font-weight: 500;
        font-size: 1.1rem;
      }
      aside_content .card-info .author-info__description {
        margin-top: -.3rem;
      }
      #aside_content .card-info .card-info-social-icons {
        margin: .3rem 0 -.3rem;
      }
      #aside_content .card-info .card-info-social-icons .social-icon {
        margin: 0 .5rem;
        color: #4c4948;
        font-size: 1.5rem;
      }

      #aside_content .item-headline {
        font-size: .8rem;
      }
      #aside_content .item-headline span {
        margin-left: .5rem;
      }

      #aside_content .card-webinfo .webinfo {
        padding: .2rem 1rem;
      }
      #aside_content .card-webinfo .webinfo .webinfo-item {
        display: block;
        padding: 4px 0 0;
      }
      #aside_content .card-webinfo .webinfo .webinfo-item div:first-child {
        display: inline-block;
      }
      #aside_content .card-webinfo .webinfo .webinfo-item div:last-child {
        display: inline-block;
        float: right;
      }

      @media screen and (min-width: 768px){
        #site-title {
            font-size: 2rem;
        }

      }

      @media screen and (max-width: 768px){
        .layout_page {
            <!-- padding: 0 5px !important; -->
        }
        #page {
            margin: 0;
            padding: 1.8rem 1.3rem;
        }
      }

      @media screen and (min-width: 900px){
        #aside_content .card-widget {
            margin-right: 15px;
        }
      }

    </style>
</head>

<body>
    <div id="body-wrap">
        <nav class="not_index_bg" id="nav" style="background-image:url(https://api.ixiaowai.cn/api/api.php)">
            <div id="page_site-info">
                <div id="site-title">
                    <span class="blogtitle"></span>
                    <script src="https://cdn.jsdelivr.net/npm/typed.js@2.0.11"></script>
                    <script>
                        var typed = new Typed(".blogtitle", {
                            strings: ['いつまでも飢えていろ、バカでいろ', 'Stay Hungry Stay Foolish'],
                            startDelay: 300,
                            typeSpeed: 100,
                            loop: true,
                            backSpeed: 50,
                            showCursor: true
                        });
                    </script>
                </div>
            </div>
        </nav>
        <main id="content-outer">
            <div class="layout_page" id="content-inner">
                <div class="aside_content" id="aside_content">
                    <div class="card-widget card-info">
                        <div class="card-content">
                            <div class="card-info-avatar is-center">
                                <img class="avatar-img"
                                    src="https://cdn.jsdelivr.net/gh/try0929/cdn/img/blog_avatar.JPG"
                                    alt="avatar">
                                <div class="author-info__name">ぷいけん</div>
                                <div class="author-info__description">食べ物が大好きで、たまにプログランミングも大好きなぷいけんです。</div>
                            </div>
                            <div class="card-info-social-icons is-center">
                                <a class="social-icon" href="https://github.com/TRY0929" target="_blank">
                                    <i class="fa fa-github"></i>
                                </a>
                            </div>
                        </div>
                    </div>

                    <div class="card-widget card-announcement">
                        <div class="card-content">
                            <div class="item-headline">
                                <i class="fa fa-bullhorn" aria-hidden="true"></i>
                                <span>一言</span>
                            </div>
                            <div id="hitokoto"></div>
                        </div>
                    </div>
                    <!-- <div class="card-widget card-announcement">
                        <div class="card-content">
                            <div class="item-headline">
                                <i class="fa fa-calendar" aria-hidden="true"></i>
                                <span>今日诗词</span>
                            </div>
                            <div id="jinrishici-sentence"></div>
                        </div>
                    </div> -->

                </div>
                <article id="page">
                    <div class="article-container">
                        <h2>スキル</h2>
                        <div class="skillbox">
                            <div class="skillbar">
                                <div class="skillbar-title"
                                    style="background: linear-gradient(to right, #FF0066 0%, #FF00CC 100%); width: 75%">
                                    <span>Javascript</span>
                                </div>
                                <div class="skill-bar-percent">75%</div>
                            </div>
                            <div class="skillbar">
                                <div class="skillbar-title"
                                    style="background: linear-gradient(to right, #9900FF 0%, #CC66FF 100%); width: 60%">
                                    <span>nodejs</span>
                                </div>
                                <div class="skill-bar-percent">60%</div>
                            </div>
                            <div class="skillbar">
                                <div class="skillbar-title"
                                    style="background: linear-gradient(to right, #2196F3 0%, #42A5F5 100%); width: 50%">
                                    <span>Java</span>
                                </div>
                                <div class="skill-bar-percent">50%</div>
                            </div>
                            <div class="skillbar">
                                <div class="skillbar-title"
                                    style="background: linear-gradient(to right, #00BCD4 0%, #f54009 100%); width: 50%">
                                    <span>SpringBoot</span>
                                </div>
                                <div class="skill-bar-percent">50%</div>
                            </div>
                            <div class="skillbar">
                                <div class="skillbar-title"
                                    style="background: linear-gradient(to right, #FFEB3B 0%, #FFF176 30%); width:30%">
                                    <span>Algorithm</span>
                                </div>
                                <div class="skill-bar-percent">30%</div>
                            </div>
                        </div>

                        <hr>
                        <h2>競技プログランミング</h2>
                        <div class="tool">
                            <a href="https://leetcode-cn.com/problemset/all/" target="_blank">Leetcode</a><br>
                            <a href="https://atcoder.jp/" target="_blank">AtCoder</a>
                        </div>

                        <hr>
                    </div>
                </article>
            </div>
        </main>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.4.1/dist/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jquery.fancybox@2.1.5/source/jquery.fancybox.js"></script>
    <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/instant.page@3.0.0/instantpage.js" type="module"></script>
    <script src="https://cdn.jsdelivr.net/npm/lazysizes@5.2.0/lazysizes.min.js" async></script>

    <!--   一言、今日诗词   -->
    <script src="https://sdk.jinrishici.com/v2/browser/jinrishici.js" charset="utf-8"></script>

    <script type="text/javascript">


        (function ($) {
            $.ajax('https://api.github.com/zen')
              .done(function (text) {
                $('#hitokoto').text(text);
              })
            $.fn.snow = function (options) {
                var $flake = $('<div id="snowbox" />').css({ 'position': 'absolute', 'z-index': '9999', 'top': '-50px' }).html('🌸'), // 🌸
                    documentHeight = $(document).height(),
                    documentWidth = $(document).width(),
                    defaults = {
                        minSize: 10,
                        maxSize: 20,
                        newOn: 1000,
                        flakeColor: "#AFDAEF" /* 此处可以定义雪花颜色，若要白色可以改为#FFFFFF */
                    },
                    options = $.extend({}, defaults, options);
                var interval = setInterval(function () {
                    var startPositionLeft = Math.random() * documentWidth - 100,
                        startOpacity = 0.2 + Math.random() / 2,
                        sizeFlake = options.minSize + Math.random() * options.maxSize,
                        endPositionTop = documentHeight - 200,
                        endPositionLeft = startPositionLeft - 500 + Math.random() * 500,
                        durationFall = documentHeight * 10 + Math.random() * 5000;
                    $flake.clone().appendTo('body').css({
                        left: startPositionLeft,
                        opacity: startOpacity,
                        'font-size': sizeFlake,
                        color: options.flakeColor
                    }).animate({
                        top: endPositionTop,
                        left: endPositionLeft,
                        opacity: 0.2
                    }, durationFall, 'linear', function () {
                        $(this).remove()
                    });
                }, options.newOn);
            };
        })(jQuery);
        $(function () {
            $.fn.snow({
                minSize: 5, /* 定义雪花最小尺寸 */
                maxSize: 50,/* 定义雪花最大尺寸 */
                newOn: 800  /* 定义密集程度，数字越小越密集 */
            });
        });

        // 浏览器搞笑标题
        var OriginTitle = document.title;
        var titleTime;
        document.addEventListener('visibilitychange', function () {
            if (document.hidden) {
                // $('[rel="icon"]').attr('href', "/funny.ico");
                document.title = 'バイバイー╭(°A°`)╮ ';
                clearTimeout(titleTime);
            }
            else {
                $('[rel="icon"]').attr('href', "https://cdn.jsdelivr.net/gh/try0929/cdn/img/ice-cream.ico");
                document.title = 'おかえりー(ฅ>ω<*ฅ) ~' + OriginTitle;
                titleTime = setTimeout(function () {
                    document.title = OriginTitle;
                }, 2000);
            }
        });
    </script>
</body>

</html>

{% endraw %}