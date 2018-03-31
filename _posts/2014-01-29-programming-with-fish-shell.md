---
title: Programming with fish shell
image: /assets/img/fishfish_logo2.png
---

[Fish](http://fishshell.com/) is really a good shell that I’m using everyday since two years now. Unfortunately there is not much guides about how to program using it. That’s sad because fish has a lot of advantages over bash to write scripts: cleaner and easier to remember syntax, good variables escaping, etc.. So I decided to write a small tutorial on the subject.

By “programming” I mean making small program that can realize simple mathematical operations, apply algorithms, etc… Yes, as strange as it way seem even when reading the full fish documentation it’s still easier to make such basic things in C rather than fish or bash. That is, in my opinion, because that documentation (which is quite good indeed) mainly speaks about an interactive shell rather than about a programming language. This article aims to correct that point.

To start, install fish, create a small .fish file, put a `#! /usr/bin/fish` on top of it, make it executable and start to read the guide.

## Variables

```
set variable_name insert value here
```

The `set` keyword takes as first argument the name of the variable. All the remaining arguments will constitute its value. If there are multiple ones the resulting variable will be a *list*.

To use that variable, use the `$` prefix:

```
echo $variable_name
```

## Mathematical operations

The dedicated command for mathematical operations in fish is `math`. Example:

```
math "1+2"
# 3
```

`math` concatenates all its arguments with a space between them before evaluating the complete expression. So `math 1+2`, `math 1 + 2` and `math 1+ 2` are all equivalent. But since many operators can also be used in the fish syntax (like <, >, \|, *,…) it is usually easier to put everything in quotes.

`math` supports most C operators and outputs the result on stdout.

To play a little with mathematical operations and variables we will also need to use *command substitution*, which is realized in fish using parentheses:

```
set x 1
set y (math "$x*3")
set y (math "$y+1")
echo $y
# 4
```

## If

The *if* in fish is quite easy to remember compared to bash:

```
if some_command_returning_true_or_false
    echo "command returned true"
else
    echo "command returned false"
end
```

There is no specific delimiter to mark the beginning of command blocks except the carriage return. The end of the structure is marked with the `end` keyword, like with all other command structures.

What is a little bit more tricky is the condition used for the if: it’s a command whose returning status should be 0 (for true) or any other value (for false). Since the `math` keyword returns its value on stdout and not as a status, how to use it with `if`? This is the trick: the returning status of `math` will be false if the calculated value is equal to 0 and true for any other value. So the following code will work:

```
if math "1==1"
    echo "1 equals 1"
else
    echo "1 is not equal to 1"
end
```

Even if it works as expected, there is still one little problem. It will output this when executed:

```
1
1 equals 1
```

That is because the `math` command will perform as it is supposed to and output its value on stdout. To avoid this, we can redirect its output to /dev/null:

```
if math "1==1" > /dev/null
    echo "1 equals 1"
else
    echo "1 is not equal to 1"
end
```

As a side note, the `test` command can also be useful for expression evaluation in `if`. It can perform checks on files and strings.

## While

```
set x 0
while math "$x<10" > /dev/null
    echo $x
    set x (math "$x+1")
end
```

Now that we explained the `if`, the `while` should be quite trivial isn’t it?

## Lists

As explained before, when we assign multiple values to a variable it will create a *list*. The different elements of that list can be accessed using the `[]` syntax:

```
set myvar "apple" "banana" "apple pie"
echo $myvar[1]
# apple
echo $myvar[2]
# banana
echo $myvar[3]
# apple pie
echo $myvar[1..2]
# apple banana
```

When passing a whole list as an argument to a command fish will expand the elements of the list as multiple arguments for the command. This makes fish infinitely superior to bash by making it, at least, somewhat possible to make scripts handling correctly spaces in the file names.

```
set myfiles "file 1.txt" "file 2.txt"
rm $myfiles
# yes, for the first time in the whole Unix history this will work as expected
```

Anyway, to get the size of a list you can use the `count` operation:

```
count $myvar
# 3
```

Lists in fish are technically immutable, but you can construct a new list and assign the new value to the same variable:

```
set myvar $myvar tomato
```

To iterate on list elements you can also use the `for ... in` structure:

```
for el in $myvar
    echo "Element value: $el"
end
#Element value: apple
#Element value: banana
#Element value: apple pie
#Element value: tomato
```

## Parsing command line arguments

When creating complex scripts using fish it can be useful to parse command line arguments. Most tutorials about this using bash will explain the usage of the `getopts` command. That command is specific to bash so it is not usable in fish, fortunately we can use an alternative which is the Unix standard `getopt` command (notice the only difference is the “s”).

Here is a complete example of usage of a command line arguments parsing program in fish. It seems long and boring but once we get used to it it becomes quite fast to write.

```
function help_exit
    echo "Usage:  [options] arguments..."
    echo "Arguments:"
    echo "-a : Do something"
    echo "-d : Do something else"
    echo "-c stuff : Do someting with stuff"
    exit 1
end

set args (getopt -s sh abc: $argv); or help_exit
set args (fish -c "for el in $args; echo \$el; end")

set i 1
while true
    switch $args[$i]
        case "-a"
            echo "argument a is specified"
        case "-b"
            echo "argument b is specified"
        case "-c"
            set i (math "$i + 1")
            echo "value of argument c is" $args[$i]
        case "--"
            break
    end
    set i (math "$i + 1")
end
set pargs
if math "$i <" (count $args) > /dev/null
    set pargs $args[(math "$i + 1")..-1]
end

echo "positional arguments:" $pargs
```

The most interesting part of this example is the call to `getopt`. `getopt`‘s first positional argument is the options specifier (here `abc:`). That options specifier is a list of characters where each character is a possible option for our program (only short options with only one character are supported with this usage). If an option is followed by a colon (;) it means that option takes a parameter. The rest of the positional arguments for `getopt` are the arguments to parse (in fish it’s the `$argv` list).

The whole purspose of `getopt` is to analyze Unix-style command line arguments and rewrite them in a more standardized format which is easy to parse with a simple `while`. By using the `-s sh` option of `getopt` and using a little trick by invoking a new fish interpreter I create a nice list where each arguments are clearly indicated. As example, if I use these arguments:

```
-ac tmp.fs hello world
```

I will obtain these elements in `args`:

```
"-a" "-c" "tmp.fs" "--" "hello" "world"
```

`getopt` was smart enough separate the `-a` and `-c` arguments and determine that `tmp.fs` was a parameter of `-c` instead of a positional argument. `--` always indicate the beginning of the positional arguments. `getopt` also return false if it detected an error during the parsing, in which case we generally exit the program after printing the help.

## The End

That’s all for now, if you have some ideas about additional stuff to explain in this guide don’t hesitate to post some comments. Just for fun, here is an implementation of 99 bottles of beer on the wall in fish:

```
#! /usr/bin/fish

for quant in (seq 99 -1 1)
    if math "$quant > 1" > /dev/null
        echo "$quant bottles of beer on the wall, $quant bottles of beer."
        if math "$quant > 2" > /dev/null
            set suffix (math "$quant - 1")
            set suffix "$suffix bottles of beer on the wall."
        else
            set suffix "1 bottle of beer on the wall."
        end
    else if math "$quant == 1" > /dev/null
        echo "1 bottle of beer on the wall, 1 bottle of beer."
        set suffix "no more beer on the wall!"
    end
    echo "Take one down, pass it around, $suffix"
    echo "--"
end
```
