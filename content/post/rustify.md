---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2016-08-09T00:00:00Z
tags: []
title: Rustify

---

# Setting Up A Windows Workstation for Rust Development

## Getting Rust

Let's start with installing Rust itself.

### Installing the standard way

The [rust-lang.org](https://www.rust-lang.org/en-US/downloads.html) site has the current installers which you can download and install.

### Installing the rustup way

My preferred method is using rustup.rs.

```
invoke-restmethod -usebasicparsing 'https://static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe' -outfile 'rustup-init.exe'
rustup-init.exe
```

That will pick the correct stable build (and allow you to customize) based on your system architecture.  [Read more about rustup.rs here.](https://github.com/rust-lang-nursery/rustup.rs)

### After installing Rust

You'll need to restart your shell for the new path additions to be picked up.

Then you should be able to `rustc --version`.

If you get an error complaining about the lack of msvcr120.dll (for the `stable-x86_64-pc-windows-msvc` target triple) or no output from `rustc --version`, you'll need the [VS 2013 Visual C++ Runtime](https://www.microsoft.com/en-us/download/details.aspx?id=40784) which you can grab from chocolatey.

```
choco install vcredist2013
```

##  Supporting Tools

If you are going to compile rust apps for the Microsoft Visual C++ runtime, you'll need the Visual C++ build tools.  If you have Visual Studio (with the C++ build tools) you are all set.  Otherwise, you'll need the standalone [Visual Studio Build Tools](https://www.microsoft.com/en-us/download/details.aspx?id=48159).  Or via chocolatey - 

```
choco install visualcppbuildtools
```

and if you want the C# and VB.net tools as well, 

```
choco install microsoft-build-tools
```

Now, you are ready to write some rust!

## Working environment

If you did not use `rustup-init` to install your rust environment, you'll want to make sure `%USERPROFILE%/.cargo/bin` is on your path.  Cargo is the package management tool for rust and that folder is where it drops binaries that it installs.

*2/12/2017 UPDATE:* Rusty Code has been languishing.  There is a more actively developed fork - `Rust`, which I'm just starting to try out.

I've become rather fond of Visual Studio Code, so I was happy to find a rust extension - Rusty Code.

Rusty Code (and the newer fork, Rust) works best with a bit of setup.  

* Install rustfmt -  `cargo install rustfmt`
* Install racer - `cargo install racer`
* Install rustsym - `cargo install rustsym`
* Clone a copy of the Rust sources - `git clone https://github.com/rust-lang/rust`
* Set a `CARGO_HOME` enviromental variable - `[Environment]::SetEnvironmentalVariable('CARGO_HOME', "$env:USERPROFILE/.cargo")`
* Add the following settings to your user or workspace preferences in VS Code.  You'll want to change the rust source path to wherever you cloned it.  NOTE: You aren't just setting the path to the cloned repo, but to the src directory in the cloned repo.  And VS Code requires the escaped backslashes.

```
{
    "rust.racerPath": null, // Specifies path to Racer binary if it's not in PATH
    "rust.rustLangSrcPath": "C:\\Users\\Steven\\source\\github\\rust-lang\\rust\\src", // Specifies path to /src directory of local copy of Rust sources
    "rust.rustfmtPath": null, // Specifies path to Rustfmt binary if it's not in PATH
    "rust.rustsymPath": null, // Specifies path to Rustsym binary if it's not in PATH
    "rust.cargoPath": null, // Specifies path to Cargo binary if it's not in PATH
    "rust.cargoHomePath": null, // Path to Cargo home directory, mostly needed for racer. Needed only if using custom rust installation.
    "editor.formatOnSave": true, // Run `rustfmt` on save.
}
```