---
title: Handling Errors using Select2's Custom Query Function
layout: post
tags:
  - programming
  - javascript
  - jquery
  - backbone.js
  - select2
  - ajax
---

Recently I encountered an issue using [Select2][sel2]'s custom `query` feature to create a custom function to use
[Backbone.js][bbjs] collection objects to handle fetching and parsing data to populate a dynamic search box.  One
problem that arose from that was the need to properly handle any variety of errors that could occur during that AJAX
request.  Select 2 handles this natively when using the built-in `ajax` option, but doing it with a custom `query`
is not well documented.  Consider this example:

{% highlight javascript %}
var collection = new MyBackboneCollection()

$('#myInput').select2({
  query: function(options) {
    collection.fetch().done(function() {
      var data = { results: [] };
      data.results = collection.map(function(obj) {
        return {
          id: obj.id,
          text: obj.get('Name')
        };
      });
      options.callback(data);
    }).fail(function() {
      var data = { results: [] };
      options.callback(data);
    });
  },
});
{% endhighlight %}

In this smple use case, we're capturing any failures and presenting it as if no results were found, at a minimum,
this is misleading to the user, and at worst, could be confusing if a user is expecting a result to be present.

There is nothing mentioned in the documentation, however after looking at the source for the Select2, we can see
that in the built-in implementation of the AJAX handler, the way the native error handling is triggered:

{% highlight javascript %}
$.extend(params, {
  url: url,
  dataType: options.dataType,
  data: data,
  success: function (data) {
      // TODO - replace query.page with query so users have access to term, page, etc.
      // added query as third paramter to keep backwards compatibility
      var results = options.results(data, query.page, query);
      query.callback(results);
  },
  error: function(jqXHR, textStatus, errorThrown){
      var results = {
          hasError: true,
          jqXHR: jqXHR,
          textStatus: textStatus,
          errorThrown: errorThrown,
      };

      query.callback(results);
  }
});
{% endhighlight %}

The key component being `hasError: true`.  So if we change our original example:

{% highlight javascript %}
var collection = new MyBackboneCollection()

$('#myInput').select2({
  query: function(options) {
    collection.fetch().done(function() {
      var data = { results: [] };
      data.results = collection.map(function(obj) {
        return {
          id: obj.id,
          text: obj.get('Name')
        };
      });
      options.callback(data);
    }).fail(function() {
      var data = { hasError: true };
      options.callback(data);
    });
  },
});
{% endhighlight %}

The native error handling kicks in and we can easily imform the user that there was an error loading their search data!
From here we can use additional parameters in the results object and the native `formatAjaxError` to do anything we
might want to do to display a nicely formatted error.

[sel2]: https://ivaynberg.github.io/select2/
[bbjs]: http://backbonejs.org/
