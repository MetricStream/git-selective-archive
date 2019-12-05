# git-selective-archive
A tool written in Python3 to export a subset of a Git repository to one or more partner.

This is mostly useful for owners of enterprise / closed source repositories who
want to share a subset of the repository with one or more partners.

The subsets are archives created by `git-archive`. Normally, `git-archive`
collect all files of a given ref.  However, this can be controlled by the
attribute `export-ignore`. `git-selective-archive` creates these attributes from
a list of `git-ignore` compliant patterns.  Only files which match the combined
patterns will be included in the archive.

## Example

The following configuration describes 2 partners.  The first one will receive
all Java files with the exception of `foo.java`, and the second one will receive
everything under `/src` with the exception of files under `src/internal`.
```
[partner1]
*.java
!foo.java
[partner2]
src/
!src/internal
```

`git selective-archive --tag release1` will create 2 files:
`partner1_release1.tar.xz` and `partner2_release1.tar.xz` with the matching
files from the tag `release1`.

The default name for the configuration file is `.gitselectivearchive` in the
current directory.  In most cases this should be the toplevel or the work tree
(i.e. next to `.git/`).

## Invocation

`git-selective-archive` understands the following parameters:

 - `--tag`, `-t`: the tag identifying the tree which forms the base of the archive
 - `--format`, `-f`: passed on to `git-archive` to specify the archive format
 - `--config`, `-c`: the name of the configuration file
 - `--repo`, `-r`: the directory of the work tree
 - `--dry-run`, `-n`: shows what would be included in every archive but does not
   create the archives
 - `--verbose`, `-v`: prints debugging information
 - `--help`, `-h`: prints a help message
