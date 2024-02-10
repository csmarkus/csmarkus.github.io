---
layout: post
title:  "Here We Go Again"
date:   2024-02-10
comments: true
---

As happens every few years I remember I have a website/blog. And I'll make a few posts, maybe explaining something or posting something I did before it's forgotten again for another decade until I repeat the process. Did I ever tell you the definition of insanity?

![Insanity](/assets/images/insanity.png){: width="400" class="post-image" }

<blockquote>
    <p>Did I ever tell you what the definition of insanity is? Insanity is doing the exact... same fucking thing... over and over again expecting... shit to change... That. Is. Crazy.</p>
    <footer class="quote-footer">Vaas Montenegro, <cite title="Source Title">Far Cry 3</cite></footer>
</blockquote>

I'm sure you find yourself asking, "Why do you continue to do it?" Unfortunately I cannot give an answer for that at the moment. Much like Vaas pictured above I'm stuck in my own loop of insanit saying this time will be different. I do intend to try and break that cycle this time, however. This year I've started on a learning journey to improve myself as a software engineer.

Part of it is finally getting out of a funk I've been in for a while, and another is tackling my ADHD issues for the first time in a long while. Perhaps that is something I'll expand on in the future. Not something I'm ready to dump into the first post.

So with an update comes a new name. Attention Deficit Dev/Development.

I'm not sure what exactly I'm going to put on this new again site. Much like most of my personal projects it is a blank slate for me two hammer away at my keyboard and shape. 

Enough rambling about the past and how things will be different, though. That's the last time that'll happen (I can already hear Morgan Freeman as the Narrator saying "It was not the last time it happened"). On to more development related topics!

I've basically spent the last week in my late evenings getting this setup. Not wanting to install ruby and run this on my direct machine I took this as an opportunity to setup a Docker dev container with the environment to run Jekyll and test it locally. That's actually been a great learning experience with Docker as I've only used it sparsely in the last few years. I'm absolutely in love with have the environment setup in a container.

The html and css have been the roughest part. I have never claimed nor will I ever claim to be a great web designer. I enjoy designing the systems and how they interact. There was a time in the past where it was the opposite, but that was more designing the sites in photoshop than actually implementing them. I feel like this is probably my best work yet, though. I don't like it flashy. Simple and flat is my style, and I think I've done well. Still need to tweak some things for a more responsive design, but that's a problem for future me!

I want to try and include some code in each post, but this is more of an intro/beginning post. So I'm just going to add a random snippet.

Url62.cs
---

{% highlight csharp %}
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

        return string.Concat(Enumerable.Range(0, length)
            .Select(_ => alphabet[random.Next(alphabet.Length)]));
    }
}
{% endhighlight %}