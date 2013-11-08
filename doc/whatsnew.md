## LightInject 3.0.0.9 ##

### Binary Distribution ###

**LightInject** now offers a binary distribution in addition to the current distribution model that simply install the source files into the target project. This means that users now have the option of either using the binary (assembly) version or the source version.

<div class="nuget-badge" >
   <p>
         <code>PM&gt; Install-Package LightInject</code>
   </p>
</div>

Binary distribution is now the "default" model for **LightInject** and users that still want to use the source version needs to remove the existing **LightInject** package and install the source version instead.
  
<div class="nuget-badge" >
   <p>
         <code>PM&gt; Install-Package LightInject.Source</code>
   </p>
</div>

These changes applies to all the LightInject packages such as **LightInject.Annotation**, **LightInject.Web** and so on. 

### Platform support ###

Added Windows Phone 8 and SilverLight 

### Parameterized service resolution ###

LightInject now support passing arguments when resolving services.

    container.Register<int, IFoo>((arg, factory) => new Foo(arg));
    var foo = (Foo)container.GetInstance<int, IFoo>(42);
    Assert.AreEqual(42,foo.Value);

<a href="#" onclick = "$('#gettingstarted').trigger('click');"> Learn more... </a>


## LightInject.Mvc 1.0.0.1 ##

**LightInject.Mvc** provides an integration that enables dependency injection in ASP.NET MVC applications.

    protected void Application_Start()
    {
        var container = new ServiceContainer();
        container.RegisterControllers();        
        //register other services
        
        container.EnableMvc()              
    }


<a href="#" onclick = "$('#mvc').trigger('click');"> Learn more... </a>

## LightInject.Web 1.0.0.2 ##

Added a new extension method that more closely resembles the actual intent of **LightInject.Web**, which is to 
enable service to be scoped per web request. 

The following line of code 
  
    LightInjectHttpModule.SetServiceContainer(serviceContainer);

can now be replaced with

    container.EnabledPerWebRequestScope();

<a href="#" onclick = "$('#web').trigger('click');"> Learn more... </a>  

## LightInject 3.0.0.8 ##

### Internals ###

When running under the .Net platform, **LightInject** is now capable of instantiating classes with the internal access modifier.

<a href="#" onclick = "$('#gettingstarted').trigger('click');"> Learn more... </a>


### Interception ###

LightInject now enables Aspect Oriented Programming through method interception. 

<a href="#" onclick = "$('#interception').trigger('click');"> Learn more... </a>


### Decorators - Bugfix ###

Decorators were not applied when the service was registered as a value.

    var foo = new Foo();    
    container.Register<IFoo>(foo);




## LightInject 3.0.0.7##

###Lazy&lt;T&gt;###

Services can now be resolved as lazy instances.

    container.Register<IFoo, Foo>()
    var lazyInstance = container.GetInstance<Lazy<IFoo>>();

<a href="#" onclick = "$('#gettingstarted').trigger('click');"> Learn more... </a>

###Mocking open generic types###

    container.Register(typeof(IFoo<>), typeof(Foo<>));
    container.StartMocking(typeof(IFoo<>), typeof(FooMock<>));

    var instance = container.GetInstance<IFoo<int>>();

    Assert.IsInstanceOfType(instance, typeof(FooMock<int>));

<a href="#" onclick = "$('#unittesting').trigger('click');"> Learn more... </a>

###Lazy Registration###

Lazy registration means that we can register services on a need to have basis. We are no longer restricted to just one composition root that needs to reference all assemblies that possibly contain services that should be configured into the container.

<a href="#" onclick = "$('#gettingstarted').trigger('click');"> Learn more... </a>

###Overall performance improvements###

Although **LightInject** already performs very well, a significant amount of work has been put into further performance improvements.



 


## LightInject 3.0.0.6##

### WinRT ##

LightInject now implements full support for WinRT making it the perfect choice for Windows Store Apps. We can now leverage the same DI framework all across the layers even if some parts are based on .Net and others on WinRT.

### Assembly scanning ###
 
When scanning an assembly LightInject provides a predicate that is used to filter services going into the service container.

Given this type in the target assembly.

    public class Foo : IFoo, IDisposeable
    { 
        ....
    }    

When scanning the containing assembly, this would cause to services to be registered. 
One that maps **IFoo** to **Foo** and another that maps **IDisposable** to **Foo**. 


    container.RegisterAssembly(someAssembly, implementingType => implementingType.Namespace == "SomeNamespace");


Starting from version 3.0.0.6 this predicate has changed to provide both the service type and the implementing type.

This means that we now have more control over services going into the service container during assembly scanning.

    container.RegisterAssembly(someAssembly, (servicetype,implementingType) => serviceType != typeof(IDisposable)
    && implementingType.Namespace == "SomeNamespace");

 