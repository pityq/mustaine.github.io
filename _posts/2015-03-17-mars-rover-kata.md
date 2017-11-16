---
layout: post
title: Mars Rover Kata
excerpt: "Mars Rover Kata"
tags: [TDD, DDD, kata]
comments: true
---

Practicing [Test-Driven-Development (TDD)](http://en.wikipedia.org/wiki/Test-driven_development){:target="_blank"} and [Domain-Driven Design (DDD)](http://en.wikipedia.org/wiki/Domain-driven_design){:target="_blank"} with the [Mars Rover Kata](http://dallashackclub.com/rover){:target="_blank"}.

I am currently reading the book [Implementing Domain-Driven design](http://vaughnvernon.co/?page_id=168){:target="_blank"} by [Vaughn Vernon](https://twitter.com/vaughnvernon){:target="_blank"}.
It is an amazing book that is helping me to understand the concepts of DDD with examples.

I solved the following [Mars Rover kata](https://github.com/mustaine/katas/tree/master/mars-rover){:target="_blank"} focusing on the design.

## Mars Rover

Develop an api that moves a rover around on a grid.

### Rules:

1. You are given the initial starting point (x,y) of a rover and the direction (N,S,E,W) it is facing.
2. The rover receives a character array of commands.
3. Implement commands that move the rover forward/backward (f,b).
4. Implement commands that turn the rover left/right (l,r).
5. Implement wrapping from one edge of the grid to another. (planets are spheres after all)
6. Implement obstacle detection before each move to a new square. If a given sequence of commands encounters an obstacle, the rover moves up to the last possible point and reports the obstacle.

I am not interested in describing each step that I implemented to solve this kata, as you could check each commit on the [history](https://github.com/mustaine/katas/commits/master/mars-rover){:target="_blank"}.
Instead I will point out the most interesting parts.

I wanted to avoid the [anemic model](http://www.martinfowler.com/bliki/AnemicDomainModel.html), so I tried to separate the [value objects](http://martinfowler.com/bliki/ValueObject.html){:target="_blank"}
and the [entities](http://martinfowler.com/bliki/EvansClassification.html){:target="_blank"}, putting the behaviour into the domain objects.

From the beginning it was clear to me the relevance of using an Enum for the directions, since some business logic should be wrapped
in it - even though it was not obvious how it was going to be used when I started.

The first version was something like:

{% highlight java %}
public enum Direction {
    N("NORTH"),
    S("SOUTH"),
    E("EAST"),
    W("WEST");

    public String value;

    Direction(String value) {
        this.value = value;
    }
}
{% endhighlight %}

An interesting functionality was the direction inversion, which I refactored as:

{% highlight java %}
public Direction inverted() {
    return Direction.values()[(ordinal() + Direction.values().length / 2) % Direction.values().length];
}
{% endhighlight %}

I did not feel a good design that the responsibility to move the Rover was in Coordinate. AFAIK, a coordinate should only define a pair of points in space.

So I refactored it by decoupling the responsibility to move the Rover between Direction that decides where to move, and Rover which decides how to move.

I ended up with something like:

{% highlight java %}
public enum Direction implements Movement {
    NORTH("N") {
        @Override
        public Coordinate move(Rover rover) {
            return rover.moveUp();
        }
    },
    EAST("E") {
        @Override
        public Coordinate move(Rover rover) {
            return rover.moveRight();
        }
    },
    SOUTH("S") {
        @Override
        public Coordinate move(Rover rover) {
            return rover.moveDown();
        }
    },
    WEST("W") {
        @Override
        public Coordinate move(Rover rover) {
            return rover.moveLeft();
        }
    };

    private final String value;

    Direction(String value) {
        this.value = value;
    }

    public String value() {
        return this.value;
    }

    public Direction inverted() {
        return Direction.values()[(ordinal() + Direction.values().length / 2) % Direction.values().length];
    }

    public Direction rotateWith(Command command) {
        if (Command.LEFT == command)
            return Direction.values()[(Direction.values().length + ordinal() - 1) % Direction.values().length];
        if (Command.RIGHT == command)
            return Direction.values()[(Direction.values().length + ordinal() + 1) % Direction.values().length];
        throw new IllegalArgumentException("Command not valid");
    }
}

interface Movement {
    Coordinate move(Rover rover);
}
{% endhighlight %}

As you can notice, it is the Direction which is responsible for where to move and how to rotate.

Then the rover adds the logic of how to move as:

{% highlight java %}
 public Coordinate moveUp() {
        return Coordinate.create(position().x(), position().y().increase(world.limit().y()));
    }

    public Coordinate moveDown() {
        return Coordinate.create(position().x(), position().y().decrease(world.limit().y()));
    }

    public Coordinate moveLeft() {
        return Coordinate.create(position().x().decrease(world.limit().x()), position().y());
    }

    public Coordinate moveRight() {
        return Coordinate.create(position().x().increase(world.limit().x()), position().y());
    }

{% endhighlight %}

To me it makes sense that it is the rover who decides how to move. And it is the Point which has the logic to get to the next point, increasing or decreasing,
solving the problem of wrapping from one edge of the grid to another.

If you have other thoughts or comments, please post them, I would be really interested in seeing other solutions.

The Point has the responsibility to get the next Point and for wrapping:

{% highlight java %}
 public Point increase() {
        return Point.create(location() + 1);
    }

    public Point increase(Point limit) {
        return Point.create(increase().location() % limit.increase().location());
    }

    public Point decrease() {
        return Point.create(location() - 1);
    }

    public Point decrease(Point limit) {
        if (location() <= 0) return limit;
        return decrease();
    }
{% endhighlight %}

Once I started to add the obstacles, it became clear that the decision to add the World/Planet/Terrain concept was right. It was easy
to add the obstacles having a World which contains them, the limits of the World and the coordinates of the Rover.

At the end the rover is moving in a surface.

I am pretty sure that using DDD helps me to decouple the responsibilities and makes the code more expressive.

Recently a well-known friend [Felipe](https://twitter.com/felipefzdz){:target="_blank"} told me a sentence which I think it is very interesting

 <cite>"70-90% of the time we are reading code and only the rest we are coding."</cite>

so why don't we build more expressive code to avoid spend a lot of time to try to figure out what it is actually doing?

The full source is on a github repo [mars-rover](https://github.com/mustaine/katas/tree/master/mars-rover){:target="_blank"}

Another solution by [Viktor Farcic](https://twitter.com/vfarcic){:target="_blank"} is on [technologyconversations](http://technologyconversations.com/2014/10/17/java-tutorial-through-katas-mars-rover){:target="_blank"}

Please post your solution as a comment so that we can compare different ways to solve it.








