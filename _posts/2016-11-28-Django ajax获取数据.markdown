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
<textarea>
function refresh(obj)
{
	console.log($(obj).attr("key"));
	if ($(obj).parent().children("ul").text() =="" ||$(obj).parent().children("ul").text() == null)
		$(obj).parent().append('<ul style="padding-left:1em;"></ul>');
	key = $(obj).attr("key");
	var list = "";
	var info = "";
	href = "/wc/watcher/context"+"?cp="+cp+"&port="+port+"&host="+host+"&key="+key;
	$.ajax({
        url : href,
        dataType : "json",
    }).done(function(data){
    	// info += '<ul>';</ul>
    	console.log(data);
    	for (var x in data[1]["keys"])
    		list += '<li><span onclick="refresh(this)" key="'+key+'/'+data[1]["keys"][x]+'"><a href="javascript:void(0);">'+data[1]["keys"][x]+'</a></span></li>';
   	for (var y in data[0]["values"])
    		info += '<tr><td>'+y+':</td><td>'+data[0]["values"][y]+'</td></tr>';
    	console.log(info);

    	$(obj).parent().children("ul").html(list);
    	$(".watcher-info tbody").html(info);
    	console.log(list);
    });
}

</textarea>
<textarea>
@login_check
def watcher_context(request):
	GET = request.GET
	cp_type = int(GET["cp"])
	cp_port = int(GET["port"])
	cp_host = GET["host"]
	cp_key  = GET["key"]
	print(cp_key)
	watcher = Watcher.Watcher(cp_type)
	watcher.clearWatchData()
	watcher.connect(cp_host,cp_port)
	watcher.requireQueryWatcher(cp_key)

	if cp_key == 'root/network/messages':
		for x in range(1,3):
			time.sleep(2)
			watcher.processOne()
			values = watcher.watchData
	else:
		time.sleep(0.5)
		watcher.processOne()
		values = watcher.watchData
	print(values)
	return HttpResponse(json.dumps(values),content_type="application/json")

</textarea>
</div>
