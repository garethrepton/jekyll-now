---
layout: post
title: Reduce unit test friction patterns - 5 - Autofixture as an automatic Test Data Builder
tags: unittesting c# patterns autofixture
---

## The Problem
This is largely the same problem as the [Test Data Builder](http://www.garethrepton.com/Reduce-unit-test-friction-patterns-4-Test-Data-Builder/), you have non trivial test data that needs repeated construction in many tests. I'm going to modify the example here though so it fits the use case of autofixture a little better, so we now have Lion and Rabbit Animal classes that we instantiate rather than just using a type on the Animal object:

   {% highlight CSharp %}
 
        [Fact]
        public void GivenMeatFeedsAllLions()
        {
            //Arrange
            var zoo = new Zoo();
            zoo.Animals.Add(new Lion(name: "Leo"));
            zoo.Animals.Add(new Lion(name: "Graham"));
            zoo.Animals.Add(new Lion(name: "Jeff"));
            zoo.Animals.Add(new Rabbit(name: "Roger"));
            zoo.Animals.Add(new Rabbit(name: "Peter"));
            zoo.Animals.Add(new Rabbit(name: "Fred"));

            //Act
            zoo.Feed("Meat");

            //Assert
            var lions = zoo.Animals.OfType<Lion>();
            var rabbits = zoo.Animals.OfRabbit<Lion>();
            foreach (var lion in lions)
                lion.IsFed.Should().BeTrue();
            foreach (var rabbit in rabbits)
                rabbit.IsFed.Should().BeFalse();
        }

   {% endhighlight %}

So we have a zoo, which houses animals, each animal is its own type, but each one also has a Name field which must be passed to the constructor which we don't care about in this particular test. Each one implements a Feed and IsFed method which I'll leave to the readers imagination as to how they work.

Now.. the test data builder solution is to create a builder to construct your test objects, so your test now looks like :

{% highlight CSharp %}
            [Fact]
            public void GivenMeatFeedsAllLions()
            {
                //Arrange
                var zoo = new ZooBuilder().WithLions("Test", 3).WithRabbits("Roger",3).Build();
                //Act
                zoo.Feed("Meat");
                //Assert
                var lions = zoo.Animals.OfType<Lion>();
                var rabbits = zoo.Animals.OfRabbit<Lion>();
                foreach (var lion in lions)
                    lion.IsFed.Should().BeTrue();
                foreach (var rabbit in rabbits)
                    rabbit.IsFed.Should().BeFalse();
            }
   {% endhighlight %}

We choose not to bother setting the name in the builder, and just have it create 3 of each type of animal, then the rest of the test continues as the previous test did. In this case, the builder requires less code in the test, and is more readable, but will take a lot more code to create the builder itself and unless you have a complex object model, the trade off isn't always worth it.

## The Autofixture Solution
This is where a test object creation tool like [Autofixture](https://github.com/AutoFixture/AutoFixture) comes in extremely handy. Basically it is a generic test data builder, so you can tell it to create many objects of a given type, give it the amount you want it to create, and it will use internal (customisable) algorithms to create that number pseudo random objects, so for strings an example would be:

{% highlight CSharp %}
var strings = new Fixture().CreateMany<string>(20);
{% endhighlight %}

This will create an enumerable of 20 strings, each populated according to its current configuration. It will also work with more complicated objects too:

{% highlight CSharp %}
public class Person
{
    public string Name {get;}
    public DateTime DateOfBirth {get;}

    public Person(string name, DateTime dateOfBirth)
    {
        this.Name = name;
        this.DateOfBirth = dateOfBirth;
    }
}

var people = new Fixture().CreateMany<Person>(5);
{% endhighlight %}

AutoFixture will automatically generate 5 Person objects in this code. The names will be pseudo random sequences of characters, and the birth dates won't make sense, but this can still be useful data in a test scenario.

Using Autofixture allowed us to reduce the amount of setup code required for the string nad Person examples above, lets see how this looks with the Zoo example:

{% highlight CSharp %}
    [Fact]
    public void GivenMeatFeedsAllLions()
    {
        //Arrange
        var fixture = new Fixture();

        var zoo = new Zoo();
        zoo.Animals.AddRange(fixture.CreateMany<Lion>(3));
        zoo.Animals.AddRange(fixture.CreateMany<Rabbit>(3));
        //Act
        zoo.Feed("Meat");
        //Assert
        var lions = zoo.Animals.OfType<Lion>();
        var rabbits = zoo.Animals.OfRabbit<Lion>();
        foreach (var lion in lions)
            lion.IsFed.Should().BeTrue();
        foreach (var rabbit in rabbits)
            rabbit.IsFed.Should().BeFalse();
    }
{% endhighlight %}

This will generate 3 lions and 3 rabbits, all with pseudo random names, the random names will automatically get piped into each objects constructor, and we don't need to worry about setting the name field that we're not interested in for this test. 
There is slightly more code in the test than with a dedicated builder object, but we do away with maintaining a separate builder object completely, and we've reduced the original setup code by several lines. This makes autofixture incredibly powerful as a simple test object builder, and in many cases you can create your model objects without having to worry about the parameters you are not explicitly testing for (just like our name field in the Animal object in the above test).

## Where it applies
Autofixture as a data builder is pretty useful for objects in tests where the value of the objects doesn't have to be specific, it just needs to be the correct type, or in cases where the test data objects have a bunch of constructor parameters that you don't really care about (i.e. the name in the above example). Autofixture also makes a good internal addition to a TestDataBuilder, for when you want to have more automatic ways to generate certain objects, but still want to keep a more readable interface for the test itself. 

## Where it probably shouldn't apply
Autofixture is not as good in places where you need to have very specific values for test data objects. I generally find if I am trying to heavily customise the standard AutoFixture algorithms (i.e. start defining weird sequences of numbers, or a bunch of valid strings that can be set) the tests can actually become more complicated than they would do by just writing the test data construction code manually. Customizations can be standardised though, so in some cases, its well worth creating a customisation for a commonly used type and reusing that in many tests.

## Summary
AutoFixture functions as an incredibly powerful automatic test data builder, but its automatability is both a blessing and a curse, if used sparingly in simpler scenarios it can really improve your test code by reducing the amount of filler code you have to write, if used in more complex scenarios it can be more complicated than manually creating the data, and somewhat obfuscate the readability of the tests. 

AutoFixture is well worth the effort of learning and having in your toolkit, when it does apply, it really is a time saver, just remember that it doesn't apply everywhere.



