---
layout: post
title:  "Printing an Array in Ruby vs. Java"
---

<img src="https://i.imgur.com/6B70Qy4.png">

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
<br>

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

<br>

According to the iteration, if we simply did this
{% highlight ruby %}
p round.turns
{% endhighlight %}

It will return this.
{% highlight console %}
[#<Turn:0x000000013a965150 @guess="tessa", @card=#<Card:0x000000013a965a88 @question="What is the first name of the main character in Silent Hill 3?", @answer="heather", @category="Video Games">>]
{% endhighlight %}

<br>

So much like how it is shown in the iteration, it will return the Turn hexadecimal, along with it's guess and the card tied to it (mainly the hexadecimal), as well as the card's details (that being a card's question, answer, and category).

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

So if we did this
{% highlight java %}

System.out.println(round.turnsTaken);

{% endhighlight %}

This would not work because the ```.turnsTaken``` instance variable is private and we cannot call it this way. However, we can create a method that will return our ArrayList.

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

What's going on here? It's not printing everything out. It seems to print out the hexadecimal but no other details about the turn. Not the Card details that are tied to the turn nor the turn's guess.

Hmmmm....how are we able to do this?

It looks like we are going to need something more. 

## A solution in Java

### Part 1: Creating a new ArrayList

So our first attempt wasn't successful. But when there's a will, there's a way.

I'll be creating a new method called ```getTurnsTaken()```

{% highlight java %}

    public String getTurnsTaken() {
        
    }

{% endhighlight %}

This method will return a String instead of an ArrayList. Lets add some stuff to it.

First we will create a new ArrayList within the method. We will name it ```formattedTurnsTaken```. More on that later.

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();
    }

{% endhighlight %}

### Part 2: forEach()

Next, we will be calling the ```forEach``` method and use it on our instance variable ```turnsTaken```

{% highlight java %}

    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

         turnsTaken.forEach(turn -> {
...
{% endhighlight %}

So what's going on here? We will be iterating through the ```turnsTaken``` ArrayList and we will ```add``` to our newly created ```formattedTurnsTaken``` ArrayList.


### Part 3: String.format()

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

So what did we pass into our method? We passed the Turn hexadecimal, the card hexadecimal, and the card's question. But this is only part of what it should return.

Let's complete it like this

{% highlight java %}
    public String getTurnsTaken() {
        ArrayList<String> formattedTurnsTaken = new ArrayList<String>();

        turnsTaken.forEach(turn -> {
            formattedTurnsTaken.add(String.format("%s guess=%s card=%s question=%s, answer=%s, category=%s", 
            turn.toString(), turn.getGuess(), turn.getCard(), turn.getCard().getQuestion(), turn.getCard().getAnswer(), turn.getCard().getCategory()));
        });
        return formattedTurnsTaken;
    }
{% endhighlight %}

Now this looks more complete! We have included everything mentioned above as well as the card's answer and the card's category.

So this means we're done right?

Well if we were to run it as it is, it throws an error

{% highlight java %}
System.out.println(round.getTurnsTaken());
{% endhighlight %}

{% highlight console %}
Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
        Type mismatch: cannot convert from ArrayList<String> to String
...
{% endhighlight %}

What does this mean? Our ```getTurnsTaken()``` method must return a String but instead tries to returns the ArrayList ```formattedTurnsTaken```. But since we explicitly stated we want to return a string, this will break.


### Part 4: toString()

What do we do now? Let's convert the ArrayList to a String. It's a simple fix.

All we need to do is add ```.toString()``` to the end of the ```formattedTurnsTaken``` ArrayList.


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

This will convert ```formattedTurnsTaken``` to a String and now this method will work correctly.

Again, let's assume there exists an instance of a turn. So now when we print this out
{% highlight java %}
System.out.println(round.getTurnsTaken());
{% endhighlight %}


This is what we get.

{% highlight console %}

[Turn@28d93b30 guess=tessa, card=Card@4554617c, question=What is the first name of the main character in Silent Hill 3?, answer=heather, category=Video Games]

{% endhighlight %}

We printed the details successfully!

### Closing thoughts

Phew! This seems like a lot for Java. But I think this process has allowed me to really challenge myself as someone relearning Java and also appreciate the convenience that Ruby provides.

To anyone looking to start coding in Java, it may seem intimidating but I assure you that it is a very great language to learn. It's the most fun I had in a while and I'm sure you will enjoy it too!