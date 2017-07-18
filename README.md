# dotnetcore-microsoft-identity-inmemory
This is just an example I wrote to figure out how to create a .NET Core app just accepting Microsoft accounts without storing anything on a disk database.

## Motivation
Suppose you want to write a simple internal web app for your team. Users will just come from a external source, like Microsoft accounts.

You just want certain users to be allowed to log in (your team members), but you don't want to implement the entire account management. They will just use their MS accounts, which is perfect if your email is hosted in Office 365.

(Implementing the entire user house keeping is not that hard using the built in Identity stuff in .NET Core, but it is even easier not to do it).

Initially I thought I could "just let external users login somehow" without creating the entire database thing. And I guess this might be possible. But, what I finally did was:

* Enable MS accounts.
* Store the "identity thing" in an InMemory database => so nothing stored on the 'real' db. Yes, data is gone on restart, but you don't care, because I just let external users login, not built-in ones.
* Add some check in ExternalLoginCallback to only allow some preconfigured users to login (otherways *any* MS account could login).

And that's all.

Once you have this in place, you can easily implement the rest of your app.

I went commit by commit recording the steps I did.

## Steps
1. Create the app with dotnet new mvc --auth Individual
2. Add in memory support with dotnet add package Microsoft.EntityFrameworkCore.InMemory -v 1.1.2
3. Edit Startup.cs to use the in memory db for the Identity stuff
4. Add Microsoft auth: dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount -v 1.1.2
5. Configure the external app and add the secret and appid.
6. Modify the ExternalLoginCallback to autocreate users coming from MS accounts.

I don't know if there are any issues with this disposable in-memory approach at this point :-S

I just keep this as a step by step example to remember how to do this stuff.

I used donet 1.0.3
