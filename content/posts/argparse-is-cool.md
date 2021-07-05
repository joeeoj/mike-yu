+++
author = "Mike Yu"
title = "argparse is cool"
slug = "argparse-is-cool"
date = "2021-07-05"
description = "argparse is good for making quick, custom CLI tools."
tags = [
    "python",
    "argparse",
    "CLI",
]
+++

# argparse is cool

I need to copy files between S3 buckets on a regular basis for my job. The aws command line tool is great but I forget flags often...and usually the same ones over and over. What if I made a custom version of `aws s3 cp` that was simpler and fit the majority of my copy usecases?

Well, here it is in 43 lines (with proper spacing) using argparse:

```python
#!/usr/bin/env python3
import argparse
import os
import subprocess
from typing import Optional

import pyperclip


AWS_PROFILE = os.getenv('AWS_PROFILE')


def cp_files(from_loc: str, to_loc: str, dryrun: bool, single_file: bool, run: bool = True) -> Optional[str]:
    flags = '--acl bucket-owner-full-control'  # always want this

    if AWS_PROFILE:  # want this if it is set as an env var
        flags += f' --profile {AWS_PROFILE}'
    if dryrun:
        flags += ' --dryrun'
    if not single_file:  # assume recursive unless told otherwise
        flags += ' --recursive'

    cmd = f'aws s3 cp {from_loc} {to_loc} {flags}'

    if run:
        subprocess.run(cmd, shell=True)
    else:
        return cmd


if __name__ == '__main__':
    parser = argparse.ArgumentParser(prog='awscp', description='Copy S3 files with acl, profile, and recursive flags added automatically')
    parser.add_argument('from_loc', help='S3 path to copy from')
    parser.add_argument('to_loc', help='S3 path to copy to')
    parser.add_argument('--dryrun', help='dryrun the S3 copy', action='store_true')
    parser.add_argument('--single_file', help='Copy a single file, otherwise assumes recursive copy', action='store_true')
    parser.add_argument('--clipboard', help='Copy command to clipboard instead of running', action='store_true')
    args = parser.parse_args()

    if args.clipboard:  # helpful if you want to Slack the command to someone
        pyperclip.copy(cp_files(args.from_loc, args.to_loc, args.dryrun, args.single_file, run=False))
    else:
        cp_files(args.from_loc, args.to_loc, args.dryrun, args.single_file)
```

The cool thing about argparse is that, as long as you fill in the program description and help for each argument, you get all of this help output basically for free:

```
$ awscp -h
usage: awscp [-h] [--dryrun] [--single_file] [--clipboard] from_loc to_loc

Copy S3 files with acl, profile, and recursive flags added automatically

positional arguments:
  from_loc       S3 path to copy from
  to_loc         S3 path to copy to

optional arguments:
  -h, --help     show this help message and exit
  --dryrun       dryrun the S3 copy
  --single_file  Copy a single file, otherwise assumes recursive copy
  --clipboard    Copy command to clipboard instead of running
```

## a few follow up things

* put your CLI scripts in a single folder and version control it if you can. I use `/usr/local/scripts`
* add the scripts folder to your path by adding an export line to your .bashrc/.zshrc/etc. This will let you run the script from any folder.
    * `export PATH="/usr/local/scripts:${PATH}"`
* save the script without the .py extension to make it look more unix-like. I saved this script as `awscp`
* put a shebang as the first line of your script so it is correctly executed as a python program, even without the .py extension -- `#!/usr/bin/env python3`
* don't forget to make your script executable with chmod -- `$ chmod +x awscp`
* also don't name your script the same as an existing shell script unless you want to confuse yourself

### other Python CLI libraries

There are other cool libraries for making Python CLIs. [Typer](https://typer.tiangolo.com/) seems especially cool and I'll learn how to use it some day but argparse is what I know now. Use the tool you know when it's crunchtime and try to use the tool you want to know if you have some extra time to do a task.

## summary

The next time you find yourself copying a semi-complex CLI command with multiple flags into Evernote to remember it for the next time, stop and see if you can instead simplify it with your own version of the tool.

## links

* [argparse documentation](https://docs.python.org/3/library/argparse.html)
* [AWS CLI homepage](https://aws.amazon.com/cli/)
* [Shebang wikipedia article](https://en.wikipedia.org/wiki/Shebang_(Unix))
* [Great Stackoverflow answer on the correct Python shebang to use](https://stackoverflow.com/a/19305076)
* the code above can be found in the code folder of the [source code for this website](https://github.com/joeeoj/mike-yu)
