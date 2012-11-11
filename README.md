## Overview

Mockatoo is a Haxe library for mocks creation, verification and stubbing.

Uses Haxe macros to generated mock implementations of classes and interfaces for testing.
Tested against Haxe 2.10 across most platforms (AVM2, JavaScript, Neko, C++, etc)

Mockatoo is inspired by **Mockito**'s public API <http://docs.mockito.googlecode.com/hg/latest/org/mockito/Mockito.html>


> Disambiguation: The **Mockatoo** belongs to the bird family *Cacatuidae* and look suspiciously like a taxidermied [Cockatoo](http://en.wikipedia.org/wiki/Cockatoo) with fake plumage. They are mostly found nesting within testing habitats and like  to repeat what you say (like a parrot). A Mockatoo may turn violent if mistaken for a *MockingBird* :)


## Installation

Install current official release from haxelib (1.3.0)

	haxelib install mockatoo

Install the latest directly from github:

	haxelib git mockatoo https://github.com/misprintt/mockatoo.git src

Or point to your local fork:

	haxelib dev mockatoo /ABSOLUTE_PATH_TO_REPO/src


## Features

Import and use the 'using' mixin

	import mockatoo.Mockatoo;
	using mockatoo.Mockatoo;

Mock any class or interface, including typedef aliases and types with generics (type paramaters)

	var mockedClass = SomeClass.mock();
	var mockedInterface = SomeInterface.mock();
	var mockedClassWithTypeParams = Foo.mock([Bar]); //e.g. Foo<Bar>

Verify a method has been called with specific paramaters (cleaner syntax since 1.3.0)

	mock.someMethod().verify();
	mock.someMethod("foo", "bar").verify();

Define a stub response when a method is invoked

	mock.foo("bar").returns("hello");
	mock.someMethod("foo", "bar").throws(new Exception("error"));

Custom argument matchers and wildcards

	mock.foo(Mockatoo.anyString()).returns("hello");
	mock.foo(Mockatoo.anyString()).verify();

Verify exact number of invocations 

	mock.foo().verify(2);//raw integers supported since 1.3.0
	mock.foo().verify(times(2));
	mock.foo().verify(atLeast(2));
	mock.foo().verify(atLeastOnce);
	mock.foo().verify(never);

Spying on real objects (Since 1.1.0)

	var spy = SomeClass.spy();//creates instance where all methods are real (not stubbed)
	spy.foo(); // calls real method;
	
	spy.foo().stub();
	spy.foo(); //calls default stub;
	
	spy.foo().returns("hello");
	spy.foo(); //calls custom stub;


Partial Mocking (Since 1.2.0)

	var mock = Mockatoo.mock(SomeClass);
	mock.foo().callsRealMethod();
	mock.foo();//calls out to real method


Mock properties that are read or write only (Since 1.2.0)

	mock.someProperty.returns("hello");
	mock.someSetter.throws("exception");
	mock.someGetter.throws("exception");
	mock.someGetter.calls(function(){return "foo"});


Click here for detailed [documentation and examples](http://github.com/misprintt/mockatoo/wiki/Developer-Guide)


## Release Notes

### New in 1.3.0

Mockatoo 1.3.0 provides a simplified, smarter, macro enhanced API when using Haxe's 
'using' mixin (and is still fully backwards compatible with existing API).

	using mockatoo.Mockatoo;
	...
	var mock = SomeClass.mock();
	var mock = SomeInterface.mock();
	var spy = SomeClass.spy(); // partial mock that calls real methods unless stubbed

New macros have been added for simplified stubbing with 'using':

	mock.someMethod().returns("foo");
	mock.someMethod("foo").throws("some error");
	mock.someMethod("bar").calls(function(args){return "bar";});
	mock.someMethod().callsRealMethod();
	mock.someMethod().stub(); // resets to default stub value (i.e. null)

You can allso stub properties and getter setters (since 1.2.0)

	mock.someProperty.returns("some value");

Verifications now also support raw integer counts

	mock.someMethod().verify(1); //converted to verify(times(1))

Verfifying and stubbing mock fields are now validated at compile time to prevent
runtime exceptions caused by out of date field references.

	mock.someMethodThatDoesNotExist().verify(); //causes compilation error

The syntax for wildcard Matchers has been updated to be compiler-safe when using 'using'

	mock.someMethod(Mockatoo.anyString()).returns("foo");

## Credits

Mockatoo is heavily inspired by **Mockito**'s public API <http://docs.mockito.googlecode.com/hg/latest/org/mockito/Mockito.html>

Mockatoo uses [tink_macros](https://github.com/back2dos/tinkerbell) for a lot of the low level macro manipulations.