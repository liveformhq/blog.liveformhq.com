---
title: Creating an Imgur clone using Github Pages and Liveform
date: 2016-11-27 18:24:54
tags:
- Image uploads
- Imgur clone
- API
---

[Yesterday, we released the ability to upload files using Liveform](http://blog.liveformhq.com/2016/11/26/file-uploads-for-your-forms/) Today, we released readonly API access to messages. These two features allow us to build simple and powerful tools with minimal work. In this blog post, we'll build a clone of [Imgur](http://imgur.com/), the popular image hosting website. We'll do this using Github Pages and Liveform. Let's get started!

  1. Create a new form called *Imgur Form* in Liveform.
{% asset_img imgur1.png Create Imgur Form %}
  2. Create a new repository in Github, let's name it *imgur_clone*
  {% asset_img imgur2.png Create Github repository %}
  3. Now let's create a branch called *gh-pages* on Github. This will hold our final website.
  4. Let us now create our home page `index.html` at the root of our repository. This is a simple stub page, with 2 links and a placeholder for our images
```html
<!doctype html>

<a href='/'>Imgur</a> | <a href='/new.html'>New Upload</a>

<br />


Show all images....
```

  5. Now, let us create a file called `new.html` which will hold the form for our uploads
```html
<!doctype html>

<a href='/'>Imgur</a> | <a href='/new.html'>New Upload</a>

<br />

<form action="https://liveformhq.com/form/369c5f72-7940-4d20-b3c7-8a963fc49a15" method="POST" accept-charset="utf-8"  enctype="multipart/form-data">
  <input type="hidden" name="utf8" value="✓">

  <input type="hidden" value="https://minhajuddin.github.io/imgur_clone/" name="_redirect" />

  <label for="title">Title</label>
  <input type="text" id="title" name="title"> <br />

  <label for="upload">Upload</label>
  <input type="file" id="upload" name="upload"> <br />

  <button type="submit">Upload</button>
</form>

```
	A few things to note here
		a. We copied the form from the Liveform setup page
    {% asset_img imgur4.png Liveform setup starter code%}
		b. We added an extra attribute to the form: `enctype="multipart/form-data"`
		c. We added two input fields, one for the title and another of type `file` for the file upload
		d. We set a hidden field called `__redirect` to redirect the user back to the home page

  6. The upload form should be all setup now. Let's flesh out our home page. We want the user to be able to view all the uploads as soon as he visits the home page. We can easily do this using the Liveform API.
  {% asset_img imgur5.png Liveform ajax starter code %}
		The code below is straightforward.
    a. We are fetching our messages using the Liveform api.
    b. With these messages we are building some html for each post
    c. We are also building the pagination footer using the `meta` info in the response
    d. Finally we are sticking this into the `#container` div.
```html
<!doctype html>

<a href='/'>Imgur</a> | <a href='/new.html'>New Upload</a>

<h2>All Images</h2>

<div id="container">
  Loading...
</div>

<script src="https://code.jquery.com/jquery-3.1.1.min.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript" charset="utf-8">
  $(function(){
    $.get("https://api.liveformhq.com/v1/forms/369c5f72-7940-4d20-b3c7-8a963fc49a15/messages", {
        api_key: "11d43d92-d928-4a8b-b55c-50f98227c794"
      },
      function(response) {
        var body = "",
          messages = response.messages;
        for(var i=0; i < messages.length; i++){
          body += imageTemplate(messages[i], i + 1)
        }

        body += pagnationTemplate(response);

        $("#container").html(body)
      })
  })

  function pagnationTemplate(response){
    var meta = response.meta;
    return  "<div>" +
      "<h4> Showing " + response.messages.length  + " records out of " + meta.total_count + ", page " + meta.current_page + " of " + meta.total_pages + " </h4>" +
      "<div>"+(meta.next_link ? "<a href='"+meta.next_link+"'>Next</a>" : "" ) + "|" + (meta.prev_link ? "<a href='"+meta.prev_link+"'>Prev</a>" : "")  +"</div>" +
            "</div>";
  }

  function imageTemplate(message, id){
    if (!message.data.upload) {
      return "";
    }

    var title = (message.data.title || "Untitled");

    return "<div class='post'>" +
        "<h2>" + id + "." + title + "</h2>" +
        "<div class=image><img alt='" + title + "' style='max-width: 100%;' src='" + message.data.upload + "' /></div>" +
        "</div>"
  }
</script>
```


** Here is the final screenshot of the website **
{% asset_img imgur6.png Imgur clone screenshot %}

Obviously this is not the same as Imgur :) But you get the idea ;)

Oh, and here is a link to the final Github Pages website: https://minhajuddin.github.io/imgur_clone/
