---
layout: default
---

<div id="flashy-big-logo"></div>

<section id="intro"><div markdown="1">

# Designed for Things to Come

Quinn was created to address the [utter](http://expressjs.com/) [lack](http://hapijs.com/) [of](http://locomotivejs.org/guide/) [small](http://mcavage.me/node-restify/), [elegant](https://www.totaljs.com/) [web](http://locomotivejs.org/guide/) [frameworks](http://sailsjs.org/#!/) in the node ecosystem.
It embraces new and upcoming ECMAScript features and is mostly a playground to explore how those features could affect framework design.

Quinn is not opinionated when it comes to flow control.
It supports raw promises.
It supports generators via [`Bluebird`](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisecoroutinegeneratorfunction-generatorfunction---function) & [`co`](https://www.npmjs.com/package/co).
But of course you'd want to use async/await via [`babel`](https://babeljs.io/)
combined with `Bluebird`'s [collection utility functions](https://github.com/petkaantonov/bluebird/blob/master/API.md#collections).[^1]
The code examples on this page favor using async/await but any of the others would work just the same.


</div></section>
<section id="intro"><div markdown="1">

# Hello, World!

{% highlight js %}
import { createServer } from 'http';
import { createApp, respond } from 'quinn';

const app = createApp(() => respond({ body: 'Hello World' }));

createServer(app).listen(3000);
{% endhighlight %}

[^1]: If you need access to legacy packages or node core modules, there's always [`thenify`](https://www.npmjs.com/package/thenify) and [friends](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisification).

</div></section>
