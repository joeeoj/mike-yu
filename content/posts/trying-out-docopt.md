+++
author = "Mike Yu"
title = "trying out docopt"
slug = "trying-out-docopt"
date = "2022-02-10"
description = "swapping argparse for docopt"
tags = [
    "python",
    "docopt",
    "CLI",
]
+++

# trying out docopt

In my last post I created a very small command line interface (CLI) using argparse, which is a built in Python module. This time around I'm going to use the same code as before but instead of argparse I'll use a third-party module calle docopt.

I got this idea from a great talk called From Python script to Open Source Project by Michał Karzyński. It has a lot of great content about putting together a fully-fledged open source Python project and I recommend watching the whole thing. At the beginning he talks about using docopt for generating a command line interface. I remember trying to use it in the past but at the time I didn't have enough familiarity with CLI conventions to understand how to use it. In the time since I've had more practice with unix tools and creating my own so I think it's worth another shot.

## docopt

docopt is actually a collection of tools in different languages, all with the same concept -- you write the help message for your CLI and docopt parses that for you into a dictionary/map that you then use to stitch everything together. It enables you to think at a "higher-level" and construct the final help message first as opposed to argparse where you need to declaratively create each part of the interace and then it generates the help message for you.

It's worth noting that both docopt and argparse have essentially the same end result, which is a dictionary-like object of your parsed CLI elements that you still have to use to get your code working as desired. So they are kind of the inverse of one another in how they get to the same result. As an aside, these are different from something like Typer which does both the parsing and stitching together for you.

As I mentioned above, I think docopt requires more familiarity with CLI syntax and in the blog post he wrote to go with the video, Michał links to a page with GNU command line syntx conventions. It is a helpful reference but nothing replaces getting experinece with real-world unix tools and looking at their help messages.

### maintenance concerns

Unfortunately it does not look like the Python version of docopt is currently being maintained. As of writing this post the last merge commit was on Aug 27, 2018 and there are 39 open PRs and 215 open issues. On one hand the package itself is limited in scope so it shouldn't need that much support, but it feels weird relying on code that hasn't been updated in a few years.

### jazzband to the rescue

docopt-ng is a fork of docopt by jazzband and was last updated in Oct 2021, including merging a lot of those 39 open PRs. It has the same interface as the original so it's a drop-in solution. Just make sure to `pip uninstall docopt` if you already have the original installed. I used this version for my testing but for a limited usecase like mine, either the original or fork should work.

By the way, here's some more info on jazzband from their website. They have a lot of projects, mostly Django-related but not exclusively.

> Jazzband is a collaborative community to share the responsibility of maintaining Python-based projects

> The purpose of the Jazzband is to help Open Source projects on GitHub share responsibility for software maintenance. It's essentially a GitHub organization that is open to everyone who is willing and able to maintain its projects. It was born out of necessity of expanding the group of maintainers of some projects whose original authors weren't able to effectively manage them when people volunteered to help out.

## anatomy of a CLI

I'm going to go over some helpful CLI terminology using the docopt docstring I wrote for the awscp script. This is the majority of work we need to do to get our script to work with docopt.

```
"""Copy files on AWS S3

Usage: awscpv2 <from-loc> <to-loc> [--dryrun --single-file --clipboard]

Options:
    -h --help       Show this screen.
    --dryrun        Dryrun of the generated S3 copy command (doesn't actually execute)
    --single-file   Copy a single file, otherwise recursive copy is assumed
    --clipboard     Copy S3 command to clipboard instead of running it

"""
```

* The very top of the docstring is a short description of the script
* Usage - this can be all in one line for a short script or split onto multiple lines
    * The first thing after the colon is the name of the script itself. If it ended in .py you would include that as well. In this case I removed the extension similar to how I did with the original awscp script, using the shebang to indicate that it should be executed by Python
* arguments - Required inputs from the user, in this case `<from-loc>` and `<to-loc>`
    * for docopt they are defined by surrounding angle brackets `<input>`
    * or can be defined using all uppercase `INPUT`
* options - These are (normally) optional inputs. You can make them required but that breaks the convention
    * They can be boolean flags or take their own arguments of different data types
    * They are denoted with a leading `-` for the short name and `--` for the long name, i.e. `-f` and `--filename` refer to the same option
    * boolean example: If the user provides `--flag` it means true which means the default value, when not given, is false
        * argparse will let you flip this and have a default value of true
        * you can do this in docopt like this: `not arg.get('--option')`
    * int example: `--num=5`
    * string example: `--filename=data.csv`
* commands - commands are words that aren't defined as arguments or options, also called sub-commands
    * These are usually used for more complex scripts. For example in `$ git checkout -B new-branch` the checkout component is a command
    * `[-]` and `[--]` are also two special commands (more on this in the docopt readme)
    * I don't have an example of this in my script but the docopt readme has examples
* `|` - mutually exclusive options, meaning the user can only provide one but not multiple
* `[]` - brackets in a help message enclose elements that are optional
    * pretty much all of the time your options will go in these
* `()` - parenthesis enclose anything that is required
    * also anything not in `[]` is also considered required
    * I think the convention of only enclosing the optional elements in `[]` and leaving everything else not enclosed, and therefore required, makes more sense. But there are circumstances when you might want the enclosing `()` to be more clear, like required arguments for commands

### using the parsed args

Here are the remaining changes to the script. We remove out all of the argparse code and replace it with a single line for parsing the docstring using docopt. The other differences are using get accessors instead of dot notation. I also upacked `from-loc` and `to-loc` into variables as a convenience to manage line lengths.

```python
if __name__ == '__main__':
    args = docopt(__doc__, version='awscp 2.0')
    from_loc, to_loc = args.get('<from-loc>'), args.get('<to-loc>')

    if args.get('--clipboard'):
        pyperclip.copy(cp_files(from_loc, to_loc, args.get('--dryrun'), args.get('--single-file'), run=False))
    else:
        cp_files(from_loc, to_loc, args.get('--dryrun'), args.get('--single-file'))
```

## things of note

### single vs double dash

I usually just use the `--` double dash long name convention when creating tools for myself. I think it is more readable and easier to remember. That said adding a single dash option is easy as long as you don't have name collisions.

### accessors

Argparse allows you to access values using the dot accessor convention, such as `args.from_loc`. You can do the same for docopt (for valid names with no spaces or hyphens) but the convention in the readme is to use the get accessor for everything like this: `args.get('<from-loc>)`. This is more verbose but has the benefit of making it very clear in your code which parts of the CLI are being accessed because you use the exact same syntax as in the docstring itself. In other words if you assign `args.get('--clipboard')` to a variable, you know immediately it is an option based on the leading double dashes whereas you know `args.get('<to-loc>')` is an argument.

### underscores vs hyphens

You might have noticed I changed argument names from underscores to hyphens (i.e. `from_loc` to `<from-loc>`). The GNU convention is to use hyphens but that causes problems for argparse so I break the convention and stick with underscores when using it. This is not an issue with docopt, however, because of the get accessor convention mentioned above.

## final thoughts

Will I use docopt again? Yeah I think so.

I like how lightweight it feels and it gives me a good feeling to have a useful docstring. It does require you to be more familiar with CLI idioms and conventions which is not a bad thing but it is a barrier to entry, or at least it was for me. I also worry about scaling issues with docopt since I don't know it as well as something like argparse, which has a lot of functionality for increased complexity.

That said, most great CLIs are easy to use because they have a single focus and use common sense defaults. That's my biggest takewawy from using docopt, actually. Most of the time you need to quickly create a simple tool that accomplishes one thing and maybe gives you a few configuration options. For those common use cases a simple help message should be all you need to define your interface.

## links

* [argparse Python docs](https://docs.python.org/3/library/argparse.html)
* [Typer website](https://typer.tiangolo.com/)
* [From Python script to Open Source Project](https://www.youtube.com/watch?v=25P5apB4XWM) -- YouTube video by Michał Karzyński (10 out of 10)
* [Python project maturity checklist](https://michal.karzynski.pl/blog/2019/05/26/python-project-maturity-checklist/) - Blog post that goes with the video above
* [GNU CLI syntax conventions](https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html) - link mentioned in the blog post
* [docopt/docopt](https://github.com/docopt/docopt) - original Python docopt repo
* [docopt website](http://docopt.org/)
* [jazzband/docopt-ng](https://github.com/jazzband/docopt-ng) - docopt-ng fork
* [jazzband website](https://jazzband.co/)
