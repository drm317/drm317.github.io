---
layout: page
title: Roulette Challenge
tagline:
categories:
- blog
tags:
- craftsmanship
- tdd
- kata
---

### How do time and random numbers affect software design?

Recently Jim Jeffries (<a href="https://www.twitter.com/jjeffries1">@jjeffries1</a>) <a href="https://twitter.com/jjeffries1/status/721690576786190337">tweeted</a> a coding challenge that he had encountered:

```
Anyone fancy having a go at a coding exercise I’ve written up and giving me
some feedback?
```

The tweet included a <a href="https://docs.google.com/document/d/18mBn4R_DsuHPSuZHCqTgXrhXfyPJW-Gd46EJS_mWy2k/edit">link</a> to a document describing the challenge.
A summary is provided here:

```
Estimated Duration: 1-3 hours
Author: James Jeffries
Language(s)/stacks: Any
Summary:
A roulette wheel has numbers from 0 to 36. A spin of the wheel takes 20
seconds and leaves the ball on a random number.
Design and implement a roulette wheel using TDD. Think about how using
time and random numbers affects your design.
Consider how much functionality each test covers and what responsibilities
you are testing.
```

Ron Jeffries (<a href="https://twitter.com/RonJeffries">@RonJeffries</a>) <a href="http://ronjeffries.com/articles/016-04/roulette-1/">blogged</a> about his approach to the challenge using TDD with Lua. He then went on to make several more attempts whilst describing his approach and thoughts along the way, involving the introduction of <a href="http://ronjeffries.com/articles/016-04/roulette-2/">more tests</a>, producing <a href="http://ronjeffries.com/articles/016-04/roulette-3/">another version</a> and adding a <a href="http://ronjeffries.com/articles/016-04/roulette-4/">graphical display</a>.

Dave Hounslow (<a href="https://twitter.com/thinkfoo">@thinkfoo</a>) also took up the challenge. The results of his efforts are <a href="https://thinkfoo.wordpress.com/2016/04/19/roulette-kata/">here</a>.

I thought I would give the challenge a go and reflect on my approach and the impact of time and randomness through emergent design and TDD.

### Iteration 1

Before coding I deliberately tried to think what the simplest test should be to start things off. Very often it's easy to get carried away with TDD and not think before you type.

Some folks like to start with constructor tests but in this case I settled for a test that would drive me to code a roulette wheel class which has a ball initially at rest. The 'ballNumber' is used to represent the position of the ball at any point in time.

```java
@Test
public void rouletteBallStartsAtZero() {
  RouletteWheel rouletteWheel = new RouletteWheel();
  assertEquals(0, rouletteWheel.getBallNumber());
}
```

Running this test gave me a red bar which I made green with the following code

```java
public class RouletteWheel {
  private int ballNumber = 0;

  public int getBallNumber() {
    return ballNumber;
  }
}
```

If I had wanted to allow the wheel to be set up with the ball at a given position I would have done this through a constructor that took an argument.

Starting with a constructor test might have driven this out but since there was no particular need to start the wheel with the ball at any other number I was happy enough.

So what next? Providing a 'spin' behaviour for the wheel seemed sensible enough so I wrote my second test

```java
@Test
public void rouletteWheelSpinsForTwentySeconds(){
  RouletteWheel rouletteWheel = new RouletteWheel();
  rouletteWheel.spin();
  assertEquals(20000, rouletteWheel.getElapsedSpinTime());
}
```

Rather than worry about the ball or the number that it lands on I simply tested that it should take twenty seconds. I chose to obtain the spin time from the roulette wheel itself rather than using a timed test

```java
private long elapsedSpinTime = 0;
private long spinStartTime = 0;

public void spin() {
  this.spinStartTime  = System.currentTimeMillis();

  do {
      bounce();
  } while (elapsedSpinTime != 20000);
}

private void bounce() {
  this.elapsedSpinTime = System.currentTimeMillis() - spinStartTime;
}

public long getElapsedSpinTime() {
  return elapsedSpinTime;
}
```

Real roulette wheels let the ball spin round before they bounce over the numbers. I chose to just let the ball start bouncing from the outset when the wheel started to spin. On each bounce the ball lands on a number which I chose to generate randomly in order to produce the final number (between 0 and 36).

```java
@Test
public void rouletteWheelBallLandsBetweenZeroAndThirtySix(){
  RouletteWheel rouletteWheel = new RouletteWheel();
  rouletteWheel.spin();
  int ballNumber = rouletteWheel.getBallNumber();
  assertTrue(0 <= ballNumber && ballNumber <= 36);
}
```

I added a random number generator to the bounce() method. This method is called over and over in the spin() method until twenty seconds is reached.

```java
private void bounce() {
  this.ballNumber = new Random().nextInt(37);
  this.elapsedSpinTime = System.currentTimeMillis() - spinStartTime;
}
```

There's no point in testing the random nature of the provided Random() class.
Given this new bounce behaviour I added tests to ensure the roulette wheel starts and ends in a stationary position

```java
@Test
public void rouletteWheelBallInitiallyNotBouncing() {
  RouletteWheel rouletteWheel = new RouletteWheel();
  assertFalse(rouletteWheel.isBouncing());
}

@Test
public void rouletteWheelBallStopsBouncing() {
  RouletteWheel rouletteWheel = new RouletteWheel();
  rouletteWheel.spin();
  assertFalse(rouletteWheel.isBouncing());
}
```

At this point I wanted to reflect on the design and how I dealt with random numbers and time in the code. 
