# CSharpSDK
C# SDK For C# Applications

Register for an account here: https://www.appfeedback.io/

_Please note, not all features are available on basic plans. Uploading images, binary files & logs require Plus or Pro plans._

Once imported do the following

Initilise the SDK:
```c#
AppFeedback.Configure("YOUR PROJECT KEY HERE!!");
```

Prepare the request like so:
```c#
AppFeedback.SendFeedbackRequest request = new AppFeedback.SendFeedbackRequest();
request.feedback = "This is your feedback string";
request.email = "youremail@email.com";
request.happiness = 10;
request.version = "1.0.0";
request.additionalData =
new Dictionary<string, string>
{
    {"cpu", "AMD" },
    {"ram", 2024.ToString() }
};
```

You can also add these to upload images, binary files and logs ( on supported plans ):
```c#
// E.g. read in a screenshot or image to send
byte[] imageBytes = System.IO.File.ReadAllBytes(@"screenshot.png");

request.imageBytes = imageBytes;

// You can also do a generic byte array for a binary file, say a save file
request.binaryBytes = null;

// Log is just the string and will be saved as a text file
request.log = "This is the log contents if you want to add something";
```

Prepare some callbacks:
```c#
static private void OnAppFeedbackSuccess()
{
    Console.WriteLine("Success");
}

static private void OnAppFeedbackFailure(AppFeedback.Error error)
{
    Console.WriteLine(error.m_RawErrorString);
}
```
Send the data     
```c#
AppFeedback.Send(request, OnAppFeedbackSuccess, OnAppFeedbackFailure);
```

**Please note:**

The send will happen async to not block main thread. The Success and Failure callbacks will be called back on main thread.
If you are going to close/end the application immediately afterwards, it may not complete sending the feedback so use the:

```c#
AppFeedback.Flush();
```
function to force wait for all current send tasks. Remember, this will block the main thread until it is finished sending the data.

