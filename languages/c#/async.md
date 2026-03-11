
### basic concepts

- Think of `await` as: “*This step may yield control — don’t block the thread.*”
- *What yield control means:* I am stopping execution here, and letting the current thread go do other work until this operation finishes.
	- You are not yielding to some “async thread”.
	- You are yielding back to the current synchronization context.
### await using & IAsyncDisposable

>This allows to perform time-consuming tasks (e.g involving I/O) on cleanup without blocking.

- This is possible because File.OpenRead implements `IAsyncDisposable`.
- https://stackoverflow.com/questions/58791938/c-sharp-8-understanding-await-using-syntax
- Example:
```c#
async Task<string> CopyFile(string sourceFilePath, string destination)  
{  
        string fileId = GenerateFileId();  
        string extension = Path.GetExtension(sourceFilePath);  
        string filename = fileId + extension;  
        string destPath = Path.Combine(destination, filename);  
        await using var src = File.OpenRead(sourceFilePath);  
        await using var dst = File.Create(destPath);  
        await src.CopyToAsync(dst);  
        return filename;  
}
```

- *Those `await`s are for **cleanup**, not for copying.*
- These two `await using` lines correspond to:
```c#
var src = File.OpenRead(sourceFilePath);
try
{
    var dst = File.Create(destPath);
    try
    {
        await src.CopyToAsync(dst);
    }
    finally
    {
        await dst.DisposeAsync();
    }
}
finally
{
    await src.DisposeAsync();
}

```