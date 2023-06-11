---
type: page
title: Kodumaro :: Saluton!
date: 2020-09-23
---
[legacy page]: /legacy.html
[montegasppa]: {{{montegasppa.url}}}
[mosaicolivre]: {{{mosaicolivre.url}}}
[waltercruz]: {{{waltercruz.url}}}

:first Hi there!

Kodumaro is a colaborative effort of three fiend[¹](#pun) programmers,
[{{ mosaicolivre.author }}][mosaicolivre],
[{{ montegasppa.author }}][montegasppa], and
[{{ waltercruz.author }}][waltercruz], to offer you some interesting tips about
programming.

{:id="pun" :class="small"} ¹I like the pun.

### Posts

<ul id="postsList">
  <noscript>Please enable Javascript to view the posts.</noscript>
</ul>

### Tags

<ul id="tagsList">
  <noscript>Please enable Javascript to view the posts.</noscript>
</ul>

### Previous posts

You can find older posts in the [legacy page][].

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
      $('#tagsList').append('<li><a href="/?tag=' + tag + '"><code>' + tag.replace(/-/g, ' ') + '</code></a></li>')
    }
  })
</script>
