# Introduction

PclZip is a PHP Class which creates and manage ZIP formatted archives.

# Understanding how it works

## Internal representation of a PKZIP archive

Each PKZIP archive is represented by an object of class PclZip.
When creating a PclZip archive (on object of class PclZip), the name
of the archive file is associated to the object. At this time the file is not
checked, nor read, it can even don't exist at this time.

```php
require_once('pclzip.lib.php');
$archive = new PclZip("archive.zip");
``` 

The archive is then manipulated by using the public methods
associated with PclZip object. To create an archive, when it does not
already exist, the method ' create()' must be used, with a list of files
and/or folders to archive as parameter.
If the archive already exist, its content can be read by access
methods like ' listContent()' or ' extract()'.

## Parameters and arguments

Each method has its own arguments which are described in the
synopsis ofthe method. These arguments can be mandatory or
optional. Example :

```php
require_once('pclzip.lib.php');
$archive = new PclZip('archive.zip');
$v_list = $archive->add('dev/file.txt',
                        PCLZIP_OPT_REMOVE_PATH, 'dev');
```

Here the first parameter 'dev/file.txt' is mandatory, while
'PCLZIP_OPT_REMOVE_PATH, ...' is optional.
Some methods can also be called only with a variable list of optional
arguments :

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "folder",
                          PCLZIP_OPT_REMOVE_PATH, "data",
                          PCLZIP_CB_PRE_EXTRACT, "callback_pre_extract",
                          PCLZIP_CB_POST_EXTRACT, "callback_post_extract");
```

Here the files are extracted in 'folder', and the 'data' path, when
present in the archive, is removed.
Also, before each extraction of a single file from the archive, a callback function, which name is provided by the user (here
'callback_pre_extract()'), is called. This function gives the ability to
change the path and the filename that is in the extraction process, or
to skip the extraction of this particular file.
At the end of the extraction of the file an other call-back function is
called, giving the ability to the user to perform specific actions on the
file before extracting the next one.

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "folder" ,
                          PCLZIP_OPT_REMOVE_ALL_PATH);
```

Here the files are extracted in the directory 'folder' and all the
memorized path of the files are removed, even if they are different.
By this feature the user does not have to specify the exact value of a
path to remove.
These small examples shows how the variable list of options works.
They offer better features (with the cost of a little more complexity).
They will also ease the introduction of new features without changing
the synopsis of all the methods.
The defined arguments, their usage and restrictions are described in
the section " Optional Variable Parameters". In the description of
each method, the list of available optional parameters are indicated.

## Return values

Return values may be different from methods to methods. They are
described in each method synopsis.
However most of the methods returns 0 on error (and set the error
flag), or an array describing each file on success.
Each entry of this array describe a file or folder, some of its properties
and the specific status of the last action on the file.

Each file is described by the following arguments :

- filename : Name of the file.
For an add, it is the name given when calling
the method.
For extraction it is the real name of the
extracted file (not the name stored in the
archive).
- stored_filename : Stored name of the file.
- size : Real size of the file
- compressed_size : Compressed size in the archive (without header overhead)
- mtime : Last modification date and time of the file
(UNIX timestamp)
- comment : Comments associated with the file.
- folder : true | false : Indicates if the name if the name
of a file or a folder.
- index : Index of the file in the archive (when set).
- content : Content of the extracted file. This row is
present only when the option argument
PCLZIP_OPT_EXTRACT_IN_STRING is set.
- status : Result of the action (depend on the action
type).
Values are :
  - ok : OK !
  - filtered : the file/folder was not
extracted (filtered by the
user).
  - already_a_directory : the file was not
extracted because a
folder with same name
already exists.
  - newer_exist : the file was not
extracted because a file
with same name
already exists and is
write protected.
  - write_protected : the file was not
extracted because a
more recent file exists.
  - path_creation_fail : the file was not
extracted because an
error occurs while
creating its path.
  - write_error : the file was not
extracted because a
write error occur
  - read_error : the file was not
extracted because a read error occur
  - invalid_header : the file was not
extracted because of a
bad header
  - skipped : the file was not
extracted or added
because a user callback
function request to
skip it.
(Introduced in version
1.3)
  - filename_too_long : the file was not added
in the archive because
the filename is too long
(maximum 255
caracters).
(Introduced in version
1.3)



