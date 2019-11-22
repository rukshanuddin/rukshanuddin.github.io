---
layout: post
title:      "Object Architecture in Ruby"
date:       2019-11-22 04:37:59 +0000
permalink:  object_architecture_in_ruby
---

## Inheritance, Supers and Modules

Everything in Ruby is an object. As everything is an object, there are basic rules that every object will follow. All objects can inherit from another object. All objects can modify methods that are inherited 

### Inheritance

What is **inheritance**?

If we look at the dictionary definition of inheritance, in other words, we see that it would refer to something received from someone else.

Bus drivers, ambulance and taxi cab drivers all share common behavior in that they navigate the roadways, but they are different from each other and each have their unique qualities that make them different from each other. We could say that they all inherit driving skills that come from just a basic drivers license. They are all different kinds of drivers. You could even say, different classes of drivers.

In the world of Ruby, inheritance refers to being able to create classes with common behavior, while still differentiating those classes.

For our example, with inheritance, we could inherit the BusDriver, AmbulanceDriver and TaxicabDriver classes from a Driver class. Then, any changes made to the Driver class would apply to the other class. In the Driver class we could have the basic skills that come with having a drivers license: stopping, taking turns, reverse, etc. All our other classes could inherit those things that the Driver does.

```

AmbulanceDriver < Driver

BusDriver < Driver

TaxicabDriver < Driver

Driver.turn, Driver.reverse, Driver.stop ….

```

We could have class methods for all those things that the Driver does. So since each of our other classes inherit from the Driver class, we could do use any of our methods that our Driver class could, with any of the classes that inherited from the Driver class. i.e:

```


AmbulanceDriver.turn

TaxicabDriver.reverse

BusDriver.stop


```
### Super
What is the super keyword? Let's take our example of drivers from before. We have a parent, Driver class, that our TaxicabDriver, BusDriverand AmbulanceDriver classes inherit from.
Let's imagine our Driver class has a method, valid_license, that sets an instance variable, @valid_license equal to true.


```

class Driver

  def valid_license
	
    @valid_license = true
		
  end
	
end

```


A taxicab driver would need to have a valid license as well. But in addition to a normal license, they need a chauffeur's license. Since we inherited the valid_license method all we would need to do is add in anything that is added to the code. i.e:


```
class TaxicabDriver < Driver
  
	def valid_license
  
      super

      @chauffeurs_license = true
			
  end
	
end
```


In my example, the TaxicabDriver inherits the valid_license method and uses super to add the fact we are setting the instance variable of chauffeurs_license to true. Real-world example would be that a taxicab driver has both a driver's license and a chauffeur's license. He has everything a normal driver has, but has a extra things that only taxicab drivers have.

### Modules
A module to put it simply, can be a set of methods that don't necessarily respond to a hierarchical order. Let's look back at our real-world examples. All our drivers inherited from the Driver class because they were all types of drivers. Let's expand on that. Let's talk about the rules of the roads. Not only do drivers have to follow them, pedestrians, motorcyclists and bicyclists all would have methods that would be shared. We could put these in a module to use inside the respective classes. For example:


```

module RoadRules

    def traffic_lights(color)

        if color == "red"
	 
           stop
				 
       elsif color == "yellow"
		 
            go if @traffic == "clear"
				 
       elsif color == "green"
		 
           go
				 
       end
		 
    end
	
	
    def go
	
       move_forward
		 
    end
	
	
    def stop
	
       stay_in_place
		 
    end
	
	
end


```

Now this is a mix of pseudocode and actual code to explain certain parts, we would have to go more in-depth to get everything to actually work (the go and stop methods don't do anything in Ruby, they are just examples of what we would expect Drivers and Pedestrians to do), but this is basically what we could want our module to do. Any one of our classes of people that use the road (Driver, Motorcyclist, Pedestrian, etc.) could include this module in their class to be able to use the "RoadRules". It doesn't make our Motorcyclist a Driver or our Pedestrian a Cyclist. All it does create a module of "RoadRules" that can be included in any class.

```
class Pedestrian

    include RoadRules

end

class Driver

include RoadRules

end
```

In the example above, both Pedestrians and Drivers include the RoadRules module, meaning that both Pedestrians and Drivers can use methods defined in RoadRules, without having a direct connection to each other. There is no hierarchical order, they are only linked because of their shared module, "RoadRules"

read the full edited version on medium! This is a working draft.


