---
layout: post
title:  "How to print an ArrayList in Java"
---

# Overview

So recently, I was working on an old Turing project but rather than work on it using Ruby, I worked on it using Java.
Something I noticed is that it is not so easy to print an array in Java like it is in Ruby. As you work on the project in Ruby, you should be able to return an array of turns taken. it's easy enough to do that like this.

##RUBY CODE
{% highlight ruby %}
p round.turns
{% endhighlight %}

And it returns
{% highlight console %}
[#<Turn:0x000000013a965150 @guess="tessa", @card=#<Card:0x000000013a965a88 @question="What is the first name of the main character in Silent Hill 3?", @answer="heather", @category="Video Games">>]
{% endhighlight %}

But when trying it in Java in a similar way, this happens
##BROKEN JAVA CODE

{% highlight java %}
System.out.println(round.getTurns());
{% endhighlight %}

{% highlight console %}
[Turn@4aa298b7]
{% endhighlight %}

What's going on here? It's not printing everything out.

Hmmmm....how are we able to do this?

This is how the 'get.Turns()' method looks like
{% highlight java %}
    public ArrayList<Turn> getTurns() {
        return turnsTaken;
    }
{% endhighlight %}

It looks like we are going to need something more.

This is the end result 

{% highlight java %}
    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

        turnsTaken.forEach(turn -> {
            formattedTurnsTaken.add(String.format("%s card=%s question=%s, answer=%s, category=%s", 
            turn.toString(), turn.getCard(), turn.getCard().getQuestion(), turn.getCard().getAnswer(), turn.getCard().getCategory()));
        });
        return formattedTurnsTaken.toString();
    }
{% endhighlight %}

