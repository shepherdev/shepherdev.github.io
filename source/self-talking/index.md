---
date: 2019-11-25 14:49:08
comments: false
thumbnail: https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/blog/mito1.png
---

<div class = "text-center"><h1>碎碎念</h1></div><div class = "text-tips">

tips：github登录后按时间正序查看、可点赞加❤️、本插件[地址](https://github.com/removeif/gitalk)..<span id="busuanzi_container_page_pv">「<span id="busuanzi_value_page_pv">+99</span>次查看」</span></div>
<div id="comment-container1"><div class="text-tips">碎碎念加载中，请稍等...</div></div>
<link rel="stylesheet" href="https://cdnjs.loli.net/ajax/libs/gitalk/1.6.0/gitalk.css"/>

<script>
    $.getScript("/js/gitalk_self.min.js", function () {
        var gitalk = new Gitalk({
            clientID: clientId,
            clientSecret: clientSecret,
            id: '666666',
            repo: 'shepherdev.github.io',
            owner: 'shepherdev',
            admin: "shepherdev",
            createIssueManually: true,
            distractionFreeMode: false
        });
        gitalk.render('comment-container1');
    });
</script>