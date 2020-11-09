|Nr|Exercise
|-|-
|1|Using the Microsoft Azure Storage Explorer and Storage Emulator
|2|Using `string` Blob ouput bindings
|3|Using `CloudBlobContainer` Blob ouput bindings
|4|Using `dynamic` Blob ouput bindings
|5|Using `Stream` Blob input bindings
|6|Using `CloudBlobContainer` Blob input bindings
|7|Using `dynamic` Blob input bindings
|8|Creating a Blob triggered function


## Using `string` Blob ouput bindings

In this exercise, we'll be creating a HTTP Function App with the default HTTPTrigger and extend it with a Blob output binding in order to write a `Player` json object to a "players/out" path in Blob Storage.

### Steps

1. In VSCode Create a new HTTP Trigger Function App with the following settings:
   1. Location: _AzureFunctions.Blob_
   2. Language: _C#_
   3. Template: _HttpTrigger_
   4. Function name: _StorePlayerWithStringBlobOutput_
   5. Namespace: _AzureFunctionsUniversity.Demo_  
   6. AccessRights: _Function_
2. Once the Function App is generated add a reference to the `Microsoft.Azure.WebJobs.Extensions.Storage` NuGet package to the project. This allows us to use bindings for Blobs, Tables and Queues. 

   > 📝 __Tip__ - One way to easily do this is to use the _NuGet Package Manager_ VSCode extension:
   >   1. Run `NuGet Package Manager: Add new Package` in the Command Palette (CTRL+SHIFT+P).
   > 2. Type: `Microsoft.Azure.WebJobs.Extensions.Storage`
   > 3. Select the most recent (non-preview) version of the package.

3. We want to store an object with (game)player data. Create a new file in the project called `Player.cs` and add the contents from this [Player.cs](../src/AzureFunctions.Blob/Models/Player.cs) file.
4. Now open the `StorePlayerWithStringBlobOutput.cs` function file and add the following output binding directly underneath the `HttpTrigger` method argument:
   ```csharp
   [Blob("players/out/string-{rand-guid}.json", FileAccess.Write)] out string playerBlob
   ``` 
    > 🔎 __Observation__ - The first part parameter of the Blob attibute is the full path where the blob will be stored. The __{rand-guid}__ section in path is a so-called __binding expression__. This specific expression creates a random guid. There are more expressions available as is described [in the documentation](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-expressions-patterns). The second parameter indicates we are writing to Blob Storage. Finally we specify that there is an output argument of type `string` named `playerBlob`.
5. We'll be doing a post to this function so the `"get"` can be removed from the `HttpTrigger` attribute.
6. Change the function input type and name from `HttpRequest req` to `Player player` so we have direct acccess to the `Player` type in the request.
7. Remove the existing content of the function method, since we'll be writing a new implementation.
8. To return a meaningful response the the client, based on a valid `Player` object, add the following lines of code in the method:
   ```csharp
   playerBlob = default;
   IActionResult result;

   if (player == null)
   {
      result = new BadRequestObjectResult("No player data in request.");
   }
   else
   {
      result = new AcceptedResult();
   }

   return result;
   ```
9. Since we're using `string` as the output type the `Player` object needs to be serialized. This can be done as follows inside the `else` statement in the method:
   ```csharp
   playerBlob = JsonConvert.SerializeObject(player, Formatting.Indented);
   ```
10. Build & run the `AzureFunctions.Blob` Function App.
11. Make a POST call to the `StorePlayerWithStringBlobOutput` endpoint and provide a valid json body with a Player object:
      ```http
      POST http://localhost:7071/api/StorePlayerWithStringBlobOutput
      Content-Type: application/json

      {
         "id": "{{$guid}}",
         "nickName" : "Ada",
         "email" : "ada@lovelace.org",
         "region" : "United Kingdom"
      }
      ```
       > 📝 __Tip__ - The `{{$guid}}` part in the body creates a new random guid when the request is made. This functionality is part of the VSCode REST Client extension. 

12. > ❔ __Question__ - Is there a blob created blob storage? What is the exact path of the blob?
13. > ❔ __Question__ - What do you think would happen if you run the function again with the exact same input?

## Using `CloudBlobContainer` Blob ouput bindings

In this exercise, we'll be adding an HttpTrigger function and use the Blob output binding with the `CloudBlobContainer` type in order to write a `Player` json object to Blob Storage.

### Steps

1. Create a copy of the `StorePlayerWithStringBlobOutput.cs` file and rename the file, the class and the function to `StorePlayerWithStreamBlobOutput`.
2. Change the `Blob` attribute as follows:
   ```csharp
   [Blob("players", FileAccess.Write)] CloudBlobContainer cloudBlobContainer
   ```
    > 🔎 __Observation__ - The `CloudBlobContainer` refers to a blob container and not directly to a specific blob. Therefore we only have to specify the `"players"` container in the `Blob` attribute.
3. Update the code inside the `else` statement. Remove the line with `playerBlob = JsonConvert.SerializeObject...` and replace it with:
   ```csharp
   var blob = cloudBlobContainer.GetBlockBlobReference($"out/cloudblob-{player.NickName}.json");
   var playerBlob = JsonConvert.SerializeObject(player);
   await blob.UploadTextAsync(playerBlob);
   ```
   > 🔎 __Observation__ - Notice that the argument for getting a reference to a blockblob includes the `out/` path. This part is a virtual folder, it is not a real container such as the `"player"` container. The filename of the blob is a concatenation of "cloudblob-", the nickname of the player object, and the json extension.
4. Build & run the `AzureFunctions.Blob` Function App.
5. Make a POST call to the `StorePlayerWithContainerBlobOutput` endpoint and provide a valid json body with a Player object:
   ```http
   POST http://localhost:7071/api/StorePlayerWithContainerBlobOutput
   Content-Type: application/json

   {
      "id": "{{$guid}}",
      "nickName" : "Margaret",
      "email" : "margaret@hamilton.org",
      "region" : "United States of America"
   }
   ```
6. > ❔ __Question__ - Is the blob created in the `players/in` location?
7. > ❔ __Question__ - What happens when you run the function with the exact same input?
8. > ❔ __Question__ - Use one of the other `Player` properties as the partial filename. Does that work?

## Using `dynamic` Blob ouput bindings

In this exercise, we'll be adding an HttpTrigger function and use a dynamic Blob output binding in order to write a `Player` json object to Blob Storage.

### Steps

1. Create a copy of the `StorePlayerWithStringBlobOutput.cs` file and rename the file, the class and the function to `StorePlayerWithStreamBlobOutput`.