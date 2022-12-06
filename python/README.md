# Tricks for ***Python***

- Define a function and define a new class type that contains the function
```python
    #myfile.py
    import sys

    def as_class(class_name):
        import functools
        def decorator_factory(method):
            new_type = type(class_name, (object,), {
                method.__name__: lambda self, *args, **kwargs: method(*args, **kwargs)
            })
            setattr(sys.modules[__name__], class_name, new_type)
            @functools.wraps(method)
            def new_method():
                return method() # return the method to be used normally
            return new_method
        return decorator_factory

    @as_class(class_name="NewClass")
    def function(name): # this method can recieve the 'self' param if it is going to use it
        print(f"hello there, {name}")

    NewClass = getattr(sys.modules[__name__],"NewClass")
    instance = NewClass()
    print(type(instance))  # <class '__main__.NewClass'>
    instance.function("user")  # hello there, user

``` 
Now, the class is accessible to other files
```python
    #anotherfile.py
    from .myfile import NewClass
    NewClass().function("new user") # hello there, new user
``` 