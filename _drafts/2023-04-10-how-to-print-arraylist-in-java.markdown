---
layout: post
title:  "Printing an Array in Ruby vs. Java"
---

# Overview

So recently, I did a redo on an old [Turing Mod 1 project called FlashCards](https://backend.turing.edu/module1/projects/flashcards/) but rather than use Ruby, I built it from scratch using Java.

Something I noticed is that it is not so easy to print an array in Java like it is in Ruby. [One of the requirements of this project](https://backend.turing.edu/module1/projects/flashcards/iteration_2), you are tasked to create a Round class. One of the instance variables you create is called ```turns``` with the default value of an empty array. The ```turns``` instance variable contains every instance of the Turn class along with an instance of the Card class tied to it as well as a ```guess``` in the form of a string.

Before proceeding further, I want to clarify that for Java, I am printing an ArrayList not an Array. For more clarification on the differences between the two, [here is a great resource](https://www.javatpoint.com/difference-between-array-and-arraylist) for a more in-depth explanation.

For some context, this is a portion of the Turn class in Ruby containing the initializer.

{% highlight ruby %}
class Turn
  attr_reader :guess, :card

  def initialize(guess, card)
    @guess = guess
    @card = card
  end
{% endhighlight %}


And this is a portion of the Turn class in Java, showing its constructor and a couple of getters.

{% highlight java %}

public class Turn {

    private String guess;
    private Card card;

    Turn(String guess, Card card) {
        this.guess = guess;
        this.card = card;
    }

    public String getGuess() {
        return this.guess;
    }

    public Card getCard() {
        return this.card;
    }

{% endhighlight %}


## In Ruby

Let's focus on the Ruby part first.

This is currently how the ```Round``` class looks like.

{% highlight ruby %}

class Round
  attr_reader :deck, :turns

  def initialize(deck)
    @deck = deck
    @turns = []
  end

{% endhighlight %}

Lets assume that an instance of a Turn exists in the ```@turns``` array.

According to the iteration, if we simply did this
{% highlight ruby %}
p round.turns
{% endhighlight %}

It will return this.
{% highlight console %}
[#<Turn:0x000000013a965150 @guess="tessa", @card=#<Card:0x000000013a965a88 @question="What is the first name of the main character in Silent Hill 3?", @answer="heather", @category="Video Games">>]
{% endhighlight %}

So much like how it is shown in the iteration, it will return the Turn object, along with it's guess and the card tied to it, as well as the card's details (that being a card's question, answer, and category).

This is what we want to do in Ruby. But what about Java?

## In Java

Much like earlier in this post and for some context, here is a portion of the Round class in Java.

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

Unfortunately, we cannot do what we can do in Ruby. 

So if we did this
{% highlight java %}

System.out.println(round.turnsTaken);

{% endhighlight %}

This would not work because the ```.turnsTaken``` part is private and we cannot call it this way. However, we can create a method that will return our ArrayList.

I'll create a ```getTurns()``` method and this is how it looks like

{% highlight java %}
    public ArrayList<Turn> getTurns() {
        return turnsTaken;
    }
{% endhighlight %}

Now that we created this method, we will call it.

{% highlight java %}
System.out.println(round.getTurns());
{% endhighlight %}

Assuming the ArrayList contains one instance of a Turn, this is the result

{% highlight console %}
[Turn@4aa298b7]
{% endhighlight %}

What's going on here? It's not printing everything out. It seems to print out the memory address but no other details about the turn. Not the Card details that are tied to the turn nor the turn's guess.

Hmmmm....how are we able to do this?

It looks like we are going to need something more. 

## A solution in Java

So our first attempt wasn't successful. But when there's a will, there's a way.

I'll be creating a new method called ```getTurnsTaken()```

{% highlight java %}

    public String getTurnsTaken() {
        
    }

{% endhighlight %}

This method will return a String instead of an ArrayList of Turn. Lets add some stuff to it.

First we will create a new ArrayList within the method. We will name it ```formattedTurnsTaken```. More on that later.

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();
    }

{% endhighlight %}

Next, we will be calling the ```forEach``` method and use it on our instance variable ```turnsTaken```

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

         turnsTaken.forEach(turn -> {
...
{% endhighlight %}

So what's going on here? We will be iterating through the ```turnsTaken``` ArrayList and we will ```add``` to our newly created ```formattedTurnsTaken``` ArrayList.

As we iterate through out ```turnsTaken``` ArrayList, we will have access to all instances of Turn, along with the guess and the Turn's card. Let's set this up.

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

         turnsTaken.forEach(turn -> {
            formattedTurnsTaken.add(String.format())
...
{% endhighlight %}

Now we're passing through the ```String format``` method. What does this do? We create a new string and format it to our liking. Lets try it.

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

         turnsTaken.forEach(turn -> {
            formattedTurnsTaken.add(String.format("%s card=%s question=%s"))
...
{% endhighlight %}

What is ```%s```? It is a format specifier that will allow us to pass data into the string. The ```String format``` method will allow us to pass in multiple arguments that will add to our string.

Remember, we're still in the loop and have access to each instance of Turn, the Turn's card and guess. Let's add more.

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

         turnsTaken.forEach(turn -> {
            formattedTurnsTaken.add(String.format("%s card=%s question=%s", turn.toString(), turn.getCard(), turn.getCard().getQuestion()))
...
{% endhighlight %}

So what did we pass into our method? 



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

First we want to explicitly mention the return type. In this case, it will return a string.

I created an empty ArrayList as a local variable within the ```getTurnsTaken()``` method.

Next, I took the ```turnsTaken``` variable and iteration over it.

What I'm basically doing is attaching the instance of Turn to the card that is part of the turn, attach the card details, and converting it to a string. Now that it is formatted, I add the newly formatted string into the ArrayList ```formattedTurnsTaken```.

Afterwards, I return the ```formattedTurnsTaken``` ArrayList.  But I must return a string since the method will return a string. So I converted the ArrayList ```formattedTurnsTaken``` to a string using ```.toString()```.


