Add classes "ANCHOR_partial" first and then "page_name" to a div in your main index.html.
Create a views folder with page_name.html containing the content of your div.

in an index.js controller, on $(document)ready:
	$.get("../views/page_name.html", function(data){
		$("div.page_name").html(data);

  //then initialize the code of the page route (otherwise the code will load before the partial):
  initializePageName();
  })

$(document).on("ANCHOR, function(){
  pages();
})

function pages(){
  //ANCHOR.page() returns page_name without the #
  switch(ANCHOR.page(){
    case "page_name":
      initializePageName();
      break;
  }

}

To manually route to a page, use ANCHOR.route("#page_name"), including the #

$(a.page_name).click(function(){
  ANCHOR.route("#page_name");
})

Requires JQuery.
