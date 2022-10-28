# automation-test  -- .Net --

## Foreach(): Do somthing woth each element:

```c#
 var rows = Page.Locator("table tr");
 
 var count = await rows.CountAsync()
for (let i = 0; i < count; ++i)
  Console.WriteLine(await rows.Nth(i).TextContentAsync());

```

</br>
</br>

>We only recommend using ElementHandle in the rare cases when you need to perform extensive DOM traversal on a static page. For all user actions and assertions use locator instead.

[For more about elementHandle and locator](https://playwright.dev/dotnet/docs/api/class-elementhandle).

</br>
</br>

### For UI view testing in Playwright:
> type in pw: $env:HEADED=1
> and then type dotnet test

</br>
</br>

### To record the steps in url, and convert it to playwright code:

```cmd 
pwsh .\bin\Debug\net6.0\playwright.ps1 codegen "enter here the url"
```

### To record the steps in a phone device (.net) :

```cmd
pwsh playwright codegen --device="iPhone 11" "enter here the url"
```

[for more example:](https://docs.specflow.org/en/latest/Examples.html)
