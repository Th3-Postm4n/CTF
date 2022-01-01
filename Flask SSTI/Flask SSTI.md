# Flask SSTI

#### Today I encountered a web problem which is vulnerable to `SSTI` Server Side Template Injection

Let's talk about it :)

There is file upload functionality which accept only `.tar.gz` file and extracts the files by decompressing it and put it in a specific folder.

If we rename the files like `../../tmp.txt` and compress it in `.tar.gz` the application is vulnerable to `directory traversal`. But the fun part is that we can also overwrite  existing files.

Going through the source code we can say that if we can overwrite the `routes.py` file, the game is over.

Let's modify the file and put an endpoint for us

![[sst1.png]]

Let's make the `.tar.gz` file with the `path` where we want to put the `routes.py` file

![[sst2.png]]

Let's upload the file and test our endpoint

![[sst4.png]]

Now we use this payload to run command:
`{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}`

And BOOM

![[sst3.png]]
