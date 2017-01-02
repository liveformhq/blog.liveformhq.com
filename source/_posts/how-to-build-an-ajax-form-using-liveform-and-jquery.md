---
title: How to build an ajax form using LiveForm and jQuery
date: 2017-01-01 23:14:43
tags:
- Ajax Form
- jQuery
- Ajax
---

In this blog post we'll see how we can use LiveForm and jQuery to build an Ajax form.

Take this form for example, it has a name, email and a message field.

```html
<form id="contact-form" method="post" action="https://liveformhq.com/form/27fb4351-d2f7-45d3-96ac-8dae0eda2d26">
    <input id="name" name="name" type="text" />
    <br />
    <input id="email" name="email" type="email" />
    <br />
    <textarea id="message" name="message" rows=8 cols=40></textarea>
    <br />
    <button id="submit" type="submit">Send Message</button>
</form>
```

By default this form will make a full page post request to the server. We can use jQuery to
make it do the submission via AJAX. All we need to do, is include jQuery (a CDN version can be found at https://code.jquery.com/)
And intercept the form submission and make an AJAX post instead of the default. Here is some code which does that:

```javascript
$(function(){
  // intercept the form submission
  $("#contact-form").on("submit", function(e) {
      e.preventDefault(); // stop the form from being submitted

      // make an ajax POST request to the form's action
      $.ajax({
          type: "POST",
          url: $(this).attr("action"),      // use the form's action attribute as the endpoint
          data: $(this).serialize(),        // use the data from the form
          headers:
          {
              "Accept": "application/json"  // this makes the server send you a JSON response
          },
          success: function(response)       // handle the successful submission of your POST
          {
              console.log(response);        // response contains the form submission that was just made
              alert("Thank you for your submission, we'll get back to you soon :)");
              $("#contact-form")[0].reset();// reset the form
          },
      });
  });
})
```

You can check out a full working demo at http://demos.liveformhq.com/ajax-form/
