---
layout: post
slug: "instagram"
title: "相册"
noDate: "true"
---

<div class="instagram" data-client-id="956dd096b6e5496aba6662165b9b8443" data-user-id="438522285">
    <a href="https://www.instagram.com/hackhookgeek/" target="_blank" class="open-ins">图片来自instagram，正在加载中…</a>
	</div>
	<!--<script src="/js/jquery.lazyload.js"></script>-->
	<script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-instagram/0.3.1/instagram.min.js"></script>
    <script>
jQuery(function($) {
  $('.instagram').on('willLoadInstagram', function(event, options) {
    console.log(options);
  });
  $('.instagram').on('didLoadInstagram', function(event, response) {
    console.log(response);
  });
  $('.instagram').instagram({
    clientId: '956dd096b6e5496aba6662165b9b8443'
  });
});
</script>