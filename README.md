# Git Large File Storage

[![CI status][ci_badge]][ci_url]

[ci_badge]: https://github.com/git-lfs/git-lfs/workflows/CI/badge.svg
[ci_url]: https://github.com/git-lfs/git-lfs/actions?query=workflow%3ACI

[Git LFS](https://git-lfs.github.com) is a command line extension and
[specification](docs/spec.md) for managing large files with Git.

The client is written in Go, with pre-compiled binaries available for Mac,
Windows, Linux, and FreeBSD. Check out the [website](http://git-lfs.github.com)
for an overview of features.

## Getting Started

### Downloading

You can install the Git LFS client in several different ways, depending on your
setup and preferences.

* **Linux users**. Debian and RPM packages are available from
  [PackageCloud](https://packagecloud.io/github/git-lfs/install).
* **macOS users**. [Homebrew](https://brew.sh) bottles are distributed, and can
  be installed via `brew install git-lfs`.
* **Windows users**. Git LFS is included in the distribution of
  [Git for Windows](https://gitforwindows.org/). Alternatively, you can
  install a recent version of Git LFS from the [Chocolatey](https://chocolatey.org/) package manager.
* **Binary packages**. In addition, [binary packages](https://github.com/git-lfs/git-lfs/releases) are
available for Linux, macOS, Windows, and FreeBSD.
* **Building from source**. [This repository](https://github.com/git-lfs/git-lfs.git) can also be
built from source using the latest version of [Go](https://golang.org), and the
available instructions in our
[Wiki](https://github.com/git-lfs/git-lfs/wiki/Installation#source).

### Installing

#### From binary

The [binary packages](https://github.com/git-lfs/git-lfs/releases) include a script which will:

- Install Git LFS binaries onto the system `$PATH`
- Run `git lfs install` to
perform required global configuration changes.

```ShellSession
$ ./install.sh
```

#### From source

- Place the `git-lfs` binary on your system’s executable `$PATH` or equivalent.
- Git LFS requires global configuration changes once per-machine. This can be done by
running:

```ShellSession
$ git lfs install
```

#### Verifying releases

Releases are signed with the OpenPGP key of one of the core team members.  To
get these keys, you can run the following command, which will print them to
standard output:

```ShellSession
$ curl -L https://api.github.com/repos/git-lfs/git-lfs/tarball/core-gpg-keys | tar -Ozxf -
```

Once you have the keys, you can download the `sha256sums.asc` file and verify
the file you want like so:

```ShellSession
$ gpg -d sha256sums.asc | grep git-lfs-linux-amd64-v2.10.0.tar.gz | shasum -a 256 -c
```

## Example Usage

To begin using Git LFS within a Git repository that is not already configured
for Git LFS, you can indicate which files you would like Git LFS to manage.
This can be done by running the following _from within a Git repository_:

```bash
$ git lfs track "*.psd"
```

(Where `*.psd` is the pattern of filenames that you wish to track. You can read
more about this pattern syntax
[here](https://git-scm.com/docs/gitattributes)).

> *Note:* the quotation marks surrounding the pattern are important to
> prevent the glob pattern from being expanded by the shell.

After any invocation of `git-lfs-track(1)` or `git-lfs-untrack(1)`, you _must
commit changes to your `.gitattributes` file_. This can be done by running:

```bash
$ git add .gitattributes
$ git commit -m "track *.psd files using Git LFS"
```

You can now interact with your Git repository as usual, and Git LFS will take
care of managing your large files. For example, changing a file named `my.psd`
(tracked above via `*.psd`):

```bash
$ git add my.psd
$ git commit -m "add psd"
```

> _Tip:_ if you have large files already in your repository's history, `git lfs
> track` will _not_ track them retroactively. To migrate existing large files
> in your history to use Git LFS, use `git lfs migrate`. For example:
>
> ```
> $ git lfs migrate import --include="*.psd" --everything
> ```
>
> For more information, read [`git-lfs-migrate(1)`](https://github.com/git-lfs/git-lfs/blob/master/docs/man/git-lfs-migrate.1.ronn).

You can confirm that Git LFS is managing your PSD file:

```bash
$ git lfs ls-files
3c2f7aedfb * my.psd
```

Once you've made your commits, push your files to the Git remote:

```bash
$ git push origin master
Uploading LFS objects: 100% (1/1), 810 B, 1.2 KB/s
# ...
To https://github.com/git-lfs/git-lfs-test
   67fcf6a..47b2002  master -> master
```

Note: Git LFS requires at least Git 1.8.2 on Linux or 1.8.5 on macOS.

## Limitations

Git LFS maintains a list of currently known limitations, which you can find and
edit [here](https://github.com/git-lfs/git-lfs/wiki/Limitations).

## Need Help?

You can get help on specific commands directly:

```bash
$ git lfs help <subcommand>
```

The [official documentation](docs) has command references and specifications for
the tool.

You can always [open an issue](https://github.com/git-lfs/git-lfs/issues), and
one of the Core Team members will respond to you. Please be sure to include:

1. The output of `git lfs env`, which displays helpful information about your
   Git repository useful in debugging.
2. Any failed commands re-run with `GIT_TRACE=1` in the environment, which
   displays additional information pertaining to why a command crashed.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for info on working on Git LFS and
sending patches. Related projects are listed on the [Implementations wiki
page](https://github.com/git-lfs/git-lfs/wiki/Implementations).
