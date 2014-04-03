# JavaScript prismic.io single-page framework

The JavaScript prismic.io single-page framework allows you to make a website with manageable content, using nothing but HTML.

## Getting started

Fork and clone this repository. Open `index.html` in your browser, it works!

To use in your own project, create your content repository on [prismic.io](https://prismic.io), and create your content (read the [Getting started section](https://developers.prismic.io/documentation/UjBaQsuvzdIHvE4D/getting-started) of prismic.io's documentation).

Tu use your content within any template, copy-paste the JavaScript files in `/js`, and include them in your HTML. Then, add this in the `<head>` section of your HTML page, replacing `your-repository-id` by your prismic.io repository ID, and setting the right OAuth client ID if you need it:

```html
<meta name="prismic-api" content="https://your-repository-id.prismic.io/api">
<meta name="prismic-oauth-client-id" content="UzJVxQEAANoLZ3N0">
<script src="js/ejs-1.0.js"></script>
<script src="js/prismic.io-1.0.9.js"></script>
<script src="js/prismic.io-singlepage-1.0.0.js"></script>
```

## Writing content queries

Content query predicates look like this:

```
<script type="text/prismic-query" data-binding="clients">
  [
    [:d = at(document.type, "client")]
  ]
</script>
```

Above, the predicate is executed, and the resulting list of documents is stored in the `clients` variable.

Read [prismic.io's documentation about how to write predicates](https://developers.prismic.io/documentation/UjBe8bGIJ3EKtgBZ/api-documentation#predicate-based-queries).

## Templating

The EJS templating language is used here, you can [learn more about it here](https://github.com/visionmedia/ejs), and you can [learn more about how to use the prismic.io documents here](https://developers.prismic.io/documentation/UjBe8bGIJ3EKtgBZ/api-documentation#kits-and-helpers).

Overall it's very simple; for instance, displaying a text fragment looks like this:

```
[%= copy.getText('copy.company_name') %]
```

And looping through a list of documents looks like this:

```
[% services.forEach(function(service) { %]
  <li>[%= service.getText('service.title') %]</li>
[% } %]
```
Read our kits' documentation to [learn more about how to manipulate other types of fragments](https://developers.prismic.io/documentation/UjBe8bGIJ3EKtgBZ/api-documentation#kits-and-helpers).


## Extra features

### Safe images

You will get an error in your JavaScript console if you write something like this, even though everything seems fine on your page: `<img src="[%= client.getImageView('client.logo', 'main').url %]">`. This is because your browser tries to find the image before EJS translated your code (which doesn't work out too well), and then once when the code is translated (which finally works). To prevent this, you can write it like this:

```
<img data-src="[%= client.getImageView('client.logo', 'main').url %]">
```

The prismic.io single-page framework will move your `data-src` attribute into a `src` attribute after EJS has done its templating.

### On load JS operations

Sometimes, you need some other JS code to start after the templating is rendered, and your content is ready. In which case, you need to execute these operations on the "prismic:rendered" event, and not when your browser tells you it's ready.

For instance, if you're using JQuery, you need to replace these kinds of calls:

```
$(document).ready(function(){
  // do something
});
```

```
$(function(){
  // do something
});
```

with this:

```
$(document).on('prismic:rendered', function(){
  // do something
});
```

## Deploying

Well, this is really easy: feel free to host the result on whatever hosting solution that handles static websites, such as [your Dropbox account](http://www.dropboxwiki.com/tips-and-tricks/host-websites-with-dropbox), or [GitHub Pages](https://pages.github.com/).

## Tutorial

Check out prismic.io's blog post "[A website with manageable content using a Bootstrap theme, client-side JavaScript and prismic.io](https://blog.prismic.io/UzIaLQEAAGMIZ3NE/a-website-with-manageable-content-using-a-bootstrap-theme-client-side-javascript-and-prismicio)", to see a complete use case from start to end.

## Licence

This software is licensed under the Apache 2 license, quoted below.

Copyright 2013 Zengularity (http://www.zengularity.com).

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this project except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0.

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
