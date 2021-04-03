# Linux File Attributes Made Easy

Linux provides us the access control by file and directory permissions on three levels which are user, group and other. These file permissions provide the basic level of security and access control.

Linux also has advanced access control features like ACLs (Access Control Lists) and attributes. Attributes define properties of files. In Linux, file attributes are meta-data properties that describe the file’s behavior. For example, an attribute can indicate whether a file is compressed or specify if the file can be deleted. Some attributes like immutability can be set or cleared, while others like encryption are read-only and can only be viewed. The support for certain attributes depends on the filesystem being used.

### Attributes in Linux

`a` - **append only**: this attribute allows a file to be added to, but not to be removed. It prevents accidental or malicious changes to files that record data, such as log files.

`c` - **compressed**: it causes the kernel to compress data written to the file automatically and uncompress it when it's read back.

`d` - **no dump**: it makes sure the file is not backed up in backups where the `dump` utility is used

`e` - **extent format**: it indicates that the file is using extents for mapping the blocks on disk.

`i` - **immutable**: it makes a file immutable, which goes a step beyond simply disabling write access to the file. The file can't be deleted, links to it can't be created, and the file can't be renamed.

`j` - **data journaling**: it ensures that on an Ext3 file system the file is first written to the journal and only after that to the data blocks on the hard disk.

`s` - **secure deletion**: it makes sure that recovery of a file is not possible after it has been deleted.

`t` - **no tail-merging**: Tail-merging is a process in which small data pieces at a file's end that don't fill a complete block are merged with similar pieces of data from other files.

`u` - **undeletable**: When a file is deleted, its contents are saved which allows a utility to be developed that works with that information to salvage deleted files.

`A` - **no atime updates**: Linux won't update the access time stamp when you access a file.

`D` - **synchronous directory updates**: it makes sure that changes to files are written to disk immediately, and not to cache first.

`S` - **synchronous updates**: the changes on a file are written synchronously on the disk.

`T` - **top of directory hierarchy**: A directory will be deemed to be the top of directory hierarchies for the purposes of the Orlov block allocator.

### List attributes using `lsattr` command

**Syntax**

```
lsattr [ -RVadv ] [ files...  ]

-R     Recursively list attributes of directories and their contents.
-V     Display the program version.
-a     List all files in directories, including files that start with `.'.
-d     List directories like other files, rather than listing their contents.
-v     List the file's version/generation number.
```

**Examples**

```
$ lsattr .
 -----a-----------e- ./file1
 ----i------------e- ./hello_dir
 -----------------e- ./usrcopy
```

```
lsattr -v .
18446744071799875138 -------------e-- ./requirements.yml
18446744071799842965 -------------e-- ./builds
18446744071799887599 -------------e-- ./docker-compose.yml
18446744071799811364 -------------e-- ./tower.pem
```

### Change attributes using `chattr` command

**Syntax**

```
chattr [ -RVf ] [ -v version ] [ mode ] files...

mode   +-=[aAcCdDeijsStTu]
-R     Recursively change attributes of directories and their contents.
-V     Be verbose with chattr's output and print the program version.
-f     Suppress most error messages.
-v     Set the file's version/generation number.
```

**Examples**

```
$ sudo chattr +i todo.txt
```

```
$ sudo chattr +iA todo.txt
```

```
$ sudo chattr -i todo.txt
```

```
$ sudo chattr "=e" todo.txt
```
