# Basic Gallery Demo Using JQuery and CONTENTdm IIIF APIs

## Create the HTML page
```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Manifest gallery demo</title>
</head>
<body>
<div id="images"></div>
<div id="photo_popup" class="modal">
<span class="close cursor">&times;</span>
<div class="modal-content">
    <div id="mySlide"></div>
    <div id="paging">
    </div>

    <div class="caption-container">
      <p id="caption"></p>
    </div>
</div>
</div>
</body>
</html>
```
## Add the appropriate JQuery libraries and Stylesheets

```
<link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/themes/smoothness/jquery-ui.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js"></script>
```

## Add the appropriate lightbox CSS
Download css 

```
<link rel="stylesheet" href="styles.css">
```

## Add Javascript to run on load
```
<script type="application/javascript">
$(document).ready(function(){
    // the rest of the javascript
});
</script>
```

## Download the manifest locally
Link kac_cmd_highlights.json

## Read a Manifest containing Gallery Information
1. Make an HTTP request for a manifest in JSON format
2. Parse the data from the manifest
3. Add to the HTML a header with the label for the manifest

``` 
   $.getJSON( "kac_cmd_highlights.json",
            function (data) {
                    $("#images").empty();
                        var html = '';
                        html += '<h2 style="text-align:center">' + data.label + '</h2>';
                        html += '<div class="row">';
                        // the images
                        html += '</div>';
                        $('#images').append(html);
                        
            });
```            
### Loop through the canvases and add them to the page in the image div
1. Add html to hold the image
2. Add a span with an id for the image service
3. Add the actual image using the image service and the IIIF Image API syntax
    a. Add alt text for the image with the canvas label

```
$.each(data.sequences[0].canvases, function(i,canvas){
    var numbertext = i + 1
    html += '<div class="column">';
    html += '<span id="' + canvas.images[0].resource.service['@id'] + '"><img src="' + canvas.images[0].resource.service['@id'] + '/full/300,/0/default.jpg" alt="' + canvas.label + '" class="hover-shadow-cursor" id="' + numbertext + '"/></span>';
    html += '</div>';
});

```

## Lightbox the Images
1. Create an "on click" event tied to the ".hover-shadow-cursor" class
2. Get the id of the item in the gallery which is the image number
3. Get the id of the span around the image clicked which contain the uri for the image service for the image clicked 
4. Get the alt attribute of the image clicked and use this as the caption for the lightbox
5. Toggle the CSS to display the image
6. Add the image to the page
7. Add the image caption to the page

```
$(document.body).on('click', '.hover-shadow-cursor', function(event) {
         
         event.preventDefault();
         // create a lightbox
         var id = $(this).attr('id');
         var img = '<div class="numbertext">' + id + ' / 4</div>';
         img += '<img src="' + $(this).parent().attr('id') + '/full/1000,/0/default.jpg" style="width:100%"/>';
         var caption = $(this).attr('alt');
         
         $('#photo_popup').css("display", "block");
         $('#mySlide').css("display", "block");
         $('#mySlide').append(img);
         
         $('#caption').append(caption);
     });
```
     
## Create function to close the Lightbox
1. Create an "on click" event tied to the ".close" class
2. Toggle the CSS to not display images
3. Remove the image and caption from the HTML

```
$(document.body).on('click', '.close', function(event) {
    $('#photo_popup').css("display", "none");
    $("#mySlide").empty();
    $("#caption").empty();
});
```