1) Add classes "ANCHOR_partial" first and then "page_name" to a div in your main index.html.

2) Create a views folder with page_name.html containing the content of your div.

3) In controllers/index.js, load your partial:
```
$(document).ready(function(){
	$.get("../views/page_name.html", function(data){
		$("div.page_name").html(data);

		//optional, route to home_page_name if anchor is undefined (no # in URL), preserve params on refresh, route to page_name on refresh
		if(ANCHOR.page() && !$.isEmptyObject(ANCHOR.getParams())){
			ANCHOR.route("#" + ANCHOR.page() + "?" + ANCHOR.getParamsString());
		}
		else if(ANCHOR.page()){
			ANCHOR.route("#" + ANCHOR.page())
		}
		else{
			ANCHOR.route("#home_page_name")
		}
	})
})
```

4) In index.js initialize page_name partial on route:
```
$(document).on("ANCHOR", function(){
  pages();
})
```

4) In index.js include partial initialization function called on each route thru "ANCHOR" event:
```
function pages(){
  //ANCHOR.page() returns page_name without the #
  switch(ANCHOR.page(){
    case "page_name":
      initializePageName();
      break;
  }

}
```

5) In controllers/page_name.js, load data from server on partial page_name init (this is called from pages()):
```
function initializePageName(){
	$.get("/page_name", 
	//optional if params
	{
		foo : ANCHOR.getParams() && ANCHOR.getParams().foo ? ANCHOR.getParams().foo : ""
	}, function(data){
		$("div.page_name").html(data);
	})
}
```

ANCHOR.setParams("key", "value") works to some extent, but raise an issue if you'd like me to fix it for you.

To manually route to a page, use 
```
ANCHOR.route("#page_name");
ANCHOR.route("#page_name?param=value&param2=value")
```
including the # before page_name partial name
```

To route on hyperlink click, use:
$(a.page_name).click(function(){
  ANCHOR.route("#page_name");
})
```

Requires JQuery. 

Can be used as a lightweight alternative to Angular or React to Route Single Page Applications using #anchors
