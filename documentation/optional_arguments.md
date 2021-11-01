
# PclZip User Guide - Optional Arguments

## Optional arguments List

Today the defined optional arguments are :

-  [PCLZIP_OPT_PATH](optional_arguments.md#argument-pclzip_opt_path)  
-  [PCLZIP_OPT_ADD_PATH](optional_arguments.md#argument-pclzip_opt_add_path)  
-  [PCLZIP_OPT_REMOVE_PATH](optional_arguments.md#argument-pclzip_opt_remove_path)  
-  [PCLZIP_OPT_REMOVE_ALL_PATH](optional_arguments.md#argument-pclzip_opt_remove_all_path)  
-  [PCLZIP_OPT_SET_CHMOD](optional_arguments.md#argument-pclzip_opt_set_chmod)  
-  [PCLZIP_OPT_BY_NAME](optional_arguments.md#argument-pclzip_opt_by_name)  
-  [PCLZIP_OPT_BY_EREG](optional_arguments.md#argument-pclzip_opt_by_ereg)  
-  [PCLZIP_OPT_BY_PREG](optional_arguments.md#argument-pclzip_opt_by_preg)  
-  [PCLZIP_OPT_BY_INDEX](optional_arguments.md#argument-pclzip_opt_by_index)  
-  [PCLZIP_OPT_EXTRACT_AS_STRING](optional_arguments.md#argument-pclzip_opt_extract_as_string)  
-  [PCLZIP_OPT_EXTRACT_IN_OUTPUT](optional_arguments.md#argument-pclzip_opt_extract_in_output)  
-  [PCLZIP_OPT_NO_COMPRESSION](optional_arguments.md#argument-pclzip_opt_no_compression)  
-  [PCLZIP_OPT_COMMENT](optional_arguments.md#argument-pclzip_opt_comment)  
-  [PCLZIP_OPT_ADD_COMMENT](optional_arguments.md#argument-pclzip_opt_add_comment)  
-  [PCLZIP_OPT_PREPEND_COMMENT](optional_arguments.md#argument-pclzip_opt_prepend_comment)  
-  [PCLZIP_OPT_REPLACE_NEWER](optional_arguments.md#argument-pclzip_opt_replace_newer)  
-  [PCLZIP_OPT_EXTRACT_DIR_RESTRICTION](optional_arguments.md#argument-pclzip_opt_extract_dir_restriction)  
-  [PCLZIP_OPT_STOP_ON_ERROR](optional_arguments.md#argument-pclzip_opt_stop_on_error)  
-  [PCLZIP_OPT_TEMP_FILE_ON](optional_arguments.md#argument-pclzip_opt_temp_file_on)  
-  [PCLZIP_OPT_TEMP_FILE_THRESHOLD](optional_arguments.md#argument-pclzip_opt_temp_file_threshold)  
-  [PCLZIP_OPT_TEMP_FILE_OFF](optional_arguments.md#argument-PCLZIP_OPT_TEMP_FILE_OFF)



## "Call-back" functions List


'Call-back' functions are specific arguments, because the value is a function name. The call-back function have a strict synopsis that must be respected, and the possible actions inside it are delimited. The algorithm inside the call-back function can be anything, it must respect the argument list and the returned value. However the call-back function must respect the core method processing, some actions may interfere with it (like deleting the archive file during process).

-  [PCLZIP_CB_PRE_EXTRACT](optional_arguments.md#call-back-argument-pclzip_cb_pre_extract)  
-  [PCLZIP_CB_POST_EXTRACT](optional_arguments.md#call-back-argument-pclzip_cb_post_extract)  
-  [PCLZIP_CB_PRE_ADD](optional_arguments.md#call-back-argument-pclzip_cb_pre_add)  
-  [PCLZIP_CB_POST_ADD](optional_arguments.md#call-back-argument-pclzip_cb_post_add)


## Optional Arguments Details


### Argument PCLZIP_OPT_PATH


This argument indicates the path of the folder where the archived files will be extracted.  
The value is a single strin

 $list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/"); 

This argument can be used with '[extract()]("?methods-extract")' and '[extractByIndex()]("?methods-extractbyindex")' methods.


### Argument PCLZIP_OPT_ADD_PATH

This argument gives the ability to insert a path while files are extracted or archived. This will allow to archive the file 'file.txt' with the path 'backup/file.txt', or extract the file 'data/file.txt' with path 'folder/data/file.txt'.  
The value is a single file path string.

 $list = $archive->create("file.txt,image.gif", PCLZIP_OPT_ADD_PATH, "backup"); 

This argument can be used with '[create()]("?methods-extract")', '[add()]("?methods-extractByIndex")' and'[extract()]("?methods-extract")' methods.


### Argument PCLZIP_OPT_REMOVE_PATH


This arguments gives the ability to suppress a part or all the path of the files (or directories) when they are extracted or archived. This will allow to archive file '/usr/local/user/test/file.txt' like a file with name 'test/file.txt', or to extract file stored with name 'folder/data/file.txt' as file 'data/file.txt' on the filesystem.  
The value is a single directory path string.

 $list = $archive->add("/usr/local/user/test/file.txt", PCLZIP_OPT_REMOVE_PATH, "/usr/local/user");

This argument can be used with '[create()]("?methods-create")', '[add()]("?methods-add")', '[extract()]("?methods-extract")' and '[extractByIndex()]("?methods-extractbyindex")' methods.  
Notice that this argument is ignored when argument [PCLZIP_OPT_REMOVE_ALL_PATH]("#argument-pclzip_opt_remove_all_path") is used in the same method call.



### Argument PCLZIP_OPT_REMOVE_ALL_PATH


This argument gives the ability to suppress all the path of the file when extracting it or adding it in the archive.  
With this argument it is not necessary to gives any path, and will sometimes simplify the use of [PCLZIP_OPT_REMOVE_PATH]("#argument-pclzip_opt_remove_path"). However this arguments must be carefully used, because if the tree arborescence is very deep, you must ensure that they will be no duplicate names (when the path is removed), specifically for the file extract.  
PCLZIP_OPT_REMOVE_ALL_PATH arguments does not need any value.

 $list = $archive->create("data/file.txt images/image.gif", PCLZIP_OPT_REMOVE_ALL_PATH); // This will remove path 'data/' for file 'data/file.txt'  
  // and path 'images/' for file 'images/image.gif' 

This argument can be used with '[create()]("?methods-create")', '[add()]("?methods-add")', '[extract()]("?methods-extract")' and '[extractByIndex()]("?methods-extractbyindex")' methods.



### Argument PCLZIP_OPT_SET_CHMOD


This argument gives the ability to modify the file access rights just after the extraction. On \*NIX systems, the file access management and the file owner (system attribute) does not allow to access all the files from anywhere. In particular, the user which owns the PHP process will give its credentials to the extracted files, and sometimes will forbid the access to the file to other users. The goal of this arguments is to try to change a little this constraint, by changing the access write, knowing that it can not change the file owner.  
The value is a single octal value (for example 0777).

 $list = $archive->extract(PCLZIP_OPT_SET_CHMOD, 0777);

This argument can be used with '[extract()]("?methods-extract")' and '[extractByIndex()]("?methods-extractByIndex")' methods.

\*\*\* This argument was not fully tested and should be considered as EXPERIMENTAL.




### Argument PCLZIP_OPT_BY_NAME


This argument allow the extraction of files/folders from the archive by indicating the full filenames of these files.

 $archive = new PclZip('test.zip'); $rule_list[0] = 'data/file1.txt'; $rule_list[1] = 'data/file2.txt'; $list = $archive->extract(PCLZIP_OPT_BY_NAME, $rule_list);  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

The filter can be an array with a filename per row, or a single string with the filenames separated by a comma.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_NAME, "data/file1.txt,data/file2.txt");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    }

See also [PCLZIP_OPT_BY_PREG]("#argument-pclzip_opt_by_preg"), [PCLZIP_OPT_BY_INDEX]("#argument-pclzip_opt_by_index") and [PCLZIP_OPT_BY_EREG]("#argument-pclzip_opt_by_ereg")



### Argument PCLZIP_OPT_BY_EREG


This argument allow the extraction of files / folder from the archive, by filtering the filenames with a regular expression.  
PclZip is using ereg() PHP function to perform this filtering.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_EREG, "txt$");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

See also [PCLZIP_OPT_BY_PREG]("#argument-pclzip_opt_by_preg"), [PCLZIP_OPT_BY_NAME]("#argument-pclzip_opt_by_name") and [PCLZIP_OPT_BY_INDEX]("#argument-pclzip_opt_by_index")



### Argument PCLZIP_OPT_BY_PREG


This argument allow the extraction of files / folder from the archive, by filtering the filenames with a regular expression.  
PclZip is using preg() PHP function to perform this filtering.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_PREG, "txt$");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

  

See also [PCLZIP_OPT_BY_NAME]("#argument-pclzip_opt_by_name") and [PCLZIP_OPT_BY_INDEX]("#argument-pclzip_opt_by_index")



### Argument PCLZIP_OPT_BY_INDEX


This argument allow the extraction of files/folders from the archive by indicating the indexes of the concerned files/folders in the archive.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_INDEX, array ('0-4','2-7','10-33'));  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

See also [PCLZIP_OPT_BY_PREG]("#argument-pclzip_opt_by_preg") and [PCLZIP_OPT_BY_NAME]("#argument-pclzip_opt_by_name")



### Argument PCLZIP_OPT_EXTRACT_AS_STRING


This argument gives you the ability to extract the content of a file in a string rather than in a file.  
It can be used when you just want to get the content of a file without using the filesystem.  
For examples :  
- displaying a "readme" file,  
- directly download the content of a file in the standard output (see also [PCLZIP_OPT_EXTRACT_IN_OUTPUT]("#argument-pclzip_opt_extract_in_output")),  
- ...

You must be carefull if you extract all the files of your archive in strings. This may exhaust the memory limit of your PHP process.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_NAME, "data/readme.txt", PCLZIP_OPT_EXTRACT_AS_STRING);  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
      exit;  
    }  
    echo $list[0]['content']; 

see also [PCLZIP_OPT_EXTRACT_IN_OUTPUT]("#argument-pclzip_opt_extract_in_output")



### Argument PCLZIP_OPT_EXTRACT_IN_OUTPUT


This argument gives you the ability to extract the content of a file directly in the standard output (like the echo command).

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_NAME, "data/readme.txt", PCLZIP_OPT_EXTRACT_IN_OUTPUT);  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

see also [PCLZIP_OPT_EXTRACT_AS_STRING]("#argument-pclzip_opt_extract_as_string")



### Argument PCLZIP_OPT_NO_COMPRESSION


This argument gives you the ability to add a file in an archive without compressing the file.

 $archive = new PclZip('test.zip'); $list = $archive->add("data/file.txt", PCLZIP_OPT_NO_COMPRESSION);  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 



### Argument PCLZIP_OPT_COMMENT


This argument gives the ability to set a comment in the PKZIP archive. If a comment already exists, this comment will replace the existing one.

 $archive = new PclZip('test.zip'); $list = $archive->create("data", PCLZIP_OPT_COMMENT, "Add a comment");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

See also [PCLZIP_OPT_ADD_COMMENT]("#argument-pclzip_opt_add_comment") and [PCLZIP_OPT_PREPEND_COMMENT]("#argument-pclzip_opt_prepend_comment")



### Argument PCLZIP_OPT_ADD_COMMENT


This argument gives the ability to add a comment in the PKZIP archive. If a comment already exists, the added comment is append at the end of the existing comment.

 $archive = new PclZip('test.zip'); $list = $archive->add("data", PCLZIP_OPT_ADD_COMMENT, "Add a comment after the existing one");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

See also [PCLZIP_OPT_COMMENT]("#argument-pclzip_opt_comment") and [PCLZIP_OPT_PREPEND_COMMENT]("#argument-pclzip_opt_prepend_comment")



### Argument PCLZIP_OPT_PREPEND_COMMENT


This argument gives the ability to add a comment in the PKZIP archive. If a comment already exists, the added comment is prepend before the existing comment.

 $archive = new PclZip('test.zip'); $list = $archive->add("data", PCLZIP_OPT_PREPEND_COMMENT, "Add a comment before the existing one");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 

See also [PCLZIP_OPT_COMMENT]("#argument-pclzip_opt_comment") and [PCLZIP_OPT_ADD_COMMENT]("#argument-pclzip_opt_add_comment")



### Argument PCLZIP_OPT_REPLACE_NEWER


By default PclZip, while extracting a file from the archive, follow these rules : if the file exist but has an older timeset, then it is replaced by the one coming from the archive. If the file in the archive is older, the file is not replaced.

By using PCLZIP_OPT_REPLACE_NEWER, you give to PclZip the ability to extract and replace the existing file in any situation. This can be usefull in situation like restoring an backup copy of some files/folders.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_BY_NAME, "data/readme.txt", PCLZIP_OPT_REPLACE_NEWER);  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 



### Argument PCLZIP_OPT_EXTRACT_DIR_RESTRICTION


PclZip can extract any file in any folder of a system. People may use this to upload a zip file and try to override a system file or any other file (password file, ...). The PCLZIP_OPT_EXTRACT_DIR_RESTRICTION gives the ability to forgive any directory transversal behavior.  
The use of this option can also be convenient to limit simple errors in some scripts.

Be aware that a better security is to use the directory restriction of PHP configuration.

The following example will reject the extraction of any file in the archive that are not in the "/var/www/data" folder.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_EXTRACT_DIR_RESTRICTION, "/var/www/data");  
    if ($list == 0) {  
      echo "ERROR : ".$archive->errorInfo(true);  
    } 



### Argument PCLZIP_OPT_STOP_ON_ERROR


When PclZip is extracting files from a PKZIP file it does not stop when it fail to extract a single file. Which mean it continue the extraction of the PKZIP file. The exact list of extracted files is returned (see [extract()]("?methods-extract") description) in a list. Each entry of the list describe the status of the extraction of each individual file or folder.

By using PCLZIP_OPT_STOP_ON_ERROR, you order PclZip to stop extraction at the first file extraction failure. It is then your responsability to clean partitially extracted content or do any corrective actions.

 $archive = new PclZip('test.zip'); $list = $archive->extract(PCLZIP_OPT_ADD_PATH, "extract_folder/", PCLZIP_OPT_STOP_ON_ERROR);  
    if ($list == 0) {  
      if ($archive->errorCode() == PCLZIP_ERR_ALREADY_A_DIRECTORY) {  
        echo "ERROR : File tries to replace a folder !";  
      }  
    } 



### Argument PCLZIP_OPT_TEMP_FILE_ON,  
  
### Argument PCLZIP_OPT_TEMP_FILE_OFF and  
  
### Argument PCLZIP_OPT_TEMP_FILE_THRESHOLD


Starting with release 2.7, PclZip is able to use temporary files to zip or extract files in archive to better support large files.

In most situation PclZIp will use "in memory" zip alorythm, which mean that PclZip will read all the file to archive, compress it and then write the result in the zip archive. By using temporary files PclZip will compress the file by reading small blocks (2048 bytes) and directly write the compressed block in a temporary file. Then PclZip add the temporary file in the archive and delete the temporary file.  
With release 2.8, the same mecanism is used for extraction of files from a zip archive.

When adding (extracting) very large files in the archive, PclZip can reach the memory limitation configured with PHP ("memory_limit" value in php.ini), and fail to create the archive. By using temporary file, PclZip will have better chance to succeed. However using temporary file is less performant than doing compression in memory.

Starting with 2.7, PclZip will auto-sense the size of the file, and, depending on the configured value of "memory_limit", will decide to use temporary files or not. The following optional arguments allow the user to disable auto-sense and decide what method PclZip should use.

*   PCLZIP_OPT_ADD_TEMP_FILE_ON : instruct PclZip to use all the time temporary files to create the zip archive,
*   PCLZIP_OPT_ADD_TEMP_FILE_THRESHOLD, : instruct PclZip to use temporary files for files with size greater than ,
*   PCLZIP_OPT_ADD_TEMP_FILE_OFF : instruct PclZip tu use all the time "in memory" compression to create the zip archive (same as PclZip 2.6 and earlier)

Sample using PCLZIP_OPT_TEMP_FILE_ON :

 include_once('pclzip.lib.php'); $archive = new PclZip('archive.zip'); $v_list = $archive->create('data/image-1.jpg,data/image-2.jpg', PCLZIP_OPT_REMOVE_PATH, 'data', PCLZIP_OPT_ADD_TEMP_FILE_ON);  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 

Sample using PCLZIP_OPT_TEMP_FILE_THRESHOLD :

 include_once('pclzip.lib.php'); $archive = new PclZip('archive.zip'); $v_list = $archive->create('data/image-1.jpg,data/image-2.jpg', PCLZIP_OPT_REMOVE_PATH, 'data', PCLZIP_OPT_TEMP_FILE_THRESHOLD, 10);  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 

Sample using PCLZIP_OPT_TEMP_FILE_OFF :

 include_once('pclzip.lib.php'); ini_set('memory_limit', '180M'); $archive = new PclZip('archive.zip'); $v_list = $archive->create('data/image-1.jpg,data/image-2.jpg', PCLZIP_OPT_REMOVE_PATH, 'data', PCLZIP_OPT_TEMP_FILE_OFF);  
  if ($v_list == 0) {  
    die("Error : ".$archive->errorInfo(true));  
  } 

These arguments can be used with '[create()]("?methods-create")', '[add()]("?methods-add")', '[extract()]("?methods-extract")' and '[extractByIndex()]("?methods-extractByIndex")' methods.

Note that "PCLZIP_OPT_ADD_TEMP_FILE_ON", "PCLZIP_OPT_ADD_TEMP_FILE_THRESHOLD" and "PCLZIP_OPT_ADD_TEMP_FILE_OFF" are former attributes name that were replaced by these new names.


### Argument PCLZIP_OPT_TEMP_FILE_THRESHOLD


Please See [PCLZIP_OPT_TEMP_FILE_ON]("index.php?option=com_content&view=article&id=45:pclzip-user-guide-pclzipopttempfileon&catid=8")



### Argument PCLZIP_OPT_TEMP_FILE_OFF


Please See [PCLZIP_OPT_TEMP_FILE_ON]("index.php?option=com_content&view=article&id=45:pclzip-user-guide-pclzipopttempfileon&catid=8")







## "Call-back" functions Details


### Call-back argument PCLZIP_CB_PRE_EXTRACT

This optional arguments gives you the ability to add a specific processing while extrating an archive by calling a "call-back" function before the extraction of each archived file. The call-back function PCLZIP_CB_PRE_EXTRACT can modify the extraction of the concerned file in two ways :  
- by modifying the path or the name of the extracted file,  
- by skipping the extraction of the file and go to the next one.

To be precise, the call-back function is applied after the optional arguments [PCLZIP_OPT_PATH]("#argument-pclzip_opt_path"), [PCLZIP_OPT_ADD_PATH]("#argument-pclzip_opt_add_path"), [PCLZIP_OPT_REMOVE_PATH]("#argument-pclzip_opt_remove_path") or [PCLZIP_OPT_REMOVE_ALL_PATH]("#argument-pclzip_opt_remove_all_path"), but before the coherence check (file does not exist, file is not an existing folder, ...).

The 'call-back' function, given as a value of the argument, must respect the following synopsis :

> _function myCallBack($p_event, &$p_header)  
> {  
> [... Your specific code ...]  
> return $result;  
> }_

When the method call the call-back function, it gives the following arguments :  
- $p_event : the identity of the call-back argument (here PCLZIP_CB_PRE_EXTRACT). This is usefull when you want to use the same function for different call-back actions.  
- $p_header : the description of the file that will be extracted. This is an array which contains informations in several fields. The most interesting are the archived file name and the destination file name of the extracted file. The array fields are described in the chapter '[Returned Values]("?understand#returned_values")'.  
The function can only modify the field 'filename' of $p_header array. This field is the destination filename of the extracted file. This gives the ability to change the destination of the extracted file. The other fields are read-only.  
The function must return 2, 1 or 0 ($result). Other values are reserved for futur use. If the function returns 1, then the extraction is resumed (with the potentially modified destination filename). If the result is 0 the file extraction is skipped, and the method process will go for the next file to extract. If the result is 2, the file extraction is skipped and the extraction normally stop before extracting the end of the archive.

 function myPreExtractCallBack($p_event, &$p_header)  
  { $info = pathinfo($p_header['filename']); // ----- gif files are skipped if ($info['extension'] == 'gif') {  
      return 0;  
    } // ----- jpg files are extracted in images folder else if ($info['extension'] == 'jpg') { $p_header['filename'] = 'images/'.$info['basename'];  
      return 1;  
    } // ----- all other files are simply extracted else {  
      return 1;  
    }  
  } $list = $archive->extract(PCLZIP_OPT_PATH, 'folder',); PCLZIP_CB_PRE_EXTRACT, 'myPreExtractCallBack'); 

In this sample the call-back function will skip the extraction of 'gif' files, and will extract the 'jpg' files in the 'images' folder (from the current path). All other files are normally extracted in the 'folder' folder.


### Call-back argument PCLZIP_CB_POST_EXTRACT


This optional arguments gives you the ability to add a specific processing while extrating an archive by calling a "call-back" function after the extraction of each archived file. The call-back function PCLZIP_CB_POST_EXTRACT can not modify the extraction process, but allow to perform user defined actions on the extracted file, like renaming or removing.

The 'call-back' function, given as a value of the argument, must respect the following synopsis :

> _function myCallBack($p_event, &$p_header)  
> {  
> [... Your specific code ...]  
> return $result;  
> }_

When the method call the call-back function, it gives the following arguments :  
- $p_event : the identity of the call-back argument (here PCLZIP_CB_POST_EXTRACT). This is usefull when you want to use the same function for different call-back actions.  
- $p_header : the description of the file that was extracted, specifically the name of the extracted file and the status of the extraction. The array fields are described in the chapter '[Returned Values]("?understand#returned_values")'.  
The function can not modify the $p_header array because the extraction is alreay done.  
The function must return 2 or 1 ($result). Other values are reserved for futur use. When the function returns 1, the extraction resume normally. If the function returns 2, the extraction is normally stopped, no more files are extracted.

 function myPreExtractCallBack($p_event, &$p_header) { ... }  
  
  function myPostExtractCallBack($p_event, &$p_header)  
  { // ----- look for valid extraction if ($p_header['status'] == 'ok') { // ----- read the file to the standard output readfile($p_header['filename']); // ----- delete the file unlink($p_header['filename'])  
    }  
  } $list = $archive->extract(PCLZIP_OPT_PATH, 'temp', PCLZIP_CB_PRE_EXTRACT, 'myPreExtractCallBack', PCLZIP_CB_POST_EXTRACT, 'myPostExtractCallBack'); 

In this sample the call-back function send to the standard output all the correctly extracted files and then destroy the file before extracting the next one.  
Notice that since release 2.1, the optional argument [PCLZIP_OPT_EXTRACT_IN_OUTPUT]("#argument-pclzip_opt_extract_in_output"), gives a better result, because no temporary files are created in the file system.


### Call-back argument PCLZIP_CB_PRE_ADD

This call back feature is called just before the add of each file in the archive. By using this call back you can :  
- modify the filename and path that will be stored in the archive (rather than keeping the real name and path),  
- skip the add of one (or several) selected file(s) during the archive process.

This call-back is called after the actions trigged by the optional arguments [PCLZIP_OPT_PATH]("#argument-pclzip_opt_path"), [PCLZIP_OPT_ADD_PATH]("#argument-pclzip_opt_add_path"), [PCLZIP_OPT_REMOVE_PATH]("#argument-pclzip_opt_remove_path") or [PCLZIP_OPT_REMOVE_ALL_PATH]("#argument-pclzip_opt_remove_all_path"), but before the check of the filename length..

The 'call-back' function, given as a value of the argument, must respect the following synopsis :

> _function myCallBack($p_event, &$p_header)  
> {  
> [... Your specific code ...]  
> return $result;  
> }_

When the method call the call-back function, it gives the following arguments :  
- $p_event : the identity of the call-back argument (here PCLZIP_CB_PRE_ADD). This is usefull when you want to use the same function for different call-back actions.  
- $p_header : the description of the file that will be added. This is an array which contains informations in several fields. The most interesting are the archived file name and the destination file name of the added file. The array fields are described in the chapter '[Returned Values]("?understand#returned_values")'.  
The function can only modify the field 'filename' of $p_header array. This field is the destination filename of the added file. This gives the ability to change the destination of the added file. The other fields are read-only.  
The function must return 1 or 0 ($result). Other values are reserved for futur use. If the function returns 1, then the add is resumed (with the potentially modified destination filename). If the result is 0 the file add is skipped, and the method process will go for the next file to add.

 function myPreAddCallBack($p_event, &$p_header)  
  { $info = pathinfo($p_header['stored_filename']); // ----- bak files are skipped if ($info['extension'] == 'bak') {  
      return 0;  
    } // ----- jpg files are add with an images folder else if ($info['extension'] == 'jpg') { $p_header['stored_filename'] = 'images/'.$info['basename'];  
      return 1;  
    } // ----- all other files are simply added else {  
      return 1;  
    }  
  } $list = $archive->add(PCLZIP_CB_PRE_ADD, 'myPreAddCallBack'); 

In this sample the call-back function does not archive the files with extension 'bak', archive the files with extension 'jpg' in an 'images'folder and archive all the other files without any changes.


### Call-back argument PCLZIP_CB_POST_ADD


This call back feature is called just after the add of each file in the archive. This call-back can not change the adding of the file, but can be usefullfor removing files or doing advanced work after the add.

The 'call-back' function, given as a value of the argument, must respect the following synopsis :

> _function myCallBack($p_event, &$p_header)  
> {  
> [... Your specific code ...]  
> return $result;  
> }_

When the method call the call-back function, it gives the following arguments :  
- $p_event : the identity of the call-back argument (here PCLZIP_CB_POST_ADD). This is usefull when you want to use the same function for different call-back actions.  
- $p_header : the description of the file that was added, specifically the name of the added file and the status of the add action. The array fields are described in the chapter '[Returned Values]("?understand#returned_values")'.  
The function can not modify the $p_header array because the add is alreay done.  
The function must return 1 ($result). Other values are reserved for futur use.

 function myPostAddCallBack($p_event, &$p_header)  
  { // ----- look for valid add if ($p_header['status'] == 'ok') { // ----- move the file to the trash can rename($p_header['filename'], 'trash/'.$p_header['filename'])  
    }  
  } $list = $archive->extract(PCLZIP_CB_POST_ADD, 'myPostAddCallBack'); 

In this sample the call-back PCLZIP_CB_POST_ADD move the archived files in a 'trash' folder that can be destroyed later.





  

