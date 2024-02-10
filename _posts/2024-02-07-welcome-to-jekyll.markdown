---
layout: post
title:  "Welcome to Jekyll!"
date:   2024-02-07 23:33:06 +0000
comments: true
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight csharp linenos %}
using System.Text;

namespace CSRedirect.Core;

public static class Url62
{
    private const string Alphabet = 
        "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private const string AlphabetUnambigous = 
        "23456789abcdefghkmnpqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ";
    private static readonly Random random = new();

    public static string Generate(int length, bool unambiguous = false)
    {
        string alphabet = unambiguous ? AlphabetUnambigous : Alphabet;

        var sb = new StringBuilder(length);

        for (var i = 0; i < length; i++)
        {
            var c = alphabet[random.Next(alphabet.Length)];
            sb.Append(c);
        }

        return sb.ToString();
    }
}
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
