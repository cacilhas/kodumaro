---
type: page
title: Kodumaro :: Saluton!
date: 2020-09-23
---

{:class="mg-first"} Hi there!

Kodumaro is a colaboration between three friend programmers:
<a href="{{{ mosaicolivre.url }}}">{{ mosaicolivre.author }}</a>,
<a href="{{{ montegasppa.url }}}">{{ montegasppa.author }}</a>,
and <a href="{{{ waltercruz.url }}}">{{ waltercruz.author }}</a>,
in an effort to offer you some interesting tips about programming.

### Posts

<ul id="postsList">
  <noscript>Please enable Javascript to view the posts.</noscript>
</ul>

### Tags

<ul id="tagsList">
  <noscript>Please enable Javascript to view the posts.</noscript>
</ul>

### Previous posts

You can find older posts in the [legacy page](/legacy.html).

<script>
  var urlParams = new URLSearchParams(window.location.search)
  var currentTag = urlParams.get('tag')

  if (currentTag) {
    $('#posts').text('Posts at ')
    $('#posts').append('<code>' + currentTag + '</code>')
    $('#postsList').append('<li><a href="/">Back home</a></li>')
    $.getJSON('/tags/' + currentTag + '.json', function(posts) {
      for (var post of posts) {
        $('#postsList').append(
          '<li><small>[' + post.date + ']</small> <a href="' + post.url + '">' + post.title + '</a></li>'
        )
      }
    })

  } else {
    $('#posts').text('Latest posts')
    $.getJSON('/posts.json', function(posts) {
      for (var post of posts) {
        $('#postsList').append(
          '<li><small>[' + post.date + ']</small> <a href="' + post.url + '">' + post.title + '</a></li>'
        )
      }
    })
  }

  $.getJSON('/tags.json', function(tags) {
    for (var tag of tags) {
      $('#tagsList').append('<li><a href="/?tag=' + tag + '"><code>' + tag + '</code></a></li>')
    }
  })
</script>
