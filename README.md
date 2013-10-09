rr
==
Bash scripts: useful, fun to write, disorganized, easy to forget, easy to lose. Let's address some negatives!

rr is a compact meta-script, which organizes and runs other scripts:
```bash
# rr
The core /usr/local/libexec/rr module directory does not exist! Try making it.
```

Oops. Okay, let's fix that:
```bash
# mkdir -p /usr/local/libexec/rr
# rr
/usr/local/libexec/rr is empty! Try making a script and/or a dir.
```

Alright, let's make something:
```bash
# echo 'echo hi' > /usr/local/libexec/rr/hello
# rr
hello => No description! Add one by starting a line with ###
```

"hello" is there, but it isn't described:
```bash
# echo '### returns a friendly greeting' >> /usr/local/libexec/rr/hello
# rr
hello => returns a friendly greeting
```

Now let's run it:
```bash
# rr hellp
usage: [debug=1] /usr/local/sbin/rr [module | command]
```

Oops! Typo. Anything invalid yields usage. Let's try again:
```bash
# rr hello
hi
```

Now, with a DEEP DIVE ('bash -x'):
```bash
# debug=1 rr hello
+ echo hi
hi
```

That's basically it. Use dirs to group scripts (dirs can be nested):
```bash
# mkdir /usr/local/libexec/rr/foo
# echo 'echo bar' > /usr/local/libexec/rr/foo/bar
# echo '### returns bar' > /usr/local/libexec/rr/foo/bar
# rr
foo/
hello => returns a friendly greeting
# rr foo
bar => returns bar
# rr foo/bar
bar
```

And so on. While I'd still find rr useful on a single host, it excels on a wider environment: deploy rr to a bindir, and recursively deploy your scripts with something like Puppet. Or store them on a safe, shared directory. Just change the 'moduledir' variable in rr if you don't want to use /usr/local/libexec/rr.

You won't forget your scripts, because they're all in one place. You will know what they do, because they have auto-encouraged one-line descriptions. They'll be widely-available, because they're widely-shared. Cool?
