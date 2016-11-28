---
layout:     post
title:      "Django Ajax 获取数据"
date:       2016-11-28 13:47:21
author:     "RM"
header-img: "img/home-bg.jpg"
tags:
    - Django
    - Python
    - 前端
---

<div>
<p>function refresh(obj)</p>
<p>{</p>
<p>	console.log($(obj).attr("key"));</p>
<p>	if ($(obj).parent().children("ul").text() =="" ||$(obj).parent().children("ul").text() == null)</p>
<p>		$(obj).parent().append('<ul style="padding-left:1em;"></ul>');</p>
<p>	key = $(obj).attr("key");</p>
<p>	var list = "";</p>
	var info = "";</p>
<p>	href = "/wc/watcher/context"+"?cp="+cp+"&port="+port+"&host="+host+"&key="+key;</p>
<p>	$.ajax({</p>
  <p>      url : href,</p>
  <p>      dataType : "json",</p>
  <p>  }).done(function(data){</p>
  <p>  	// info += '<ul>';</ul>
  <p>  	console.log(data);</p>
  <p>  	for (var x in data[1]["keys"])
    		list += '<li><span onclick="refresh(this)" key="'+key+'/'+data[1]["keys"][x]+'"><a href="javascript:void(0);"><p>'+data[1]["keys"][x]+'</a></span></li>';</p>
<p>
  <p>  	for (var y in data[0]["values"])</p>
  <p>  		info += '<tr><td>'+y+':</td><td>'+data[0]["values"][y]+'</td></tr>';</p>
   <p> 	console.log(info);</p>
<p></p>
  <p>  	$(obj).parent().children("ul").html(list);</p>
  <p>  	$(".watcher-info tbody").html(info);</p>
  <p>  	console.log(list);</p>
 <p>   });</p>
<p>}</p>



<p>@login_check</p>
<p>def watcher_context(request):</p>
<p>	GET = request.GET</p>
<p>	cp_type = int(GET["cp"])</p>
<p>	cp_port = int(GET["port"])</p>
<p>	cp_host = GET["host"]</p>
<p>	cp_key  = GET["key"]</p>
<p>	print(cp_key)</p>
<p>	watcher = Watcher.Watcher(cp_type)</p>
<p>	watcher.clearWatchData()</p>
<p>	watcher.connect(cp_host,cp_port)</p>
<p>	watcher.requireQueryWatcher(cp_key)</p>
<p></p>
<p>	if cp_key == 'root/network/messages':</p>
<p>		for x in range(1,3):</p>
<p>			time.sleep(2)</p>
<p>			watcher.processOne()</p>
<p>			values = watcher.watchData</p>
<p>	else:</p>
<p>		time.sleep(0.5)</p>
<p>		watcher.processOne()</p>
<p>		values = watcher.watchData</p>
<p>	print(values)</p>
<p>	return HttpResponse(json.dumps(values),content_type="application/json")</p>


</div>
