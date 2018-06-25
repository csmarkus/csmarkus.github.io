---
layout: post
title: Mysteriously Missing Rows with SQL Right Join
---

So I ran into an interesting problem at work today. One of our reports wasn't showing all of the required locations. One instance turned out to be an error with another one of our systems. The rest led me down a 3 hour rabbit hole.

To start off with, this is the basic gist of the query:

{% highlight sql %}
select
    a.locationId,
    b.locationId,
from Table1 a
    right join linkedServer01.databaseName.dbo.v_alldata b on a.wlocId = b.otherLocationId
where
    a.dateEntered between '1/1/2016' and '1/31/2016
{% endhighlight %}

The data we were joining to was on a linked server. For whatever reason when that view on the linked server was right joined it would drop about 380 rows. This is bad because the report we were generating required that there be an entry even if there was no data. Thus the use of right join made sense as it would still generate a record for the specific location but fill it with null data which could be used to display "0" in the report.

For the first hour it boggled me as I tore the original query apart and made the super simplified version above. The next was poking and prodding at the query to see if it was anything with the data. No dice there.

After about two hours I contacted a friend (and some one who had originally written this) for some help/ideas. With his help I managed to find a work around:

{% highlight sql %}
select
    a.locationId,
    b.locationId,
from linkedServer01.databaseName.dbo.v_alldata b
right join (
    select 
        *
    from Table1
    where
        a.dateEntered between '1/1/2016' and '1/31/2016'
) a on a.wlocId = b.otherLocationId
{% endhighlight %}

So because we querying the table directly provided all the results now I'm querying the data from the local table in the join and then joining on all the data which forces it to get everything properly.