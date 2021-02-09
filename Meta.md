# Ruby metaprogramming 
## What is metaprogramming?
>### Metaprogramming is a technique by which we can write code that writes code by itself dynamically at runtime. This means we can define methods and classes during runtime. It can be used a way to add, edit or modify the code of our program while it’s running. Using it, we can make new or delete existing methods on objects, reopen or modify existing classes, catch methods that don’t exist, and avoid repetitious coding to keep our program _**dry**_.
## Techniques of metaprogramming
>* ### method_missing
>* ### Singleton class
>* ### instance_eval
>* ### class_eval
>* ### Introspect on instance variables
  
  
>>## method_missing
  ### It is a method in Ruby that intercepts calls to methods that don’t exist. It handles any messages that an object can’t respond to. Let’s take the example of it.

```ruby
class Human
  def walk
   puts "Human can walk"
   end
 end  
```
I’m telling a Akshay to walk,which is something he can do. So when we create an object Akshay from the class Human and pass it a method walk then it should respond to the method.

~~~ruby
Akshay = Human.new          #create a new instance called Akshay 
Akshay.walk                 #pass method walk to Akshay
=> "Human can walk!"        #Returns the correct value
~~~
### But let's tell something different
```
Akshay.fly
NoMethodError: undefined method 'fly' for object:Akshay
```
Now instead of the **NoMethodError** exception we get,I would like to tell the **"Akshay can't fly!**". This is where method_missing comes in handy. Method misiing let's us customize our own error messages with the concept of method overriding.
Here’s a syntax of method missing as defined in the basic object
``` ruby
def method_missing(m,*arg,&block)
  #return
end
```
#### This method takes in three parameters;
>m ~ This is the name of the missing method called on an object. It’s a symbol usually converted to a string before it’s used. It is a required parameter.

>*args ~ This sponges up any other remaining arguments. It’s an optional parameter.

>&blocks ~ This includes blocks called within the method. It’s also an optional parameter.
``` ruby
#creates a class called Random
class Random

#define method_missing
def method_missing(m,*args,&block)

#if the method passed is "fly"
if m.to_s=="fly"            
 puts "Akshay can"t fly!" #return this
 else                     #else
   super                  #return exception as original
  end
 end
end 
```
We now have our own class with our own method_missing. Let’s now create a Akshay object and call fly on it
```ruby
#creating a new instance of class Random
Akshay = Random.new          

#telling Akshay to fly
Akshay.fly                   
"Akshay can"t fly!"
```
### But what now if i tell something new...?
```
Akshay.sing
NoMethodError: undefined method 'sing' for object:Akshay
```
This is handled by the else part of our code above. When we call super, it refers us to the original **method_missing** on the Basic Object which has the **NoMethodError**. 
>>## Singleton class
  The singleton class is used when only a single object requires an addition, alteration, or deletion. It is designed exactly for this and allows us to do all that and more.
  ``` ruby
  name= "Amit kumar"

def name.k
  self
end

name.k   # => "Amit kumar"
```
On the first line, we create a new variable called **name** that represents a simple String value. On the second line, we create a new method called **name.k**, and give it a very simple content. Ruby allows us to choose what object to attach a method definition to by using the format **some_object.method_name**, which we may recognize as the same syntax for adding class methods to classes . In this case, as we had name first, the method has been attached to our name variable. On the final line, we call the new **name.k** method we just defined.

The **name.k** method has access to the entire object it has been attached to; in Ruby, we always refer to that object as **self**. In this case, **self** refers to the String value we attached it to. If we had attached it to a Array, self would have returned that Array object.
>>## instance_eval
 Having singleton classes is all good, but to truly work with objects dynamically we need to be able to re-open them at runtime within other functions. Unfortunately, Ruby does not syntactically allow us to have any class statements within a function. This is where we need instance_eval.
 
 It is defined in Ruby's **kernel** module and allows us to add instance methods to an object just like singleton class syntax.
 ``` ruby
 name = "Amit"
  name.instance_eval do
  def full_name
    "Amit kumar"
  end
end

name.full_name  # => "Amit kumar"
```
The **instance_eval** method can take a block or a string of code to be evaluated. Inside the block, we can define new methods as if we were writing a class, and these will be added to the singleton class of the object.
>>## class_eval
The **instance_eval** and **class_eval** are very similar, but their scope and application differs ever so slightly. We can remember what to use in each situation by remembering that we use instance_eval to make instance methods, and use class_eval to make class methods.
``` ruby
name = "Amit"
name.class.class_eval do
  def full_name
    "Amit kumar"
  end
end

name.full_name # => "Amit kumar"
```
>>## Introspect on instance variables
A trick that uses to make instance variables from the controller available in the view is to introspect on an objects instance variables. This is a grave violation of encapsulation, of course, but can be really handy sometimes. It's easy to do with **instance_variables, instance_variable_get and instance_variable_set**. To copy all **instance_variables** from one object to another, we could do it like this:
``` ruby
 from.instance_variables.each do |v|
 to.instance_variable_set v, from.instance_variable_get(v)
end
```
## **References**
* https://www.youtube.com/watch?v=lZfv4H-9ato
* https://www.youtube.com/watch?v=c7bhHGNL3t8
*  https://medium.com/podiihq/method-missing-in-ruby-af4c6edd5130
* https://medium.com/swlh/metaprogramming-in-ruby-1b69b1b54202
* https://www.youtube.com/watch?v=W9uGmyHsrZ0
* https://www.youtube.com/watch?v=R8JNZeasliI