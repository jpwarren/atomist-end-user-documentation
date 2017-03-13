# Automate All The Things!

> “If we are engineering processes and solutions that are not automatable,
> we continue having to staff humans to maintain the system. If we have to
> staff humans to do the work, we are feeding the machines with the blood,
> sweat, and tears of human beings.”
>
> *-- Joseph Bironas, Google Site Reliability Engineer*

Programming is exciting and challenging when you’re adding features and making computers do new things.

Programming is boring when you’re:

- Setting up yet another service in your Continuous Integration tools.
- Wrapping retry logic around another HTTP call
- Creating an issue, linking the pull request, closing issues
- Noticing when the build failed and tracking it down
- Tracking down every project that uses this specific library with a bug in it
- Upgrading existing services to the latest coding standards
- Keeping library versions up to date everywhere

Whew! That’s a lot of boring.

What if you could replace every boring task with a few lines of carefully-considered code?

What if that meant never having to do that boring task ever again? Would you do it?

Why wouldn’t you do this?

This is what Atomist is all about.

## Infrastructure Automation Exists

Our friends in infrastructure land have made great progress with automating
tedious tasks&mdash;from the old days of Unix shell scripts to modern tools
like Puppet and Chef&mdash;but development automation hasn’t really progressed
much past continuous integration. That’s a great beginning, but it’s still a
long way from taking care of the majority of the tedious tasks that still exist.

It’s clear that we’re not going to be seeing less automation, either. The push
to move faster, to ship earlier, to continuously improve everything requires
automation to make it possible. Humans have been using machines to make their
lives easier since the first cave-person picked up a stick.

## How Atomist Helps You
Atomist helps you concentrate on the fun, productive kind of development by
taking care of two major classes of tedious work: setting up tools, and making
standard code changes.

## Auto-Wiring
Atomist knows how to connect development tools together so they work the way
you want them to. If you start a new project, Atomist can do all the tedious
setup work for you: creating a repository, adding the right licensing statements,
connecting it into the CI tool, linking it to Slack, etc. All the boilerplate
stuff that you’d normally have to slog through before you even get to writing the fun code.
Or the stuff you usually skip because it’s boring and you want to start coding,
and then you have to fix it all up later when it’s messy and hard.
Atomist makes it easier to get started, and to start off right.

It wasn’t a huge problem in the days of only starting a new project once a year,
but with modern development approaches, new projects are started constantly.
Manual methods simply can’t keep up with the pace required for modern approaches.
Humans aren’t very good at doing exactly the same thing, time after time.

But computers are.

Instead of always having to be perfectly disciplined, Atomist makes it easy to be
the developer you are on your best day. Every day.

[Learn more] about the integrations that Atomist already supports.

## Auto Changes
Setting things up is one thing, but what about the changes that happen in
the middle of projects? Atomist can help you here, too, and in ways that might surprise you.

Consider changing the logging or tracing framework. The end goal is worth it,
but the process of making the change itself is pretty tedious. You need to track
down every reference to the libraries and change them over. You need to make sure
all the calls to the old library are updated to use the new library instead,
possibly changing the syntax of the calls themselves.

Atomist understands code structure and meaning, so it can find and change
references in the right way, across multiple projects, simultaneously.
Atomist doesn’t make typos, or forget a file, or miss that one line in that
module you hardly ever use (and haven’t written a test for yet).
Just ask Atomist to make the changes for you, and voilá!, it’s done,
faster than you could process a pull request.

Imagine being able to constantly roll through library changes to multiple
projects to ensure they all use the correct version. Imagine being able to
find all the code that uses a broken library, and fix it, automatically.

Imagine that you get all this with the full traceability you’d get if a
human developer made the changes by hand: commit comments, 

