---
layout: default
---

<div id="flashy-big-logo">
  <img src="/quinn-logo.png" title="Huge logo to hide any actual information" />
</div>

<section id="intro"><div markdown="1">

## Designed for Things to Come

Quinn was created to address the
[utter](http://expressjs.com/)
[lack](http://hapijs.com/)
[of](http://locomotivejs.org/guide/)
[small](http://mcavage.me/node-restify/),
[elegant](https://www.totaljs.com/)
[web](http://locomotivejs.org/guide/)
[frameworks](http://sailsjs.org/#!/)
in the node ecosystem.
It achieves breathtaking performance,
made possible by the almost complete absence of features.
It embraces new and upcoming ECMAScript features
and acts as a playground to explore how those features could affect framework design.

Quinn is not opinionated when it comes to flow control.
It supports raw promises,
generators via
[`Bluebird`](https://github.com/petkaantonov/bluebird/blob/master/API.md#generators)
&
[`co`](https://www.npmjs.com/package/co),
and of course async/await via [`babel`](https://babeljs.io/).[^1]
The code examples on this page favor using async/await
but any of the other flavors would work just as well.


### More Resources

* API docs for Quinn
* The Quinn Cookbook
* [Wegweiser](https://github.com/quinnjs/wegweiser/): A router with support for [decorators](https://github.com/wycats/javascript-decorators), built for Quinn


</div></section>
<section id="intro"><div markdown="1">

## Hello, World!

This is how a traditional node.js solution to "Hello, World!" might look like:

{% highlight js %}
createServer(
  (req, res) => res.end('Hello, World!')
).listen(3000);
{% endhighlight %}

In summary:
request dispatching is defined using a function that takes a request and a response.
This function has no meaningful return value.
There's no notion of the function being "done" processing the request,
other than the call to `.end`.
This forced many node developers to rely on hacks like patching `.end` for things as fundamental as logging response codes.

Quinn makes one small but important tweak to this model:
Instead of taking the response as a second argument,
the dispatch function is expected to *return* the response:

{% highlight js %}
createServer(
  quinn(req => respond({ body: 'Hello World!' }))
).listen(3000);
{% endhighlight %}

This gains us a couple of helpful properties.
The moment the function returns,
both the status code and all headers are known.
They can safely be written to the wire or a log file
because it is impossible for anybody to change them from there on.
Errors during request handling can bubble up naturally
and be attributed to a request without the need to track continuation contexts
or resorting to best effort guesses.

Another convenient consequence is that dispatch functions suddenly compose quite nicely
and there's no need for special middleware mechanics anymore.
A "middleware" for Quinn can be as simple as this:

{% highlight js %}
function requestLogger(next) {
  return async req => {
    console.log('<- %s %s', req.method, req.url);
    const res = await next(req);
    console.log('-> %s %s', res.statusCode, req.url);
    return res;
  };
}
{% endhighlight %}

And using it in the app from above is as easy as calling a function:

{% highlight js %}
createServer(
  quinn(
    requestLogger(req => respond({ body: 'Hello World!' }))
  )
).listen(3000);
{% endhighlight %}

Since there's nothing more going on than functions calling other functions,
there's less need for libraries with "Quinn support".
You can just keep using [`lodash`](https://lodash.com/docs#flow):

{% highlight js %}
const middleware = [ requestLogger /*, ...more middleware */ ];
const createApp = _.compose(createServer, quinn, ...middleware);
createApp(req => respond({ body: 'Hello World!' })).listen(3000);
{% endhighlight %}

You can find more examples like this in the Quinn Cookbook.
Or if you are interested in the exact details of `respond` and `createApp`,
check out Quinn's API docs.


[^1]: If you need access to legacy packages or node core modules, there's always [`thenify`](https://www.npmjs.com/package/thenify) and [friends](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisification).

</div></section>
