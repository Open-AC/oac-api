---
icon: trowel-bricks
coverY: 0
---

# API Basics

Before diving into the basics, it is important to know what is an API.

An API, or Application Programming Interface, is like a friendly bridge that lets different software programs talk to each other and share data.

Think of it like this: if you have a weather app on your phone, the app uses an API to get the latest weather updates from a weather service. So when you check the app, it asks the weather service’s API for current conditions, and the API sends back the information so you can see whether it’s sunny or rainy outside.

It's a simple way to get the info you need without having to gather it all yourself!

***

Now that we know what an API is, we can dive straight into how it works. Currently, OpenAC implements its API through the `ModuleScript` named "OpenAC" in `ServerScriptService`.

To utilize the API, one needs to call the `require` function from Luau, like how one would use a `ModuleScript`. It will return, in this case, a table containing the Methods and Signals of the OpenAC API.

<pre class="language-lua"><code class="lang-lua"><strong><a data-footnote-ref href="#user-content-fn-1">local OpenAC</a> = <a data-footnote-ref href="#user-content-fn-2">require(game:GetService("ServerScriptService").OpenAC)</a>
</strong></code></pre>

As I said earlier, requiring the OpenAC module will return a table. The table structure looks like this:

```lua
{
    Methods = {"...Methods related to OpenAC..."},
    Signals = {"...Signals related to OpenAC..."},
}
```

The methods by default all take `self` for readability reasons, which means they must be called like this:

```lua
--// Example of calling the `OpenAC::Crash` method of the API
OpenAC.Methods:Crash(SomePlayer)

--// Calling without self will work, but we don't recommend it.
OpenAC.Methods.Crash(SomePlayers)
```

[^1]: We of course need to save the result of the `require` function in order to use the API properly.

[^2]: Calls the `require` function with the path to the OpenAC `ModuleScript`.
