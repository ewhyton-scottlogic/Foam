# Linking Functions to Blob Storage

Task that I was set:
Make an HTTP function which writes a message in a queue, then make another function which gets that message and writes it to a blob storage.

Pain points:

- C# was my biggest hurdle to overcome and lack of knowledge in the language held me back
- Not being able to follow the docs as for some reason the functions made in VS Code were read-only

Takeaways

- Understanding better how to write triggers and bindings in the code and not relying on the portal/ UI.
- Been doing it in an 'in-process' style, apparently the future of functions in .NET 8 is isolated (so not being reliant on the version on C#)
