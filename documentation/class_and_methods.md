
# PclZip User Guide - Class and Methods

- [PclZip::PclZip()](class_and_methods.md#method-pclzippclzip) : Class creator  
- [PclZip::create()](class_and_methods.md#method-pclzipcreate) : Create the PKZIP file and add files or folders  
- [PclZip::listContent()](class_and_methods.md#method-pclziplistcontent) : List content of an archive  
- [PclZip::extract()](class_and_methods.md#method-pclzipextract) : Extract all or part of the content of the archive  
- [PclZip::properties()](class_and_methods.md#method-pclzipproperties) : Get properties of the archive  
- [PclZip::add()](class_and_methods.md#method-pclzipadd) : Get properties of the archive  
- [PclZip::delete()](class_and_methods.md#method-pclzipdelete) : Delete files inside the archive  
- [PclZip::merge()](class_and_methods.md#method-pclzipmerge) : Add one archive content in a second archive  
- [PclZip::duplicate()](class_and_methods.md#method-pclzipduplicate) : Duplicate the archive  



## Method PclZip::PclZip

### Overview

 This method is the constructor of the PclZip object.

### Synopsis

> PclZip($zip_filename)

### Arguments

- $zip_filename : filename of the PKZIP archive

### Description

This method creates a PclZip object which represent the PKZIP archive. At this point only the filename of the archive is set, and some check done. No action is performed.  
While creating the PclZip object the constructor checks that the zlib extension is available. If not the PHP script abort with an error message.

### Samples

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip  
  if ($archive->create('file.txt data/text.txt folder/') == 0) {  
    die('Error : '.$archive->errorInfo(true));  
  } 
```

### See Also

[create()](#method-pclzipcreate), [add()](#method-pclzipadd), [extract()](#method-pclzipextract), [delete()](#method-pclzipdelete)


## Method PclZip::create

### Overview

This method creates PKZIP archives with the specified files.

### Synopsis

> PclZip::create($filelist, [optional arguments])

### Arguments

- $filelist :
An array of filenames or dirnames,  
or  
A string containing the a filename or a dirname,  
or  
A string containing a list of filename or dirname separated by a comma.

### Optional arguments :

This method support the following optional arguments :  
- [PCLZIP_OPT_REMOVE_PATH](optional_arguments.md#argument-pclzip_opt_remove_path)  
- [PCLZIP_OPT_REMOVE_ALL_PATH](optional_arguments.md#argument-pclzip_opt_remove_all_path)  
- [PCLZIP_OPT_ADD_PATH](optional_arguments.md#argument-pclzip_opt_add_path)  
- [PCLZIP_CB_PRE_ADD](optional_arguments.md#argument-PCLZIP_CB_PRE_ADD)  
- [PCLZIP_CB_POST_ADD](optional_arguments.md#argument-PCLZIP_CB_POST_ADD)  
- [PCLZIP_OPT_NO_COMPRESSION](optional_arguments.md#argument-pclzip_opt_no_compression)  
- [PCLZIP_OPT_COMMENT](optional_arguments.md#argument-pclzip_opt_comment)  
- [PCLZIP_OPT_TEMP_FILE_THRESHOLD](optional_arguments.md#argument-pclzip_opt_temp_file_threshold)  
- [PCLZIP_OPT_TEMP_FILE_ON](optional_arguments.md#argument-pclzip_opt_temp_file_on)  
- [PCLZIP_OPT_TEMP_FILE_OFF](optional_arguments.md#argument-pclzip_opt_temp_file_off)  
See chapter "[Optional arguments](#options)" for more informations.

### Returned values

- 0 : On error.
- an array : An array file porperties. (See "[Returned values](manual.md#return_values)")

### Description

This method creates an archhive with all the files and folders indicated in the argument paramètre $filelist. When the filename is the name of a folder, all the files and subdirs of the folder are added, the filesystem structure is memorized.  
The optional arguments gives the ability to archive files with a different path namethan the realone. This allow to put the script somewhere in a private path, and archive files without showing the path between the scriptand the files.  
This can be used also to prepare a 'default' extraction path of the archived files.

### Samples

```php
 include_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->create('file.txt,data/text.txt,folder  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample the PKZIP archive 'archive.zip' is created. The files file.txt and data/text.txt are added. All the folder 'folder' is added (its files and recursively all the subdirs)

```php
 include_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->create('data/file.txt,data/text.txt', PCLZIP_OPT_REMOVE_PATH, 'data', PCLZIP_OPT_ADD_PATH, 'install  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

in this sample the files 'file.txt' and 'text.txt', which are locally in the folder 'data', are added in the archive without the path 'data', but with the path 'install'. They are stored with filenames 'install/file.txt' and 'install/text.txt'.

### See also

[PclZip()](#method-pclzippclzip), [add()](#method-pclzipadd), [extract()](#method-pclzipextract), [delete()](#method-pclzipdelete)


##  Method PclZip::listContent

### Overview

This method gives back the list and properties of the files and foders of the archive.

### Synopsis

> PclZip::listContent()

### Returned values

- 0 : On error.
- an array : An array file porperties. (See "[Returned values](manual.md#return_values) ")


### Description

This method gives the list of the archive content. The result is an array, each entry in the array describes an archived file or a folder.  
For the details of the array entry (the file properties) see : "[Returned values](manual.md#return_values) "

### Sample

```php
 include_once('pclzip.lib.php $zip = new PclZip("test.zip");  
    
  if (($list = $zip->listContent()) == 0) {  
    die("Error : ".$zip->errorInfo(true));  
  }  
    
  for ($i=0; $i<sizeof($list); $i++) {  
    for(reset($list[$i]); $key = key($list[$i]); next($list[$i])) {  
      echo "File $i / [$key] = ".$list[$i][$key]."";  
    }  
    echo "";  
  }   
```

This sample will give the following result :

```php
 File 0 / [filename] = data/file1.txt  
 File 0 / [stored_filename] = data/file1.txt  
 File 0 / [size] = 53  
 File 0 / [compressed_size] = 36  
 File 0 / [mtime] = 1010440428  
 File 0 / [comment] =   
 File 0 / [folder] = 0  
 File 0 / [index] = 0  
 File 0 / [status] = ok

 File 1 / [filename] = data/file2.txt  
 File 1 / [stored_filename] = data/file2.txt  
 File 1 / [size] = 54  
 File 1 / [compressed_size] = 53  
 File 1 / [mtime] = 1011197724  
 File 1 / [comment] =   
 File 1 / [folder] = 0  
 File 1 / [index] = 1  
 File 1 / [status] = ok
```

## Method PclZip::extract()

### Overview

This method extract the files (and folders) archived in the PKZIP file.

### Synopsis

> PclZip::extract([options list])

### Arguments

### Optional arguments

This method support the following optional arguments :  
- [PCLZIP_OPT_PATH](optional_arguments.md#argument-pclzip_opt_path)  
- [PCLZIP_OPT_REMOVE_PATH](optional_arguments.md#argument-pclzip_opt_remove_path)  
- [PCLZIP_OPT_REMOVE_ALL_PATH](optional_arguments.md#argument-pclzip_opt_remove_all_path)  
- [PCLZIP_OPT_ADD_PATH](optional_arguments.md#argument-pclzip_opt_add_path)  
- [PCLZIP_CB_PRE_EXTRACT](optional_arguments.md#argument-PCLZIP_CB_PRE_EXTRACT)  
- [PCLZIP_CB_POST_EXTRACT](optional_arguments.md#argument-PCLZIP_CB_POST_EXTRACT)  
- [PCLZIP_OPT_SET_CHMOD](optional_arguments.md#argument-pclzip_opt_set_chmod)  
- [PCLZIP_OPT_BY_NAME](optional_arguments.md#argument-pclzip_opt_by_name)  
- [PCLZIP_OPT_BY_EREG](optional_arguments.md#argument-pclzip_opt_by_ereg)  
- [PCLZIP_OPT_BY_PREG](optional_arguments.md#argument-pclzip_opt_by_preg)  
- [PCLZIP_OPT_BY_INDEX](optional_arguments.md#argument-pclzip_opt_by_index)  
- [PCLZIP_OPT_EXTRACT_AS_STRING](optional_arguments.md#argument-pclzip_opt_extract_as_string)  
- [PCLZIP_OPT_EXTRACT_IN_OUTPUT](optional_arguments.md#argument-pclzip_opt_extract_in_output)  
- [PCLZIP_OPT_REPLACE_NEWER](optional_arguments.md#argument-pclzip_opt_replace_newer)  
- [PCLZIP_OPT_STOP_ON_ERROR](optional_arguments.md#argument-pclzip_opt_stop_on_error)  
- [PCLZIP_OPT_EXTRACT_DIR_RESTRICTION](optional_arguments.md#argument-pclzip_opt_extract_dir_restriction)  
- [PCLZIP_OPT_TEMP_FILE_THRESHOLD](optional_arguments.md#argument-pclzip_opt_temp_file_threshold)  
- [PCLZIP_OPT_TEMP_FILE_ON](optional_arguments.md#argument-pclzip_opt_temp_file_on)  
- [PCLZIP_OPT_TEMP_FILE_OFF](optional_arguments.md#argument-pclzip_opt_temp_file_off)  
See chapter "[Optional arguments](#options)" for more informations.

### Returned values

- 0 : On error.
- an array : An array with the extracted files.  
Notice that if one file extraction fail, the full extraction does not fail. The method does not return an error, but the file status is set with the error reason.  
(See "[Returned values](manual.md#return_values) ")

### Description

This method extract all or part of the files contained in the PKZIP file.  
Filtering can be done by the optional arguments [PCLZIP_OPT_BY_NAME](optional_arguments.md#argument-pclzip_opt_by_name), [PCLZIP_OPT_BY_EREG](optional_arguments.md#argument-pclzip_opt_by_ereg), [PCLZIP_OPT_BY_PREG](optional_arguments.md#argument-pclzip_opt_by_preg) and [PCLZIP_OPT_BY_INDEX](optional_arguments.md#argument-pclzip_opt_by_index).  
Other optional arguments gives the ability to extract in a specific folder ([PCLZIP_OPT_PATH](optional_arguments.md#argument-pclzip_opt_path), [PCLZIP_OPT_ADD_PATH](optional_arguments.md#argument-pclzip_opt_add_path)), remove the archived path ([PCLZIP_OPT_REMOVE_ALL_PATH](optional_arguments.md#argument-pclzip_opt_remove_all_path)) or remove only a prefix of this path ([PCLZIP_OPT_REMOVE_PATH](optional_arguments.md#argument-pclzip_opt_remove_path)).  
When extracting few files, or small files, the option [PCLZIP_OPT_EXTRACT_AS_STRING](optional_arguments.md#argument-pclzip_opt_extract_as_string) gives the ability to extract the content of a file in the returned array rather than writing it in a file. This can be usefull for example for extracting the readme file of an archived package, before doing the full extract.  
An other option is to directly send the file content to the standard output ([PCLZIP_OPT_EXTRACT_IN_OUTPUT](optional_arguments.md#argument-pclzip_opt_extract_in_output)).

### Sample

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip  
  if ($archive->extract() == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample all the files of the archive are extracted in the current directory.

```php
 include('pclzip.lib.php $archive = new PclZip('archive.zip  
  if ($archive->extract(PCLZIP_OPT_PATH, 'data', PCLZIP_OPT_REMOVE_PATH, 'install/release') == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample all the files are extracted in the folder 'data'. All the files, with path prefix 'install/release', are extracted in 'data', not in 'data/install/release'.


## Method PclZip::properties()

### Overview

This method gives back the general properties of the PKZIP archive.

### Synopsis

> PclZip::**properties**()

### Returned values

- 0 : On error.
- an array : An array with the PKZIP properties (see bellow)

### Description

This method gives the general properties of the PKZIP archive.  
The available properties are :

- nb : Number of entries (files / folders) in the archive
- comment : Comments associated with the archive.
- status : Status of the archive (only OK is available today)


## Method PclZip::add()

### Overview

This method add files or folders in a PKZIP archive.  
(PclZip >= 1.1)

### Synopsis

> PclZip::add($filelist, [options list])

### Argument

- $filelist : An array of filenames or dirnames,  
or  
A string containing the a filename or a dirname,  
or  
A string containing a list of filename or dirname separated by a comma.


### optional arguments :

This method support the following optional arguments :  
- [PCLZIP_OPT_REMOVE_PATH](optional_arguments.md#argument-pclzip_opt_remove_path)  
- [PCLZIP_OPT_REMOVE_ALL_PATH](optional_arguments.md#argument-pclzip_opt_remove_all_path)  
- [PCLZIP_OPT_ADD_PATH](optional_arguments.md#argument-pclzip_opt_add_path)  
- [PCLZIP_CB_PRE_ADD](optional_arguments.md#argument-PCLZIP_CB_PRE_ADD)  
- [PCLZIP_CB_POST_ADD](optional_arguments.md#argument-PCLZIP_CB_POST_ADD)  
- [PCLZIP_OPT_NO_COMPRESSION](optional_arguments.md#argument-pclzip_opt_no_compression)  
- [PCLZIP_OPT_COMMENT](optional_arguments.md#argument-pclzip_opt_comment)  
- [PCLZIP_OPT_ADD_COMMENT](optional_arguments.md#argument-pclzip_opt_add_comment)  
- [PCLZIP_OPT_PREPEND_COMMENT](optional_arguments.md#argument-pclzip_opt_prepend_comment)  
- [PCLZIP_OPT_TEMP_FILE_THRESHOLD](optional_arguments.md#argument-pclzip_opt_temp_file_threshold)  
- [PCLZIP_OPT_TEMP_FILE_ON](optional_arguments.md#argument-pclzip_opt_temp_file_on)  
- [PCLZIP_OPT_TEMP_FILE_OFF](optional_arguments.md#argument-pclzip_opt_temp_file_off)  
See chapter "[Optional arguments](#options)" for more informations.

### Returned values

- 0 : On error.
- an array : An array file properties. (See "[Returned values](manual.md#return_values) ")

### Description :  

This method adds in the existing PKZIP archive the files and folders set in the $filelist argument. When the name is a directory name, all the folder name and sub-folders are added and the filesystem structure is memorized.  
Becareful that if a file already exist in an archive it is added at the end of the archive, but not automatically replaced.  
If the archive does not exist, it is automatically created.

The optional arguments gives the ability to modify the way PclZip add the files in the archive. The path can be removed or changed, the file can be added without any compression. Comments can be added globally in the archive.

### Samples :  

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->add('file.txt,data/text.txt,folder/  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample the files 'file.txt', 'data/text.txt' and all the content of the directory 'folder' are added in the archive.

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->add('dev/file.txt,dev/text.txt', PCLZIP_OPT_ADD_PATH, 'install', PCLZIP_OPT_REMOVE_PATH, 'dev  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample the files 'dev/file.txt' and 'dev/text.txt' are added in the archive. However the path 'dev/' is removed and replaced by 'install/'. Therefore the files are stored with names 'install/file.txt' and 'install/text.txt'.



## Method PclZip::delete()

### Overview

This method remove from the archive all or part of the files contained in the PKZIP file.

### Synopsis

> PclZip::delete([options list])

### Arguments

### options list :

This method support the following optional arguments :  
- [PCLZIP_OPT_BY_NAME](optional_arguments.md#argument-pclzip_opt_by_name)  
- [PCLZIP_OPT_BY_EREG](optional_arguments.md#argument-pclzip_opt_by_ereg)  
- [PCLZIP_OPT_BY_PREG](optional_arguments.md#argument-pclzip_opt_by_preg)  
- [PCLZIP_OPT_BY_INDEX](optional_arguments.md#argument-pclzip_opt_by_index)  
See chapter "[Optional arguments](#options)" for more informations.

### Returned values

- 0 : On error.
- an array : The list of the files (and their properties) that are still present in the archive..  
(See "[Returned values](manual.md#return_values) ")

### Description

This method remove from the archive all or part of the files contained in the PKZIP file.  
Filtering can be used to remove only specific files from the archive. Filters are defined by the optional arguments [PCLZIP_OPT_BY_NAME](optional_arguments.md#argument-pclzip_opt_by_name), [PCLZIP_OPT_BY_EREG](optional_arguments.md#argument-pclzip_opt_by_ereg), [PCLZIP_OPT_BY_PREG](optional_arguments.md#argument-pclzip_opt_by_preg) and [PCLZIP_OPT_BY_INDEX](optional_arguments.md#argument-pclzip_opt_by_index).

### Sample

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->delete();  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample all the files contained in the archive are removed.

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->delete(PCLZIP_OPT_BY_INDEX, '1-3,5,8-10  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 
```

In this sample, files at index 1 to 3, 5 and 8 to 10 removed from the archive. Files (and folders entries) index can be found be using the [listContent()](#method-pclziplistcontent) method.  
Please notice that a folder has its own entry (with its own index). When removing the folder entry the files belonging to this folder are not removed.

```php
 require_once('pclzip.lib.php $archive = new PclZip('archive.zip $v_list = $archive->delete(PCLZIP_OPT_BY_EREG, 'txt$  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  }
```

In this sample all files finishing by 'txt' are removed from the archive.


## Method PclZip::merge()

### Overview

Merge the content of a zip archive at the end of the current archive.  
(PclZip >= 1.2)

### Synopsis

> PclZip::merge($archive_filename)

### Arguments

- $archive_filename : The filename of the archive to merge with.

### Returned values

- 0 : On error
- 1 : On Success.

### Description

The function add the content of the archive identified by its filename ($archive_filename) in the archive represented by the current object archive. No check is done on the archive content, for exemple duplicated files in the archives are not detected. This is not an update function, this is a 'stupid' merge.


## Method PclZip::duplicate

### Overview

Duplicate a zip archive.  
(PclZip >= 1.2)

### Synopsis

> PclZip::duplicate($archive_filename)

### Arguments

- $archive_filename : The filename of the archive to duplicate.

### Returned values

- 0 : On error.
- 1 : On Success.

### Description

This function duplicate the archive identified by its filename ($archive_filename). The current PclZip object should be just created, without any other action. If the PclZip objet is associated with an existing archive, this archive is deleted and replaced by the duplicated archive.

