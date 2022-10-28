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

</br>
</br>

### To add diffrent scenario in one scenario syncroniesly : 

--- Feature File

```feature
Feature: User registration

@property:email=E-mail_addresses.Valid
Scenario Outline: recording user information on successful registration

Given a visitor registering as "Mike Scott" with email <email>
When the registration completes
Then the account system should record <email> related to user "Mike Scott"

Examples: key examples
  | Variant            | email              |
  | simple valid email | simple@example.com |

@property:email=E-mail_addresses.Invalid
Scenario: rejecting invalid emails

Given a visitor registering as "Mike Scott" with email <email>
When the registration completes
Then the account system should not record <email> 
And the error response should be "Invalid Email"
```

</br>
</br>


--- step.cs File 

```c#
using NUnit.Framework;
using TechTalk.SpecFlow;

namespace Specs.Steps
{
    [Binding]
    public class RegistrationBindings
    {
        private readonly ScenarioContext _scenarioContext;

        private string _name;
        private string _email;
        private string _errorMessage = "";

        public RegistrationBindings(ScenarioContext scenarioContext)
        {
            _scenarioContext = scenarioContext;
        }
        [Given(@"a visitor registering as ""(.*)"" with email (.*)")]
        public void GivenAVisitorRegisteringAsWithEmail(string name, string email)
        {
            this._email = email;
            this._name = name;
        }

        [When(@"the registration completes")]
        public void WhenTheRegistrationCompletes()
        {
            //Simulate some email validation
            if (!_email.Contains("@"))
            {
                _email = null;
                _errorMessage = "Invalid Email";
            }
        }

        [Then(@"the account system should record (.*) related to user ""(.*)""")]
        public void ThenTheAccountSystemShouldRecordEmailToUser(string email, string name)
        {
            Assert.AreEqual(name, _name);
            Assert.AreEqual(email, _email);
        }

        [Then(@"the account system should not record (.*)")]
        public void ThenTheAccountSystemShouldNotRecord(string p0)
        {
            Assert.IsNull(_email);
        }

        [Then(@"the error response should be ""(.*)""")]
        public void ThenTheErrorResponseShouldBe(string expectedErrorMessage)
        {
            Assert.AreEqual(expectedErrorMessage, _errorMessage);
        }

    }
}

```

</br>
</br>

### To enter by default a variable to scenario : 

--- feature file 

```feature
Feature: Comments

@property:text=Lorems
Scenario: comment wrapping
Given a comment with text <text>
And the screen height of 400px
When the comment box is shown 
Then the the comment size should not exceed 2 lines
```

</br>
</br>
 
 --- step.cs  file 
 
```c#
using TechTalk.SpecFlow;
using TechTalk.SpecFlow.Infrastructure;

namespace Specs.Steps
{
    [Binding]
    public class CommentBindings
    {
        private readonly ScenarioContext _scenarioContext;

        public CommentBindings(ScenarioContext scenarioContext)
        {
            _scenarioContext = scenarioContext;
        }

        [Given(@"a comment with text (.*)")]
        public void GivenACommentWithText(string text)
        {
        }

        [Given(@"the screen height of (.*)px")]
        public void GivenTheScreenHeightOfPx(int height)
        {
        }

        [When(@"the comment box is shown")]
        public void WhenTheCommentBoxIsShown()
        {
        }

        [Then(@"the the comment size should not exceed (.*) lines")]
        public void ThenTheTheCommentSizeShouldNotExceedLines(int lines)
        {
        }
    }
}
```
