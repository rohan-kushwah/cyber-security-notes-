## What is a Linux Shell and Why is it Important?

A **shell** is a command-line interpreter program that parses and sends commands to the [operating system](https://phoenixnap.com/glossary/operating-system). This program represents an operating system's interactive interface and the kernel's outermost layer (or shell). It allows users and programs to send signals and expose an operating system's low-level utilities.

![OS layers communication](https://phoenixnap.com/kb/wp-content/uploads/2022/10/os-layers-communication.png)

The terminal program (or terminal emulator) enables interaction with the system's utilities. When we run any command in the terminal, such as [ls](https://phoenixnap.com/kb/linux-ls-commands) or [cat](https://phoenixnap.com/kb/linux-cat-command), the shell parses, evaluates, searches for, and executes the corresponding program, if found.

## Types of Linux Shells

Linux offers different shell types for addressing various problems through unique features. The shells developed alongside Unix often borrowed features from one another as development progressed.

Below is a brief **overview of different shell types and their features**.

### 1. Bourne Shell (sh)

The **Bourne shell** was the first default shell on Unix systems, released in 1979. The shell program name is **sh**, and the traditional location is _/bin/sh_. The prompt switches to **$**, while the root prompt is **#**.

![Bourne shell sh example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/bourne-shell-sh-example-commands.png)

The Bourne shell quickly became popular because it is **compact** and **fast**. However, **sh** lacks some standard features, such as:

- Logical and arithmetic expansion.
- Command history.
- Other comprehensive features, such as autocomplete.

Modern Unix-like systems have the _/bin/sh_ executable file. The program does not start the Bourne shell but acts as an executable file pointing to the **default system shell**.

![/bin/sh link Ubuntu dash CentOS bash](https://phoenixnap.com/kb/wp-content/uploads/2022/10/bin-sh-link-ubuntu-dash-centos-bash.png)

For most systems, the hard or [symbolic link](https://phoenixnap.com/kb/symbolic-link-linux) points to _bash_, while on Ubuntu and Debian, the link is to _dash_. In both cases, the link mimics the Bourne shell as much as possible.

### 2. C Shell (csh)

The **C shell** (**csh**) is a Linux shell from the late 1970s whose main objective was to improve interactive use and mimic the C language. Since the Linux kernel is predominantly written in C, the shell aims to provide stylistic consistency across the system.

The path to the C shell executable is _/bin/csh_. The prompt uses **`%`** for regular users and **`#`** for the root user.

![C shell csh example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/c-shell-csh-example-commands.png)

New interactive features included:

- History of the previous command.
- User-defined aliases for programs.
- Relative home directory (_~_).
- Built-in expression grammar.

The main drawbacks of the C shell are:

- Syntax inconsistencies.
- No support for standard input/output (stdio) file handles or functions.
- Not fully recursive, which limits complex command handling.

The C shell improved readability and performance compared to the Bourne shell. The interactive features and innovations in csh influenced all subsequent Unix shells.

### 3. TENEX C Shell (tcsh)

The **TENEX C shell** (**tcsh**) is an extension of the C shell (**csh**) merged in the early 1980s. The shell is backward compatible with csh, with additional features and concepts borrowed from the TENEX OS.

The TENEX C shell executable path is in _/bin/tcsh_. The user prompt is **`hostname:directory>`** while the root prompt is **`hostname:directory#`**. Early versions of Mac OS and the default root shell of FreeBSD use tcsh.

![TENEX C shell tcsh example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/tenex-c-shell-tcsh-example-commands.png)

Additional features of the shell include:

- Advanced command history.
- Programmable autocomplete.
- Wildcard matching.
- Job control.
- Built-in where command.

Since tcsh is an extension of the C shell, many drawbacks persist in the extended version.

### 4. KornShell (ksh)

The **KornShell** (**ksh**) is a Unix shell and language based on the Bourne shell (sh) developed in the early 1980s. The location is in _/bin/ksh_ or _/bin/ksh93_, while the prompt is the same as the Bourne shell (**`$`** for a user and **`#`** for root).

![Korn shell ksh example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/korn-shell-ksh-example-commands.png)

The shell implements features from the C shell and Bourne shell, aiming to focus on both interactive commands and programming features. The KornShell adds new features of its own, such as:

- Built-in mathematical functions and floating-point arithmetic.
- Object-oriented programming.
- Extensibility of built-in commands.
- Compatible with the Bourne shell.

The shell is faster than both the C shell and the Bourne shell.

### 5. Debian Almquist Shell (dash)

The **Debian Almquist Shell** (**dash**) is a Unix shell developed in the late 1990s from the Almquist shell (ash), which was ported to Debian and renamed.

Dash is famous for being the default shell for Ubuntu and Debian. The shell is minimal and [POSIX](https://phoenixnap.com/glossary/posix) compliant, making it convenient for OS startup scripts.

The executable path is _/bin/dash_, in addition to _/bin/sh_ pointing to _/bin/dash_ on Ubuntu and Debian. The default and root user prompt is the same as in the Bourne shell.

![Debian Almquist shell dash example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/debian-almquist-shell-dash-example-commands.png)

Dash features include:

- Execution speeds up to 4x faster than bash and other shells.
- Requires minimal disk space, CPU, and RAM compared to alternatives.

The main drawback is that dash is not bash-compatible. The features not included in dash are known as "bashisms." Therefore, [bash scripts](https://phoenixnap.com/kb/write-bash-script) require additional reworkings of bashisms to run succesfully.

### 6. Bourne Again Shell (bash)

The [Bourne Again Shell](https://phoenixnap.com/kb/what-is-bash) is a Unix shell and command language created as an extension of the Bourne shell (**sh**) in 1989. The shell program is the default login shell for many Linux distributions and earlier versions of macOS.

The shell name shortens to **bash**, and the location is _/bin/bash_. Like the Bourne shell, the bash prompt is **`$`** for a regular user and **`#`** for root.

![Bourne again shell bash example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/bourne-again-shell-bash-example-commands.png)

Bash introduces features not found in the Bourne shell, some of which include:

- Brace expansion.
- Command completion.
- Basic debugging and signal handling.
- Command history.
- Conditional commands, such as the [bash if](https://phoenixnap.com/kb/bash-if-statement) and [bash case](https://phoenixnap.com/kb/bash-case-statement) statements.
- [Heredoc](https://phoenixnap.com/kb/bash-heredoc) support.

**Note:** Some features are not unique to Bash, but rather borrowed from other shells.

Since bash is a superset of the Bourne shell, most sh scripts execute in bash without any additional changes.

### 7. Z Shell (zsh)

The **Z shell** (**zsh**) is a Unix shell created as an extension for the Bourne shell in the early 1990s. The feature-rich shell borrows ideas from ksh and tcsh to create a well-built and usable alternative.

The executable location is in _/bin/zsh_. The prompt is **`user@hostname location %`** for regular users and **`hostname#`** for the root user. The Z shell is the default shell of Kali Linux and Mac OS.

![Z shell zsh example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/z-shell-zsh-example-commands.png)

Some new features added to the zsh include:

- Shared history among all running shell sessions.
- Improved array and variable handling.
- Spelling corrections and command name autofill.
- Various compatibility modes.
- Extensibility through plugins.

The shell is highly configurable and customizable due to the community-driven support through the [Oh My Zsh](https://ohmyz.sh/) framework.

**Note:** Learn how to [install Zsh in Ubuntu](https://phoenixnap.com/kb/install-zsh-ubuntu), how to [set environmental variables in ZSH](https://phoenixnap.com/kb/zsh-environment-variables) or how to [switch from Zsh to Bash on Mac](https://phoenixnap.com/kb/change-zsh-to-bash-mac).

### 8. Friendly Interactive Shell (fish)

The **Friendly Interactive Shell** (**fish**) is a Unix shell released in the mid-2000s with a focus on usability. The feature-rich shell does not require additional configuration, which makes it user-friendly from the start.

The default executable path is _/usr/bin/fish_. The user prompt is **`user@hostname location>`**, while the root prompt is **`root@hostname location#`**.

![Friendly interactive shell fish example commands](https://phoenixnap.com/kb/wp-content/uploads/2022/10/friendly-interactive-shell-fish-example-commands.png)

Features in the shell include:

- Advanced suggestions/tab completion based on the current directory history.
- [Helpful syntax highlighting](https://phoenixnap.com/glossary/syntax-highlighting) and descriptive error messages.
- Web-based configuration.
- Command history with search options.

The main drawback of fish is non-POSIX compliance. However, the developers aim to improve flawed designs from POSIX.