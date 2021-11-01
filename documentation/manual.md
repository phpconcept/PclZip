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


## Class and Methods

- [PclZip::PclZip()](class_and_methods.md#method-pclzippclzip) : Class creator  
- [PclZip::create()](class_and_methods.md#method-pclzipcreate) : Create the PKZIP file and add files or folders  
- [PclZip::listContent()](class_and_methods.md#method-pclziplistcontent) : List content of an archive  
- [PclZip::extract()](class_and_methods.md#method-pclzipextract) : Extract all or part of the content of the archive  
- [PclZip::properties()](class_and_methods.md#method-pclzipproperties) : Get properties of the archive  
- [PclZip::add()](class_and_methods.md#method-pclzipadd) : Get properties of the archive  
- [PclZip::delete()](class_and_methods.md#method-pclzipdelete) : Delete files inside the archive  
- [PclZip::merge()](class_and_methods.md#method-pclzipmerge) : Add one archive content in a second archive  
- [PclZip::duplicate()](class_and_methods.md#method-pclzipduplicate) : Duplicate the archive  



## Return values

Return values may be different from methods to methods. They are
described in each method synopsis.
However most of the methods returns 0 on error (and set the error
flag), or an array describing each file on success.
Each entry of this array describe a file or folder, some of its properties
and the specific status of the last action on the file.

Each file is described by the following arguments :

- filename : Name of the file. For an add, it is the name given when calling the method. For extraction it is the real name of the extracted file (not the name stored in the archive).
- stored_filename : Stored name of the file.
- size : Real size of the file
- compressed_size : Compressed size in the archive (without header overhead)
- mtime : Last modification date and time of the file (UNIX timestamp)
- comment : Comments associated with the file.
- folder : true | false : Indicates if the name if the name of a file or a folder.
- index : Index of the file in the archive (when set).
- content : Content of the extracted file. This row is present only when the option argument PCLZIP_OPT_EXTRACT_IN_STRING is set.
- status : Result of the action (depend on the action type).
Values are :
  - ok : OK !
  - filtered : the file/folder was not extracted (filtered by the user).
  - already_a_directory : the file was not extracted because a folder with same name already exists.
  - newer_exist : the file was not extracted because a file with same name already exists and is write protected.
  - write_protected : the file was not extracted because a more recent file exists.
  - path_creation_fail : the file was not extracted because an error occurs while creating its path.
  - write_error : the file was not extracted because a write error occur
  - read_error : the file was not extracted because a read error occur
  - invalid_header : the file was not extracted because of a bad header
  - skipped : the file was not extracted or added because a user callback function request to skip it. (Introduced in version 1.3)
  - filename_too_long : the file was not added in the archive because the filename is too long (maximum 255 caracters). (Introduced in version 1.3)



## Optional Arguments


### Overview

The optional arguments can be separated in two main families. The first one are classical arguments that give informations or instructions to the method. The second one are call-back functions, hooks that give the user the ability to perform specific actions during PclZip processing. "Call-backs" are more complex to understand, but gives a better control of archived files.



The optional arguments are identified by a name, which is in reality a static integer value. The value of the argument can be a single value or a list of values. In some cases they don't take any value, their name is enough to indicate a specific action to the method.



In the following sections we will describe the arguments of the two families.



### Optional arguments


Today the defined optional arguments are :

>  [PCLZIP_OPT_PATH](optional_arguments.md#argument-pclzip_opt_path)  
>  [PCLZIP_OPT_ADD_PATH](optional_arguments.md#argument-pclzip_opt_add_path)  
>  [PCLZIP_OPT_REMOVE_PATH](optional_arguments.md#argument-pclzip_opt_remove_path)  
>  [PCLZIP_OPT_REMOVE_ALL_PATH](optional_arguments.md#argument-pclzip_opt_remove_all_path)  
>  [PCLZIP_OPT_SET_CHMOD](optional_arguments.md#argument-pclzip_opt_set_chmod)  
>  [PCLZIP_OPT_BY_NAME](optional_arguments.md#argument-pclzip_opt_by_name)  
>  [PCLZIP_OPT_BY_EREG](optional_arguments.md#argument-pclzip_opt_by_ereg)  
>  [PCLZIP_OPT_BY_PREG](optional_arguments.md#argument-pclzip_opt_by_preg)  
>  [PCLZIP_OPT_BY_INDEX](optional_arguments.md#argument-pclzip_opt_by_index)  
>  [PCLZIP_OPT_EXTRACT_AS_STRING](optional_arguments.md#argument-pclzip_opt_extract_as_string)  
>  [PCLZIP_OPT_EXTRACT_IN_OUTPUT](optional_arguments.md#argument-pclzip_opt_extract_in_output)  
>  [PCLZIP_OPT_NO_COMPRESSION](optional_arguments.md#argument-pclzip_opt_no_compression)  
>  [PCLZIP_OPT_COMMENT](optional_arguments.md#argument-pclzip_opt_comment)  
>  [PCLZIP_OPT_ADD_COMMENT](optional_arguments.md#argument-pclzip_opt_add_comment)  
>  [PCLZIP_OPT_PREPEND_COMMENT](optional_arguments.md#argument-pclzip_opt_prepend_comment)  
>  [PCLZIP_OPT_REPLACE_NEWER](optional_arguments.md#argument-pclzip_opt_replace_newer)  
>  [PCLZIP_OPT_EXTRACT_DIR_RESTRICTION](optional_arguments.md#argument-pclzip_opt_extract_dir_restriction)  
>  [PCLZIP_OPT_STOP_ON_ERROR](optional_arguments.md#argument-pclzip_opt_stop_on_error)  
>  [PCLZIP_OPT_TEMP_FILE_ON](optional_arguments.md#argument-pclzip_opt_temp_file_on)  
>  [PCLZIP_OPT_TEMP_FILE_THRESHOLD](optional_arguments.md#argument-pclzip_opt_temp_file_threshold)  
>  [PCLZIP_OPT_TEMP_FILE_OFF](optional_arguments.md#argument-PCLZIP_OPT_TEMP_FILE_OFF)



### "Call-back" functions


'Call-back' functions are specific arguments, because the value is a function name. The call-back function have a strict synopsis that must be respected, and the possible actions inside it are delimited. The algorithm inside the call-back function can be anything, it must respect the argument list and the returned value. However the call-back function must respect the core method processing, some actions may interfere with it (like deleting the archive file during process).


>  [PCLZIP_CB_PRE_EXTRACT](optional_arguments.md#call-back-argument-pclzip_cb_pre_extract)  
>  [PCLZIP_CB_POST_EXTRACT](optional_arguments.md#call-back-argument-pclzip_cb_post_extract)  
>  [PCLZIP_CB_PRE_ADD](optional_arguments.md#call-back-argument-pclzip_cb_pre_add)  
>  [PCLZIP_CB_POST_ADD](optional_arguments.md#call-back-argument-pclzip_cb_post_add)


## Error Handling

Since version 1.3, error handling is integrated inside the PclZip class. Before 1.3 the error handling was done by an external library. The main interest to do that is to be able to distribute PclZip as a single file. It is still possible to use the external error handling (see "[Customize PclZip](\"http://www.phpconcept.net/pclzip/man/en/index.php?errors#customize\")" section).

When a method returns an error code (most of the methods returns 0 on error), the error code, name and sometimes additional informations are available through specific methods :

> -  errorName() : Returns a string containing the name of the error.  
> -  errorCode() : Returns the error code value.  
> -  errorInfo() : Returns a description associated with the error.

Error handling samples :

-  Error code reading

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");  
if ($list == 0) {  
  die("Unrecoverable error, code ".$archive->errorCode());  
}    
Unrecoverable error, code -6  
```

-  Error name reading

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");  
if ($list == 0) {  
  die("Unrecoverable error '".$archive->errorName(). "'");  
}    
Unrecoverable error 'PCLZIP_ERR_BAD_FORMAT'  
```

-  Error name and code reading

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");  
if ($list == 0) {  
  die("Unrecoverable error '".$archive->errorName(true)."'");  
}  
   
Unrecoverable error 'PCLZIP_ERR_BAD_FORMAT(-10)'  
```

-  Error description reading

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");  
if ($list == 0) {  
  die("Error : '".$archive->errorInfo()."'");  
}  
   
Error : 'Invalid archive structure \[code -10\]'  
```
-  Full error description reading

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");  
if ($list == 0) {  
  die("Error  : '".$archive->errorInfo(true)."'");  
}  
   
Error : 'PCLZIP_ERR_BAD_FORMAT(-10) : Invalid archive structure'  
```


## Customizing PclZip

PclZip can be customized by configuring static constants. It is recommended that you do not change these parameters, unless this is really necessary and that you understand exactly what you are doing.  
Moreover when you update PclZip to a new release you will have to manually reconfigure these parameters.

The customizable values are :

> -  [PCLZIP_READ_BLOCK_SIZE](\"#parameter_PCLZIP_READ_BLOCK_SIZE\")  
> -  [PCLZIP_TEMPORARY_DIR](\"#parameter_PCLZIP_TEMPORARY_DIR\")  
> -  [PCLZIP_SEPARATOR](\"#parameter_PCLZIP_SEPARATOR\")  
> -  [PCLZIP_TEMPORARY_FILE_RATIO](\"#parameter_PCLZIP_TEMPORARY_FILE_RATIO\")  
> -  [PCLZIP_ERROR_EXTERNAL](\"#parameter_PCLZIP_ERROR_EXTERNAL\")

 -  PCLZIP_READ_BLOCK_SIZE

> This value is the size of the data block that will be read or wirte while accessing a file. It can be interesting to change this value if you know the optimized value of file access associated with your system.  
> The default value is 2048.

 -  PCLZIP_TEMPORARY_DIR

> When compressing or extracting files, PclZip might use temporary files. These files are created then destroyed by PclZip. By default these temporary files are created in the current working folder. This can be a problem in some situation (for example if the folder is read-only).  
> This customizable value allow for the definition of a static temporary folder that will be used for this working file access. The temporary folder must be read/write accessible and must exist (PclZip will not create it if missing). It is recommended to use a absolute path definition from the filesystem (and avoid relative path from web root or include folders).  
> The folder path MUST finish by '/'.  
> Exemple
> 
```php
define('PCLZIP_TEMPORARY_DIR', '/usr/www/temp/');  
include_once('pclzip.lib.php');  
```

-  PCLZIP_SEPARATOR

> In version 1.x of PclZip, the separator for file list is a space (which is not a very smart choice, specifically for windows paths !). A better separator should be a comma (,). This constant gives you the abilty to change that. However notice that changing this value, may have impact on existing scripts, using space separated filenames.  
> Recommended value for compatibility with older versions :  
> define( 'PCLZIP_SEPARATOR', ' ' );  
> Default value for smart separation of filenames :  
> define( 'PCLZIP_SEPARATOR', ' ' );
> 
```php
define('PCLZIP_SEPARATOR', ';');  
include_once('pclzip.lib.php');  
```

-  PCLZIP_TEMPORARY_FILE_RATIO

> Optional threshold ratio for use of temporary files
> 
> Pclzip sense the size of the file to add/extract and decide to use or not temporary file. The algorythm is looking for memory_limit of PHP and apply a ratio.  
>   threshold = memory_limit \* ratio.  
> Recommended values are under 0.5. Default 0.47.

-  PCLZIP_ERROR_EXTERNAL

> Deprecated parameter used in the past to enable an external error library not anymore distributed with PclZip.



