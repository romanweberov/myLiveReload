myLiveReload
============

Reload page when editing html, css, js, etc., without using remote or local server.

Working in Firefox browser.

```js
window.onload = function(){
  var files = [document.URL, 'css/style.css', 'js/scripts.js'];
	
  //scroll position
  if(localStorage["change-reload"]){
    localStorage["change-reload"] = false;
    document.documentElement.scrollTop = localStorage["scrolltop"];
  }

  var params = {};
  for(var i=0; i<files.length; i++){
    params[i] = {};
    params[i].oldDate = null;
    params[i].newDate = null;
		
    initFile(i);
  }
	
  function initFile(i){
    setInterval(function(){
      var xmlhttp = new XMLHttpRequest();

      xmlhttp.open("HEAD", files[i] ,true);
      xmlhttp.onreadystatechange = function(){
        if(xmlhttp.responseXML && xmlhttp.readyState==4){
          if(params[i].oldDate == null){
            params[i].oldDate = new Date(xmlhttp.responseXML.lastModified).getTime();
          } else {
            params[i].newDate = new Date(xmlhttp.responseXML.lastModified).getTime();

            if(params[i].newDate > params[i].oldDate){
              params[i].oldDate = params[i].newDate;
              localStorage.setItem("scrolltop", document.documentElement.scrollTop);
              localStorage.setItem("change-reload", true);
              document.location.reload(true);
            }
          }
        }
      }
      xmlhttp.send(null);

    }, 600);
  }
}
```
