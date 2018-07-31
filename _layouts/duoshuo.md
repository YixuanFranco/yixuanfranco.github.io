
                <!-- Duoshuo Share start -->
                <!--
                <style>
                    .ds-share{
                        text-align: right;
                    }

                    @media only screen and (max-width: 700px) {
                        .ds-share {

                        }
                    }
                </style>

                <div class="ds-share"
                    data-thread-key="{{page.id}}" data-title="{{page.title}}"
                    data-images="{{ site.url }}/{% if page.header-img %}{{ page.header-img }}{% else %}{{ site.header-img }}{% endif %}"
                    data-content="{{ content | strip_html | truncate:80 }} | Yixuan's Geek Area."
                    data-url="{{site.url}}{{page.url}}">
                    <div class="ds-share-inline">
                      <ul  class="ds-share-icons-16">

                        <li data-toggle="ds-share-icons-more"><a class="ds-more" href="#">分享到：</a></li>
                        <li><a class="ds-wechat flat" href="javascript:void(0);" data-service="wechat">微信</a></li>
                        <li><a class="ds-weibo flat" href="javascript:void(0);" data-service="weibo">微博</a></li>
                        <li><a class="ds-douban flat" href="javascript:void(0);" data-service="douban">豆瓣</a></li>
                      </ul>
                      <div class="ds-share-icons-more">
                      </div>
                    </div>
                <hr>
                </div>
                -->
                <!-- Duoshuo Share end-->


                <!-- 多说评论框 start -->
					<div class="ds-thread" data-thread-key="{{page.id}}" data-title="{{page.title}}" data-url="{{site.url}}{{page.url}}"></div>
                <!-- 多说评论框 end -->

                <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
                <script type="text/javascript">
                var duoshuoQuery = {short_name:"yixuan123"};
					(function() {
						var ds = document.createElement('script');
						ds.type = 'text/javascript';ds.async = true;
						ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
						ds.charset = 'UTF-8';
						(document.getElementsByTagName('head')[0]
						 || document.getElementsByTagName('body')[0]).appendChild(ds);
					})();
					</script>
                <!-- 多说公共JS代码 end -->


<!-- .......................................................... -->
