Hypodermic
==========

Hypodermic is an IoC container for C++. It provides dependency injection to your existing design.


Ackowledgment
=============

Hypodermic is mostly inspired from the famous .NET [http://code.google.com/p/autofac/ Autofac]. A thousand thanks to its creators.

Adding components
=================

Components are _registered_ in a `ContainerBuilder`.
```
ContainerBuilder builder;
```

Registering concrete types
==========================

Considering these classes:

http://hypodermic.googlecode.com/hg/resources/home_page_simple_diagram.png

You can invoke `ContainerBuilder::registerType()` with a type as a template parameter.
```
builder.registerType< Driver >();
```

Expressing dependencies
=======================

```
class Car : public ICar
{
public:
    Car(std::shared_ptr< IDriver > driver);
}
```
Note that `Car` is needing to be injected a `IDriver`. We need to tell the container that `Driver` is actually a `IDriver` by setting up this concrete type as an interface:
```
builder.registerType< Driver >()->as< IDriver >();
```

Now, we can setup `Car`:
```
builder.registerType< Car >(CREATE(new Car(INJECT(IDriver))))->as< ICar >();
```


Resolving types
===============

Calling `ContainerBuilder::build()` creates a container:
```
auto container = builder.build();
```
Now it is time to get our instance:
```
auto car = container->resolve< ICar >();
```

That's all folks!
