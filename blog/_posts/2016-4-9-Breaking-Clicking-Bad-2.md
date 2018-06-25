---
layout: post
title: Breaking Clicking Bad 2.0
---

Back in 2013 I wrote a [blog post](http://web.archive.org/web/20140517110403/http://infectedshadow.net/) on a fun little Breaking Bad themed clicker game called [Clicking Bad](http://clickingbad.nullism.com/). It was an interesting little game. I had found it early in the development and being the mischeviously curious developer that I am I decided to poke around the code. I managed to write my own little "hack" menu for the game which allowed me to add my own values. The developer kept upping the complexity a bit more over time, even adding a little honeypot for anyone who looked at the game object that triggered a "Counterfeit" achievement.

I had forgotten about it until the other night when a friend of mine posted in our Discord server the link to the game. Unfortunately I had lost my original javascript code with the auto-clicking and value changers. So what did I do with my Friday night? You guessed it. I rewrote my original hack. For those who just want the code, scroll to the bottom.

I did a few things differently this time. I wanted it to work organically into the site once the javascript was run in the console. So I did some poking and as soon as it runs it creates a tab and amenu page for my options and then auto open that menu. Next I used a Module design pattern to setup the functions. I had to make two internal functions for handling the cooking and selling because I found that if I just tried to run those stright through the setInterval they threw errors.

The one thing that had drastically changed from the last time I wrote this was the way to get around the counterfeit achievment. Not too much different that a little reverse engineering couldn't solve, though.

{% highlight js %}
    var hack = (function() {
        console.log("Breaking Clicking Bad");

        $('#tab_btns').append('<button onclick="switch_tab(\'hacks\');" id="hack_tab" class="tab">Hacks</button>');
        $('#tab_divs').append('<div class="tab_div w_div" id="hacks_div" style="display:none;"></div>')
        
        $('#hacks_div').append('<strong><span class="brba">Br</span>eaking <span class="brba">Cl</span>icking <span class="brba">Ba</span>d</strong><hr />');
        $('span.brba').css({'background':'#349E4E', 'color':'#FFFFFF', 'border':'1px solid #1F6931', 'padding':'0px 1px 0px 1px'});
        
        $('#hacks_div').append('<h3>Auto-Clickers</h3>');
        
        $('#hacks_div').append('<label><input type="checkbox" id="hack-cooker" onchange="hack.toggleCook()"> Auto Cook</label><br />');
        $('#hacks_div').append('<label><input type="checkbox" id="hack-seller" onchange="hack.toggleSell()"> Auto Sell</label><br /><br />');
        
        $('#hacks_div').append('<h3>Add Cash/Meth Without Triggering Counterfeit</h3>');
        
        $('#hacks_div').append('<input id="hack-addCash" class="hack-textbox" type="text" /><button onclick="hack.addCash(document.getElementById(\'hack-addCash\').value)">Add Cash</button><br />');
        $('#hacks_div').append('<input id="hack-addMeth" class="hack-textbox" type="text" /><button onclick="hack.addMeth(document.getElementById(\'hack-addMeth\').value)">Add Meth</button><br />');
        $('.hack-textbox').css({ 'border': '1px solid #aaa', 'background': '#fff', 'border-radius': '5px', 'padding': '5px' });

        $('#hacks_div').append('<br /><br /><hr />');
        $('#hacks_div').append('<span style="font-size: 8px;"><strong>Hack created by:</strong> Craig Smarkus (<a href="http://blog.csmarkus.com">http://blog.csmarkus.com</a>)</span>');
        
        $('#hack_tab').trigger('click');
        
        var cookClicker = false;
        var cooker = null;

        var sellClicker = false;
        var seller = null;

        var interval = 10;

        var doCook = function() {
            gm.do_make_click();
        }

        var doSell = function() {
            gm.do_sell_click();
        }
        
        var doAddCash = function(amount) {
            gm.do_export();
            var save = $.parseJSON(Base64.decode($('#impexp').val()));
            save.sv.c = (parseInt(save.sv.c, 24) + parseInt(amount)).toString(24);
            $('#impexp').val(Base64.encode(JSON.stringify(save)));
            gm.do_import();
        }
        
        var doAddMeth = function(amount) {
            gm.do_export();
            var save = $.parseJSON(Base64.decode($('#impexp').val()));
            save.sv.w = (parseInt(save.sv.w, 24) + parseInt(amount)).toString(24);
            $('#impexp').val(Base64.encode(JSON.stringify(save)));
            gm.do_import();
        }

        var toggleCookClicker = function() {
            if (!cookClicker)
                cooker = setInterval(doCook, interval) 
            else
                clearInterval(cooker) 

            cookClicker = !cookClicker;
        }

        var toggleSellClicker = function() {
            if (!sellClicker)
                seller = setInterval(doSell, interval) 
            else
                clearInterval(seller) 

            sellClicker = !sellClicker;
        }

        return {
            toggleCook: toggleCookClicker,
            toggleSell: toggleSellClicker,
            addCash: doAddCash,
            addMeth: doAddMeth
        };

    })();
{% endhighlight %}
