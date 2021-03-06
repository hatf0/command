# command
This is a simple command-line framework that I devised while writing a "very secret" project for an internship.

## Status
This library is still in-dev.

**PLEASE NOTE: By default, the library will *NOT* spawn a new thread and read from stdin. This functionality is *opt-in*, and you MUST compile the library with the `cli` version for this. This is done via:**
```
// dub.sdl
subConfiguration "command" "cli"
// dub.json
"subConfigurations": {
    "command": "cli"
}
    
```
**ADDITIONAL NOTE: Compiling the library with the `cli` will bring in `linenoise` as a dependency.** 

## Design Goals:
- Lightweight
    - Minimal dependencies (only 2, but 1 is optional)
- Portable
- Easy to use
## Examples
### Untyped example:
```d
class TestExample {
@CommandNamespace("test"):
    // name, description, min args, max args
    @Command("hello", "Hello, World!", 0, 0)
    string hello(string[] args) {
        return "Hello, World!";
    }

    @Command("user", "Say hello to someone!", 1, 1)
    string hello_user(string[] args) {
        return "Hello, " ~ args[0] ~ "!";
    }
}

mixin RegisterModule!TestExample;
```
### Typed example:
```d
class TestExample {
@CommandNamespace("test"):
    // example with no parameters
    @TypedCommand("hello", "Hello, World!", 0, 0)
    string hello() {
        return "Hello, World!";
    }

    // any POD parameter is allowed here (float, string, double, integer, etc)
    // as well, the return type may be any POD
    @TypedCommand("user", "Say hello to someone!", 1, 1)
    string hello_user(string user) {
        return "Hello, " ~ user ~ "!";
    }

    @TypedCommand("meaning_of_life", "What is the meaning of life?", 0, 0)
    int meaningOfLife() {
        return 42;
    }
}
```


then, in the CLI:
```
> test.hello()
Hello, World!
> test.user("Foo")
Hello, Foo!
> test.user(test.hello())
Hello, Hello, World!!
```
(also found in `command:greeter`)
## TODO:
- Proper return types
    - Avoid using strings as a type everywhere
- Small standard library for ease of use
    - No user-defined functions however
        - By design, we're not trying to be a scripting engine
- Well-defined instantiation order
    - In the end, this won't matter (since you can't call *until* the engine has been fully initialized), but might be worth investigating


