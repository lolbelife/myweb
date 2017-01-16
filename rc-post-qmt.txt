var monthNames = new Array("01","02","03","04","05","06","07","08","09" ,"10","11","12");
var now = new Date();
var d0 = now.getDate();
var m0 = monthNames[now.getMonth()];
var y0 = now.getYear();
if(y0 < 1900) {y0 += 1900}; // corrections if Y2K display problem

function removeHtmlTag(strx,chop){
	var s = strx.split("<");
	for(var i=0;i<s.length;i++){
		if(s[i].indexOf(">")!=-1){
			s[i] = s[i].substring(s[i].indexOf(">")+1,s[i].length);
		}
	}
	s =  s.join("");
	s = s.substring(0,chop-1);
	return s;
}

function showrecentposts(json) {
	img  = new Array();

              for (var i = 0; i < numposts; i++) {
    	var entry = json.feed.entry[i];
    	var posttitle = entry.title.$t;
		var pcm;
    	var posturl;
    	if (i == json.feed.entry.length) break;
    	for (var k = 0; k < entry.link.length; k++) {
      		if (entry.link[k].rel == 'alternate') {
        		posturl = entry.link[k].href;
        		break;
      		}
    	}
		
		for (var k = 0; k < entry.link.length; k++) {
      		if (entry.link[k].rel == 'replies' && entry.link[k].type == 'text/html') {
        		pcm = entry.link[k].title.split(" ")[0];
        		break;
      		}
    	}
		
    	if ("content" in entry) {
      		var postcontent = entry.content.$t;}
    	else
    	if ("summary" in entry) {
      		var postcontent = entry.summary.$t;}
    	else var postcontent = "";
  		
	s = postcontent	; a = s.indexOf("<img"); b = s.indexOf("src=\"",a); c = s.indexOf("\"",b+5); d = s.substr(b+5,c-b-5);

	if((a!=-1)&&(b!=-1)&&(c!=-1)&&(d!="")) {img[i] = d;} else {img[i]="http://1.bp.blogspot.com/_u4gySN2ZgqE/SosvnavWq0I/AAAAAAAAArk/yL95WlyTqr0/s400/noimage.png";}
	
    var postdate = entry.published.$t;
	var d = postdate.split("-")[2].substring(0,2);
	var m = postdate.split("-")[1];
	var y = postdate.split("-")[0];

      if ((d==d0)&&(m==m0)&&(y==y0)) {var newitem = '<img src="http://quantrimang.com.vn/images/icon_new1.jpg" border="0">';}
	  else {var newitem = '';}

    var tdpc1 = '<div class="Title_article"><a href="'+posturl+'">'+posttitle+'</a></div><div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="imgModule" valign="top"><a href="'+posturl+'"><img src="'+img[i]+'" hspace="3" vspace="3" align="left" border="0"></a>'+removeHtmlTag(postcontent,sumPosts)+' ...</td></tr></tbody></table></div>'; 
    var tdpc2 = '<li><img src="http://quantrimang.com.vn/Styles/images/icon.jpg" align="absmiddle">&nbsp;<a href="'+posturl+'">'+posttitle+' '+newitem+' </a> </li>'; 
				 
                 if ((i>=0)&&(i < list1)) { var tdpc = tdpc1; }
                 if (i==list1) { var tdpc = '<div class="Tin_lienquan"><ul>'+tdpc2;}
                 if ((i > list1)&&(i<numposts-1)) { var tdpc = tdpc2;}
                 if (i==numposts-1) { var tdpc = tdpc2+'</ul></div>';}
				 
document.write(tdpc);
}
}
if (mode=="blog") {
	document.write("<script src=\""+home_page+"feeds/posts/default?max-results="+numposts+"&orderby=published&alt=json-in-script&callback=showrecentposts\"><\/script>");
}
else if (mode=="label") {
	document.write("<script src=\""+home_page+"feeds/posts/default/-/"+label+"?max-results="+numposts+"&orderby=published&alt=json-in-script&callback=showrecentposts\"><\/script>");
}

