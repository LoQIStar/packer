---
layout: docs
sidebar_current: docs-commands
page_title: Commands
description: |-
  Packer is controlled using a command-line interface. All interaction with
  Packer is done via the `packer` tool. Like many other command-line tools, the
  `packer` tool takes a subcommand to execute, and that subcommand may have
  additional options as well. Subcommands are executed with `packer SUBCOMMAND`,
  where "SUBCOMMAND" is the actual command you wish to execute.
---

# Packer Commands (CLI)

Packer is controlled using a command-line interface. All interaction with Packer
is done via the `packer` tool. Like many other command-line tools, the `packer`
tool takes a subcommand to execute, and that subcommand may have additional
options as well. Subcommands are executed with `packer SUBCOMMAND`, where
"SUBCOMMAND" is the actual command you wish to execute.

If you run `packer` by itself, help will be displayed showing all available
subcommands and a brief synopsis of what they do. In addition to this, you can
run any `packer` command with the `-h` flag to output more detailed help for a
specific subcommand.

In addition to the documentation available on the command-line, each command is
documented on this website. You can find the documentation for a specific
subcommand using the navigation to the left.

## Machine-Readable Output

By default, the output of Packer is very human-readable. It uses nice
formatting, spacing, and colors in order to make Packer a pleasure to use.
However, Packer was built with automation in mind. To that end, Packer supports
a fully machine-readable output setting, allowing you to use Packer in automated
environments.

The machine-readable output format is easy to use and read and was made with
Unix tools in mind, so it is awk/sed/grep/etc. friendly.

### Enabling Machine-Readable Output

The machine-readable output format can be enabled by passing the
`-machine-readable` flag to any Packer command. This immediately enables all
output to become machine-readable on stdout. Logging, if enabled, continues to
appear on stderr. An example of the output is shown below:

```text
$ packer -machine-readable version
1376289459,,version,0.2.4
1376289459,,version-prerelease,
1376289459,,version-commit,eed6ece
1376289459,,ui,say,Packer v0.2.4.dev (eed6ece+CHANGES)
```

The format will be covered in more detail later. But as you can see, the output
immediately becomes machine-friendly. Try some other commands with the
`-machine-readable` flag to see!

~> The `-machine-readable` flag is designed for automated environments and is
mutually-exclusive with the `-debug` flag, which is designed for interactive
environments.

### Format for Machine-Readable Output

The machine readable format is a line-oriented, comma-delimited text format.
This makes it extremely easy to parse using standard Unix tools such as awk or
grep in addition to full programming languages like Ruby or Python.

The format is:

```text
timestamp,target,type,data...
```

Each component is explained below:

- `timestamp` is a Unix timestamp in UTC of when the message was printed.

- `target` is the target of the following output. This is empty if the message
  is related to Packer globally. Otherwise, this is generally a build name so
  you can relate output to a specific build while parallel builds are running.

- `type` is the type of machine-readable message being outputted. There are a
  set of standard types which are covered later, but each component of Packer
  (builders, provisioners, etc.) may output their own custom types as well,
  allowing the machine-readable output to be infinitely flexible.

- `data` is zero or more comma-separated values associated with the prior type.
  The exact amount and meaning of this data is type-dependent, so you must read
  the documentation associated with the type to understand fully.

Within the format, if data contains a comma, it is replaced with
`%!(PACKER_COMMA)`. This was preferred over an escape character such as `\'`
because it is more friendly to tools like awk.

Newlines within the format are replaced with their respective standard escape
sequence. Newlines become a literal `\n` within the output. Carriage returns
become a literal `\r`.

### Machine-Readable Message Types

The set of machine-readable message types can be found in the
[machine-readable format](/docs/commands/index.html) complete
documentation section. This section contains documentation on all the message
types exposed by Packer core as well as all the components that ship with
Packer by default.
