# The C Macro

The C Macro makes your local clipboard easily accessible in your Terminal. While xclip/pbcopy can do the same (and are used in the backend), the c macro is consistent across platforms with some neat extras.

The C Macro is ideal for collaboration. We observed too often that many things get sent in messengers, spamming everyones notifcations and requiring an internet connection. Example usages include but are not limited to:

```bash
# Dev A: hey have a look at that weird log output
$ ./thing_that_produces_weird_log_output | c log

# Dev B:
$ c log
Lots of log output...
```

```bash
# Dev A: what folder you want that file scp'd to?

# Dev B:
$ cf data/ hereplease

# Dev A: data comming your way bro
$ scp -r data/ server:$(c hereplease) 
```

```bash
# Dev A: can you send me that file

# Dev B: it's in 'c file'
$ cat file | c file

# Dev A: thanks bro
$ c file > file
```


## Local Usage

Everything from stdin goes directly to the clipboard
```bash
# Store stdout in your local clipboard
$ echo "Hello C" | c
```


Otherwise your clipboard is printed to stdout
```bash
# Retrieve from the clipboard
$ c 
Hello C
$ c | wc -w
2
```


## Remote Usage

If setup, c can also send/retrieve the data from a remote server, instead of your local clipboard. 


Start a local server and set the appropriate environment variable
```bash
$ docker run --rm -d --name c-server -p 8000:80 -e STORAGE_BACKEND=file -e STORAGE_PATH=/tmp rettier/c-server
# to clean up after the tutorial run $ docker stop c-server

# fish
$ set -gx C_HOST http://127.0.0.1:8000/

# bash
$ export C_HOST=http://127.0.0.1:8000/
```


To retrieve/store from the remote server simply pass an extra argument, specifying the name
```bash
$ echo "Hello Remote" | c test
saved as test

$ c test
Hello Remote
```


The remote server also supports folders
```bash
$ echo "Hello Folder" | c folder hello # equivalent to  "c folder/hello"
saved as folder/hello
```


Also stored files/folders can be listed via ls/ll
```bash
$ c ls
folder/ test

$ c ll
   (dir) folder
   33.0B test
   
$ c ll folder
   28.0B folder/hello
```

## Misc

an additional macro is provided to copy the full path of the specified file/folder
```bash
# copy the fullpath of the current path into your local clipboard
$ cf .
/Users/reitph/Development/c/docker

# stores the same path on the remote server named path
$ cf . path
/Users/reitph/Development/c/docker
```

to get the remote data into your local clipboard
```bash
$ c test | c
```


to paste from your local clipboard to the remote 
```bash
$ c | c test
```
