---
layout: post
title:  "Taking Pictures with Meteor"
date:   2015-03-23 19:01:00
categories: meteor camera cordova
---

Meteor is a very simple javascript framework that allow us to build amazing applications with a minimal effort. In this post I'll teach you how to access the camera of your desktop or smartphone using the [Meteor Camera Package](https://github.com/meteor/mobile-packages/tree/master/packages/mdg:camera).

First, lets create our app:
{% highlight bash %}
$ meteor create meteorgram
{% endhighlight %}


And before we go any further, let's jump to our project folder and add the camera package:
{% highlight bash %}
$ cd meteorgram
$ meteor add mdg:camera
{% endhighlight %}


All right. Now, let's create a folder of our feature to keep the project organized. Create a folder called `camera_button` and  then, add a `camera_button.html` and `camera_button.js` files into her.

Your project tree should be like this right now:
{% highlight bash %}
├── camera_button
|   ├── camera_button.html
|   └── camera_button.js
├── meteorgram.css
├── meteorgram.html
├── meteorgram.js
├── .meteor
{% endhighlight %}


Now let's define our template that will be our button and the javascript that will handle his events.

{% highlight html %}
<!-- camera_button.html -->
<template name="camera_button">
  <button id="camera-button">Take a Picture!</button>
</template>
{% endhighlight %}

{% highlight javascript %}
// camera_button.js
if (Meteor.isClient){
  Template.camera_button.events({
    'click #camera-button': function(){
      // Define the camera settings
      var options = {
        width: 640,
        height: 640,
        quality: 100
      };
      // Call the camera API
      MeteorCamera.getPicture(options, function(error, data){
        if (error){
          // Handle the error as you wish
          console.log(error);
        }else{
          // Store the a base64-encoded data URI for the image
          // taken by the camera
          Session.set('picture', data);
        }
      });
    }
  });
}
{% endhighlight %}

We have implemented our feature already, but now we need to call
the `template` into our main HTML file.
{% highlight html %}
{% raw %}
<!-- meteorgram.html -->
<head>
  <title>Meteorgram</title>
</head>
<body>
  <h1>Welcome to Meteorgram!</h1>
  {{> camera_button}}
</body>
{% endraw %}
{% endhighlight %}

Test yourself and see what is happening. You'll notice that we are
not showing the pictures that we are taking from the camera. So, we need to add a [`helper`](http://docs.meteor.com/#/full/template_helpers) into our `meteorgram.js` file to handle this. Remember we store the `picture` key in the [`Session`](http://docs.meteor.com/#/full/session) and set his value as the data that came from the camera. Let's use this data.

{% highlight javascript %}
// meteorgram.js
if (Meteor.isClient){
  Template.body.helpers({
    picture: function(){
      return Session.get('picture');
    }
  });
}
{% endhighlight %}

Now we need to call our helper into the `meteorgram.html`. Override the `<body>` tag by the code below.

{% highlight html %}
{% raw %}
<!-- meteorgram.html -->
<body>
  <h1>Welcome to Meteorgram!</h1>
  {{> camera_button}}
  <div id="picture">
    <img src={{picture}}>
  </div>
</body>
{% endraw %}
{% endhighlight %}

As you see, our picture data can be used directly in the `src` attribute of an image tag. This is supported by all browsers that has methods for embedding images and other files as strings. You can check [here](http://caniuse.com/#search=Data%20URI) the browsers support.

You can see the [Demo](http://meteorgramdemo.meteor.com/) to check it how its looks like. I uploaded the code on github and you can visit the [repository](https://github.com/lnmunhoz/meteorgram) for more details.

Hope you enjoy!
