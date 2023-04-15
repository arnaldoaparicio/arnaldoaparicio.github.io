---
layout: post
title:  "How to print an ArrayList in Java"
---

# Overview

So recently, I was working on an old Turing project but rather than work on it using Ruby, I worked on it using Java.
Something I noticed is that it is not so easy to print an array in Java like it is in Ruby. As you work on the project in Ruby, you should be able to return an array of turns taken. it's easy enough to do that like this.

For some context, this is a portion of the Turn class in Ruby

{% highlight ruby %}
class Turn
  attr_reader :guess, :card

  def initialize(guess, card)
    @guess = guess
    @card = card
  end
{% endhighlight %}

{% highlight ruby %}
p round.turns
{% endhighlight %}

And it returns
{% highlight console %}
[#<Turn:0x000000013a965150 @guess="tessa", @card=#<Card:0x000000013a965a88 @question="What is the first name of the main character in Silent Hill 3?", @answer="heather", @category="Video Games">>]
{% endhighlight %}

But when trying it in Java in a similar way, this happens

{% highlight java %}
System.out.println(round.getTurns());
{% endhighlight %}

{% highlight console %}
[Turn@4aa298b7]
{% endhighlight %}

What's going on here? It's not printing everything out.

Hmmmm....how are we able to do this?


Much like earlier in this post and for some context, here is a portion of the Turn class in Java.

{% highlight java %}
import java.util.ArrayList;
import java.util.stream.*;
import java.util.List;
public class Round {

    private Deck deck;
    private ArrayList<Turn> turnsTaken = new ArrayList<Turn>();

    Round(Deck deck) {
        this.deck = deck;
    }
{% endhighlight %}

This is how the ```get.Turns()``` method looks like
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

This may be A LOT to digest but lets break this down.

First, I created an empty ArrayList as a local variable within the ```getTurnsTaken()``` method.

Next, I took the ```turnsTaken``` variable and iteration over it.

What I'm basically doing is attaching the instance of Turn to the card that is part of the turn, attach the card details, and converting it to a string. Now that it is formatted, I add the newly formatted string into the ArrayList ```formattedTurnsTaken```.

Afterwards, I return the ```formattedTurnsTaken``` ArrayList.  But I must return a string since the method will return a string. So I converted the ArrayList ```formattedTurnsTaken``` to a string using ```.toString()```.


