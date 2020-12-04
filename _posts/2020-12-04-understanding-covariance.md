---  
title: "How I Understand Covariance"
comments: true
category: ["scala", "fp"]
---  
The definition says that Covariance can be defined as:  
>A type parameter T of a generic class can be made covariant by using the annotation +T. For some class List[+T], making T covariant implies that for two types A and B where B is a subtype of A, then List[B] is a subtype of List[A]. This allows us to make very useful and intuitive subtyping relationships using generics.  
<br/>
Easy. Right?  
<br/>
A situation where this can be applied is in case of a Tree.<br/>
A Tree can be defined with - The Tree itself, a Node, an End, and a definition for apply that takes only a value:<br/>
* Tree:<br/>
{% highlight scala %}
sealed trait Tree[+A]
{% endhighlight %}  
<br/>
* Node:<br/>
{% highlight scala %}
case class Node[A](value: A, left: Tree[A], right: Tree[A]) extends Tree[A]
{% endhighlight %}<br/>
* End:<br/>
{% highlight scala %}
case object End extends Tree[Nothing]
{% endhighlight %}<br/>
* Apply for End: 
{% highlight scala %}
object Node {
  def apply[A](value: A): Node[A] = Node(value, End, End)
}
{% endhighlight %}<br/>
What makes this Covariant?<br/>
In the Scala type hierarchy, the bottom most type is called `Noting` and all types have `Nothing` as their subtype.<br/>
For Example: Nothing <: Int<br/>
By modifying the type parameter for Tree[A] to Tree[+A], we basically would establish a rule that tells this story:<br/>
{% highlight shell %}
if Nothing <: Int then Tree[Nothing] <: Tree[Int]
{% endhighlight %}  
