# automation-test  -- .Net --


## Gherkin-Syntax -Given - When - Then-

1- Given:

> Given steps are used to describe the initial context of the system – the scene of the scenario. It is typically something that happened in the past.


```feature
Feature: Calculator
 
Calculator for adding two numbers
 
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered 50 into the calculator

```

</br>
</br>

2- And :
> And-keyword is used to extend to your steps.
 Helps you to elaborate your feature-file further.

```feature
Feature: Calculator
 
Calculator for adding two numbers
 
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered 50 into the calculator
And I have entered 70 into the calculator

```

</br>
</br>

3- When:

> When keyword defines the test action which will be executed.

 By test action we mean the user input action.

```feature
Feature: Calculator
 
Calculator for adding two numbers
 
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered 50 into the calculator
And I have entered 70 into the calculator
 
When I press add
```

</br>
</br>

4- Then :

> Then keyword defines the outcome of previous steps.

```feature
Feature: Calculator
 
Calculator for adding two numbers
 
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered 50 into the calculator
And I have entered 70 into the calculator
 
When I press add
Then the result should be 120 on the screen
```
</br>
</br>


5- Tags:
> You can place tags above Feature to group related features, independent of your file and directory structure.

 Furthermore you can filter tags within your CI/CD-pipeline.

```feature 
Feature: Calculator
 
Calculator for adding two numbers
 
@mytag
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered 50 into the calculator
And I have entered 70 into the calculator
 
When I press add
Then the result should be 120 on the screen

```

</br>
</br>

6- Delimited parameters :

> Delimited parameters <> are used as references which are referred to in the example tables. SpecFlow will automatically replace these parameters when executing  tests.

```feature
Feature: Calculator
 
Calculator for adding two numbers
 
@mytag
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered <First> into the calculator
And I have entered <Second> into the calculator
 
When I press add
Then the result should be <Result> on the screen
```

</br>
</br>

7- Data tables:

> Data Tables are handy for passing a list of values to a step definition. A useful tool in complex Gherkin examples.

```feature
Feature: Calculator
 
Calculator for adding two numbers
 
@mytag
Scenario: Add two numbers
Add two numbers with the calculator
Given I have entered <First> into the calculator
And I have entered <Second> into the calculator
 
When I press add
Then the result should be <Result> on the screen
Examples:
| First | Second | Result |
| 50 | 70 | 120 |
| 30 | 40 | 70 |
| 60 | 30 | 90 |
```

</br>
</br>


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

### To run other browser test drom CLI:

```cmd
pwsh .\bin\Debug\net6.0\playwright.ps1  codegen  -b webkit "url"
```


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

### Another Example with outline in Scenario :

--- feature file 

```feature 
@Calculator
Feature: Calculator
![Calculator](https://specflow.org/wp-content/uploads/2020/09/calculator.png)
Simple calculator for adding **two** numbers

Link to a feature: [Calculator](CalculatorSelenium.Specs/Features/Calculator.feature)
***Further read***: **[Learn more about how to generate Living Documentation](https://docs.specflow.org/projects/specflow-livingdoc/en/latest/LivingDocGenerator/Generating-Documentation.html)**

Scenario: Add two numbers
	Given the first number is 50
	And the second number is 70
	When the two numbers are added
	Then the result should be 120


Scenario Outline: Add two numbers permutations
	Given the first number is <First number>
	And the second number is <Second number>
	When the two numbers are added
	Then the result should be <Expected result>

Examples:
	| First number | Second number | Expected result |
	| 0            | 0             | 0               |
	| -1           | 10            | 9               |
	| 6            | 9             | 15              |

```

</br>
</br>

--- step.cs file

```c#
using CalculatorSelenium.Specs.Drivers;
using CalculatorSelenium.Specs.PageObjects;
using FluentAssertions;
using TechTalk.SpecFlow;

namespace CalculatorSelenium.Specs.Steps
{
    [Binding]
    public sealed class CalculatorStepDefinitions
    {
        //Page Object for Calculator
        private readonly CalculatorPageObject _calculatorPageObject;

        public CalculatorStepDefinitions(BrowserDriver browserDriver)
        {
            _calculatorPageObject = new CalculatorPageObject(browserDriver.Current);
        }

        [Given("the first number is (.*)")]
        public void GivenTheFirstNumberIs(int number)
        {
            //delegate to Page Object
            _calculatorPageObject.EnterFirstNumber(number.ToString());
        }

        [Given("the second number is (.*)")]
        public void GivenTheSecondNumberIs(int number)
        {
            //delegate to Page Object
            _calculatorPageObject.EnterSecondNumber(number.ToString());
        }

        [When("the two numbers are added")]
        public void WhenTheTwoNumbersAreAdded()
        {
            //delegate to Page Object
            _calculatorPageObject.ClickAdd();
        }

        [Then("the result should be (.*)")]
        public void ThenTheResultShouldBe(int expectedResult)
        {
            //delegate to Page Object
            var actualResult = _calculatorPageObject.WaitForNonEmptyResult();

            actualResult.Should().Be(expectedResult.ToString());
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


</br>
</br>

### A Long Example :

--- Feature --TestingCardDialog-- file:

```feature
Feature: TestingCardDialog

Created By Omar J Zant

#add card to  the user click the title of any cardems
Scenario: add card to task item
	Given the logged out user opened the login page
	When the user click on add icon on top right  the toolbar
	And select the type equal bug
	And enter a title for this new card
	And press out the card 
	Then a new card will be added

Example: nbr of milestone

#Close dialog
Scenario: close dialog when clicking the x button in dialog
	Given the logged out user opened the login page
	When the user click the title of new card
	And the user click the x button in dialog
	Then the dialog is closed 

#same id between card and dialog 
Scenario: get the same id between card and dialog
	Given the logged out user opened the login page
	When the user click the title of new card after edit title
	Then the card Id and the id on the top of dialog is equal

#edit title of card from dialog 
Scenario: edit the title of dialog
	Given the logged out user opened the login page
	When the user click the title of new card
	And enter edited text
	And press out
	Then the title card is edited

#edit assignee of card from dialog 
Scenario: edit assignee item
	Given the logged out user opened the login page
	When the user click the title of new card
	And click on the assignee
	And the user select one assignee
	And reload page
	Then the assignee card is edited
	
#the item Type in dialog is same to type of card
Scenario: same type between item type in dialog and card
	Given the logged out user opened the login page
	When the user click the title of new card after edit title
	Then the item type equal to type of card

# edit date of card from dialog
Scenario: edit the date picker 
	Given the logged out user opened the login page
	When the user click the title of new card
	And click on the due date picker
	And select a month
	And click on the due date picker
	And select day
	And press out
	And reload page
	Then the date is same in card and in dialog

#edit estimate of card from dialog 
Scenario: edit the estimate  
	Given the logged out user opened the login page
	When the user click the title of new card 
	And click the estimate 
	And change the estimate value
	And press enter
	Then the estimate in the card was edited

#edit tags of card from dialog 
Scenario: edit tag from dialog
	Given the logged out user opened the login page
	When the user click the title of new card
	And click on the tag dropDown List
	And select tags from tag dropdownlist in dialog
		| Tag1    | Tag2          |
		| Project | High Priority |
	And press out
	And reload page
	Then the same tags was enterred is same to card's tag

#delete one of the tags from dialog 
Scenario: delete one of the tag
	Given the logged out user opened the login page
	When the user click the title of new card
	And click on the tag dropDown List
	And select tags from tag dropdownlist in dialog
		| Tag1    | Tag2          |
		| Quality | High Priority |
	And press out
	And reload page
	And the user click the title of new card
	And click on the tag dropDown List
	And click on x button of the tag
		| removeTag |
		| High Priority |
	And press out

	And reload page
	Then the deleted tag will be removed from the card 

#delete  all tags selected 
Scenario: delete all tags selected in dialog
	Given the logged out user opened the login page
	When the user click the title of new card
	And click on the tag dropDown List
	And select tags from tag dropdownlist in dialog
		| Tag1    | Tag2          |
		| Quality | High Priority |
	And press out
	And reload page
	And the user click the title of new card
	And click on the tag dropDown List
	And click x button of the dropdown List
	And press out
	And reload page
	Then all the ags will be deleted

#Margin the dialog to fullScreen
Scenario: make dialog on fullScreen
	Given the logged out user opened the login page
	When the user click the title of new card after edit title
	And click the expand icon
	Then the dialog margin to  fullscreen


#Margin the dialog  in normal size
Scenario: make dialog in normal size
	Given the logged out user opened the login page
	When the user click the title of new card after edit title
	And click the expand icon
	And click the collapse icon
	Then the dialog  in normal size
	
#testing rich text editor --add text--
Scenario: add text to description in dialog 
	Given the logged out user opened the login page
	When the user click the title of new card
	And click on the description input
	And enter a description 
	And press out
	Then the description is saved in dialog of the same card only
	
#testing rich text editor --add text with link--
Scenario: test link description text in dialog 
	Given the logged out user opened the login page
	When the user click the title of new card after edit title
	And click on the description input
	And select the text
	And click on the link button
	And select the text
	And click on the link button
	And enter the link
	And press on save button
	And press out
	Then the link is added to text

#testing if the link entered in description is run right
Scenario: test link description if run in dialog 
	Given the logged out user opened the login page
	When the user click the title of new card after edit title
	And click on the description input
	And enter a text to description
	And select the text
	And click on the link button
	And enter the link
	And press on save button
	And press out
	And click on this link
	Then the page was redirect to the link entered

```

</br>
</br>

--- feature --Filters-- file:

```feature

Feature: Filters

Created by Noura Hlayhel


@types
Scenario Outline: Filter By types
	Given the logged out user opened the login page
	When the user chooses to filter items and clicks the filter button
	And the user chooses to filter by a certain type <typeOfItem>
	Then only this type will show up
Examples: 
| typeOfItem  |
| Task        |
| Bug         |
| Impediment  |
| Development |
| GUI Design  |


@filters
Scenario Outline: Filter By Assignee
	Given the logged out user opened the login page
	When the user chooses to filter items and clicks the filter by assignee
	And the user chooses checks <assignee_name>
	Then only items assigned to the user should be shown
Examples: 
| assignee_name |
| Sirine Osman  |
| Hala Taleb    |


@filterbytag
Scenario Outline: Filter By Tag
	Given the logged out user opened the login page
	When the user chooses to filter items and clicks the filter by tag
	And the user checks whatever <tag> he wants
	Then only items related to this tag should be shown
Examples: 
| tag           |
| High Priority |
| Project       |


@morethanoneassignee
Scenario: Filter By More than one Assignee
	Given the logged out user opened the login page
	When the user chooses to filter items and clicks the filter by assignee
	And the user chooses checks Sirine Osman
	And the user chooses checks Hala Taleb
	Then only items assigned to the chosen assignees should be shown


@multi-filtering
Scenario: User filter the Items list by the Assignee and the Tags (Or any combination of filters)
	Given the logged out user opened the login page
	When the user chooses to filter items and clicks the filter by assignee
	And the user chooses checks Sirine Osman
	And the user chooses to filter items and clicks the filter button
	And the user chooses to filter by a certain type Development
	Then only items assigned to the user should be shown
	And only this type will show up
```

</br>
</br>

--- step.cs

```c#
using exactAutomationTest.Drivers;
using Microsoft.Playwright;
using NUnit.Framework;
using System.Dynamic;
using TechTalk.SpecFlow.Assist;

namespace exactAutomationTest.StepDefinitions
{

    [Binding]
    [TestFixture]
    public class ExactStepdefinitions : Driver
    {
        private const string EmailInputSelector = "#email";
        private const string PasswordInputSelector = "#password";
        private const string SignInButtonSelector = "text='Sign In'";
        private const string searchIcon = "span.search-icon";
        private const string searchInput = "input.search-input";
        private const string ValidEmail = "ozant@osdservices.com";
        private const string ValidPassword = "Novo123$";
        private const string ValidTextForSearch = "Automation";
        private const string assigneeCard = "span.assignee-name";
        private const string estimateCard = "span.e-editable-value";
        private const string tagCard = "span.chip.closable span.text";
        private const string titleAddCardToBugs = "Test Bugs -Carddialog-";
        private const string titleAddCardToBugsEdited = "Test Bugs -Carddialog- text edited";
        private const string assigneeAddCardToBugsEdited = "Omar Zant";
        private const string descriptionTestInDialog = "Test description automation in dialog";
        private const string descriptionTestInDialogLink = "Link";
        private const string testURLDescription = "https://angular.io/";
        private string testURLDescriptionSaveLink = "";
        public const string selectMonth = "Oct";
        public const string selectDay = "15";
        private const string estimateAddCardToBugsEdited = "3";
        private const string itemTypeDialogCardBug = "Bug";
        private string newCardId = "";
        private List<string>? tagsDialog;


        private const string tasksList = "cdk-drop-list-12";
        private const string developmentsList = "cdk-drop-list-0";
        private const string guiDesignsList = "cdk-drop-list-18";
        private const string bugsList = "cdk-drop-list-8";
        private const string impedimentsList = "cdk-drop-list-15";
        private string type = "";
        private string? assignee_name;
        private string? tag;


        public async Task ClickSignIn()
        {
            await this.page.Locator(SignInButtonSelector).ClickAsync();
        }

        public async Task FillCredentials()
        {
            await this.page.Locator(EmailInputSelector).FillAsync(ValidEmail);

            await this.page.Locator(PasswordInputSelector).FillAsync(ValidPassword);
    }

        public void GetIdCard(string id) => this.newCardId = id;

        //*****************************************************************


        [Given("the logged out user opened the login page")]
        public async Task OpenPage()
        {
            await this.page.GotoAsync(GetUrl());
            await this.FillCredentials();
            await this.ClickSignIn();
        }


        //*****************************************************************


        /// <summary>
        ///     ItemToolbar automation
        /// </summary>

        [When("the user click on the search icon")]
        public async Task ClickSearchIcon() => await this.page.Locator(searchIcon).ClickAsync();

        [When("the user click on the input of search")]
        public async Task ClickSearchInput() => await this.page.Locator(searchInput).ClickAsync();

        [When("the user enter a valid text in search")]
        public async Task FillSearchInput()
        {
            await this.page.Locator(searchInput).FillAsync("Automation");
            await this.page.Keyboard.PressAsync("Enter");
        }

        [When("the user enter an invalid text in search")]
        public async Task WhenTheUserEnterAnInvalidTextInSearch()
        {
            await this.page.Locator(searchInput).FillAsync("assss");
        }



        [When("the user click on add icon on top right  the toolbar")]
        public async Task ClickAddIconToAddTask()
        {
            await this.page.Locator("#toolbar-footer").GetByRole(AriaRole.Button).ClickAsync();
        }

        [When("select the type of item")]
        public async Task WhenSelectTheTypeOfItem()
        {
            await this.page.GetByRole(AriaRole.Link, new() { NameString = "Tasks" }).ClickAsync();
            var countTodoListOld = await this.page.Locator("#cdk-drop-list-0 div").Nth(2).CountAsync();
            Console.WriteLine(countTodoListOld);
        }

        [When("create title for new card")]
        public async Task WhenCreateTitleForNewCard()
        {
            await this.page.Locator("#toolbar-footer").GetByRole(AriaRole.Button).ClickAsync();
            await this.page.GetByRole(AriaRole.Link, new() { NameString = "Tasks" }).ClickAsync();
            await this.page.Locator("app-card:has-text(\"AssigneeUnassignedDue dateEstimateEmptyTags High Priority Quality Project Admini\")").GetByPlaceholder("Add title").FillAsync("gfhjgghjghj");
            await this.page.Locator("#toolbar-footer").ClickAsync();
        }


        [When("the user click avatar on top right the toolbar")]
        public async Task WhenTheUserClickAvatarOnTopRightThToolbar()
        {
            await this.page.Locator("#toolbar-footer").GetByRole(AriaRole.Presentation).ClickAsync();
        }

        [When("the user click log out")]
        public async Task WhenTheUserClickLogOut() => await this.page.GetByRole(AriaRole.Button, new() { NameString = "Log Out" }).ClickAsync();


        [When("the user click the exact icon")]
        public async Task WhenTheUserClickTheExactIcon() => await this.page.Locator("#toolbar").GetByRole(AriaRole.Img).ClickAsync();


        ///////////////////////////////////////////////////////////////////////////////////////////////


        [When("the user click the title of new card")]
        public async Task WhenTheUserClickTheTitleOfAnyCard()
        {
            await this.page.Locator($"#{bugsList}").GetByText(titleAddCardToBugs).First.ClickAsync();
        }

        [When("the user click the title of new card after edit title")]
        public async Task WhenTheUserClickTheTitleOfAnyCardAfterEditTitle()
        {
            await this.page.GetByText(titleAddCardToBugsEdited).ClickAsync();
        }


        [When("click on the title in Dialog")]
        public async Task WhenClickOnTheTitleInDialog() => await this.page.Locator("exact-sidebar-modal").GetByText(titleAddCardToBugs).ClickAsync();

        [When("enter edited text")]
        public async Task WhenEnterEditedText() => await this.page.FillAsync($"exact-sidebar-modal .title-input:has-text('{titleAddCardToBugs}')", titleAddCardToBugsEdited);

        [When("click out the title")]
        public async Task WhenClickOutTheTitle() => await this.page.Locator("#toolbar-footer").ClickAsync();

        [When("reload page")]
        public async Task WhenReloadPage() => await this.page.GotoAsync("https://exact-dev.osdservices.com/");



        [When("click on the assignee")]
        public async Task WhenClickOnTheAssignee() => await this.page.Locator("exact-sidebar-modal").GetByText("Unassigned").ClickAsync();

        [When("the user select one assignee")]
        public async Task WhenTheUserSelectOneAssignee() => await this.page.Locator("exact-sidebar-modal").GetByText(assigneeAddCardToBugsEdited).ClickAsync();


        [When("click on the due date picker")]
        public async Task WhenClickOnTheDueDatePicker() => await this.page.Locator("exact-sidebar-modal exact-date-picker .input").ClickAsync();


        [When("select a month")]
        public async Task WhenSelectADate()
        {
            await this.page.Locator(".pointer.selected-month").ClickAsync();
            await this.page.Locator($"div:has-text('{selectMonth}')").First.ClickAsync();
        }

        [When("select day")]
        public async Task WhenSelectDay()
        {
            await this.page.Locator($"span .number:has-text('{selectDay}')").ClickAsync();
        }



        [When("the user click the x button in dialog")]
        public async Task WhenTheUserClickTheXButtonInDialog() => await this.page.Locator("fa-icon:nth-child(3) > .svg-inline--fa").ClickAsync();

        [When("click on add icon on the top right the toolbar")]
        public async Task WhenClickOnAddIconOnTheTopRightTheToolbar() => await this.page.Locator("#toolbar-footer").GetByRole(AriaRole.Button).ClickAsync();

        [When("select the type equal bug")]
        public async Task WhenSelectTheTypeEqualBug()
        {
            await this.page.Locator("a:has-text(\"Bug\")").ClickAsync();
        }

        [When("enter a title for this new card")]
        public async Task WhenEnterATitleForThisNewCard() => await this.page.GetByRole(AriaRole.Textbox, new() { NameString = "Add title" }).FillAsync(titleAddCardToBugs);

        [When("press out")]
        public async Task WhenPressOut() => await this.page.Locator("exact-sidebar-modal").GetByText("Estimate").First.ClickAsync();


        [When("press out the card")]
        public async Task WhenPressOutTheCardAsync() => await this.page.Locator($"#{bugsList} div:has-text(\"To Do\")").Nth(1).ClickAsync();

        [When("reload thethis.page")]
        public async Task WhenReloadThePage() => await this.page.WaitForURLAsync("https://exact-dev.osdservices.com/");


        [When("click the estimate")]
        public async Task WhenClickTheEstimate() => await this.page.Locator("exact-sidebar-modal #numericInput").ClickAsync();

        [When("change the estimate value")]
        public async Task WhenChangeTheEstimateValue() => await this.page.Locator("exact-sidebar-modal #numericInput").FillAsync(estimateAddCardToBugsEdited);

        [When("press enter")]
        public async Task WhenPressEnter() => await this.page.Locator("exact-sidebar-modal #numericInput").PressAsync("Enter");


        [When("click on the tag dropDown List")]
        public async Task WhenClickOnTheTagDropDownList() => await this.page.Locator("exact-sidebar-modal .chips-input").ClickAsync();


        [When("select tags from tag dropdownlist in dialog")]
        public async Task WhenSelectTagsFromTagDropdownlistInDialog(Table table)
        {
            dynamic data = GetData(table);
            this.tagsDialog = new List<string>
            {
                (string)data.Tag2,
                "\n",
                (string)data.Tag1,
            };
            await this.page.Locator($"exact-sidebar-modal exact-input-chips div:has-text('{data.Tag1}')").Nth(4).ClickAsync();
            await this.page.Locator($"exact-sidebar-modal exact-input-chips div:has-text('{data.Tag2}')").Nth(4).ClickAsync();

        }
        private static ExpandoObject GetData(Table table)
        {
            return table.CreateDynamicInstance();
        }


        [When("click on x button of the tag")]
        public async Task WhenClickOnXButtonOfTheTag(Table table)
        {
            dynamic data = table.CreateDynamicInstance();
            await this.page.HoverAsync("exact-sidebar-modal exact-input-chips span.chip");
            await this.page.WaitForTimeoutAsync(2000);
            await this.page.Locator($"exact-sidebar-modal exact-input-chips span:has-text('{data.removeTag}') fa-icon").ClickAsync();
        }

        [When("click x button of the dropdown List")]
        public async Task WhenClickXButtonOfTheDropdownList()
        {
            await this.page.HoverAsync("exact-sidebar-modal exact-input-chips span.chip");
            await this.page.WaitForTimeoutAsync(1000);
            await this.page.Locator("exact-sidebar-modal exact-input-chips .close").ClickAsync();
        }


        [When("click the expand icon")]
        public async Task WhenClickTheExpandIcon() => await this.page.Locator(".header-icons-wrapper > fa-icon > .svg-inline--fa").First.ClickAsync();

        [When("click the collapse icon")]
        public async Task WhenClickTheCollapseIcon() => await this.page.Locator(".header-icons-wrapper > fa-icon > .svg-inline--fa").First.ClickAsync();

        [When("click on the description input")]
        public async Task WhenClickOnTheDescriptionInput() => await this.page.Locator(".rich-text-editor-static").First.ClickAsync();

        [When("enter a description")]
        public async Task WhenEnterADescription()
        {
            await this.page.Locator("p-editor:has-text(\"HeadingSubheadingNormal centerrightjustify\")").GetByRole(AriaRole.Paragraph).FillAsync(descriptionTestInDialog);
            await this.page.GetByText(descriptionTestInDialog).PressAsync("Enter");
        }

        [When("enter a text to description")]
        public async Task WhenEnterATextToDescription() => await this.page.Locator("p-editor:has-text(\"HeadingSubheadingNormal centerrightjustify\")").GetByRole(AriaRole.Paragraph).FillAsync(descriptionTestInDialogLink);

        [When("select the text")]
        public async Task WhenAddALinkToThisText()
        {
            await this.page.Locator($"p:has-text('{descriptionTestInDialogLink}')").SelectTextAsync();
        }

        [When("click on the link button")]
        public async Task WhenClickOnTheLinkButton() => await this.page.Locator("p-editor button.ql-link").ClickAsync();

        [When("enter the link")]
        public async Task WhenEnterTheLink() => await this.page.GetByPlaceholder("https://quilljs.com").FillAsync(testURLDescription);

        [When("press on save button")]
        public async Task WhenPressOnSaveButton() => await this.page.Locator("p-editor:has-text(\"HeadingSubheadingNormal centerrightjustify Link\") a").Nth(1).ClickAsync();

        [When("click on this link")]
        public async Task WhenClickOnThisLink()
        {
            await this.page.Locator("exact-rich-text-editor a").EvaluateAsync("el => el.removeAttribute('target')");
            await this.page.GetByRole(AriaRole.Link, new() { NameString = "Link" }).ClickAsync();
            this.testURLDescriptionSaveLink =this.page.Url;
            await this.page.GotoAsync("https://exact-dev.osdservices.com/");
        }




        // *****************  Created By Noura ************** //

        // *****************  Filters *********************** // 

        [When(@"the user chooses to filter items and clicks the filter button")]
        public async Task WhenTheUserChoosesToFilterItemsAndClicksTheFilterButton()
        {
            await page.GetByRole(AriaRole.Button).Nth(1).ClickAsync();
            await page.GetByRole(AriaRole.Button).Nth(2).ClickAsync();
        }

        [When(@"the user chooses to filter by a certain type  (.*) Bug of Item")]
        public async Task WhenTheUserChoosesToFilterByACertainTypeBugOfItem(string typeOfItem)
        {
            await page.GetByLabel(typeOfItem).CheckAsync();
            this.type = typeOfItem;
        }

        [Then("only this type will show up")]
        public async Task CheckFilteringByTypes()
        {

            var tasks = await page.GetByRole(AriaRole.Heading, new() { NameString = "Tasks" }).IsVisibleAsync();
            if (type == "Tasks") 
                Assert.IsTrue(tasks);
            else 
                Assert.IsFalse(tasks);

            var bugs = await page.GetByRole(AriaRole.Heading, new() { NameString = "Bugs" }).IsVisibleAsync();
            if (type == "Bugs")
                Assert.IsTrue(bugs);
            else 
                Assert.IsFalse(bugs);

            var developments = await page.GetByRole(AriaRole.Heading, new() { NameString = "Developments" }).IsVisibleAsync();
            if (type == "Developments")
                Assert.IsTrue(developments);
            else 
                Assert.IsFalse(developments);

            var impediments = await page.GetByRole(AriaRole.Heading, new() { NameString = "Impediments" }).IsVisibleAsync();
            if (type == "Impediments") 
                Assert.IsTrue(impediments);
            else
                Assert.IsFalse(impediments);

            var guiDesign = await page.GetByRole(AriaRole.Heading, new() { NameString = "GUI Designs" }).IsVisibleAsync();
            if (type == "GUI Designs")
                Assert.IsTrue(guiDesign);
            else
                Assert.IsFalse(guiDesign);
        }


        [When("the user chooses to filter items and clicks the filter by assignee")]
        public async Task FilteringbyAssignee()
        {
            await page.ReloadAsync();
            await page.GetByRole(AriaRole.Button).Nth(2).ClickAsync();
        }

        [When(@"the user chooses checks (.*)")]
        public async Task ChoosingAssignee(string assignee_name)
        {
            await page.GetByLabel(assignee_name).CheckAsync();
            this.assignee_name = assignee_name;
        }

        [Then("only items assigned to the user should be shown")]
        public void CheckFilteringByAssignee()
        {
            var texts = page.Locator(".assignee-name").AllInnerTextsAsync();
            var count = texts.Result.Count();
            for (int i = 0; i < count; i++)

            {
                var element = texts.Result[i].ToString();
                Assert.IsTrue(element == assignee_name);
            }
        }


        [When("the user chooses to filter items and clicks the filter by tag")]
        public async Task FilteringbyTag()
        {
            await page.GetByRole(AriaRole.Button).Nth(3).ClickAsync();
            await page.GetByRole(AriaRole.Button).Nth(4).ClickAsync();
        }

        [When(@"the user checks whatever (.*) he wants")]
        public async Task ChoosingTag(string tag)
        {
            await page.GetByLabel(tag).CheckAsync();
            this.tag = tag;
        }

        [Then("only items related to this tag should be shown")]
        public void CheckFilteringByTag()
        {
            int cardCount = page.Locator("div.card").CountAsync().Result; ;
            var tagcount = 0;
            var texts = page.Locator(".text").AllInnerTextsAsync();
            var count = texts.Result.Count();
            
            for (int i = 0; i < count; i++)
            {
                var element = texts.Result[i].ToString();
                if (element == tag)
                    tagcount++;
            }
            Assert.AreEqual(cardCount, tagcount);
        }


        [Then("only items assigned to the chosen assignees should be shown")]
        public void CheckFilteringByAssignees()
        {
            var texts = page.Locator(".assignee-name").AllInnerTextsAsync();
            var count = texts.Result.Count();

            for (int i = 0; i < count; i++)
            {
                var element = texts.Result[i].ToString();
                Assert.IsTrue(element == "Sirine Osman" || element == "Hala Taleb");
            }
        }


        [When(@"the user chooses to filter by a certain type (.*)")]
        public async Task FilterByType(string type)
        {
            await page.GetByLabel(type).CheckAsync();
            this.type = type;
        }


        // *************************************************************** //
        // *************************** Accordion Item ******************** //

        [When(@"the user clicks on the arrow next to item (.*)")]
        public async Task ArrowClick(string type)
        {
            await page.Locator("h3.board-title").GetByText(type).ClickAsync();
        }

        [Then(@"all items of this (.*) should disappear")]
        public async Task HideItems(string type)
        {
            if (type == "Task")
            {
                await Assertions.Expect(page.Locator($"cdk-drop-list-{tasksList}")).ToBeHiddenAsync();
            }
            if (type == "Bug")
            {
                await Assertions.Expect(page.Locator(bugsList)).ToBeHiddenAsync();
            }
            if (type == "Impediment")
            {
                await Assertions.Expect(page.Locator(impedimentsList)).ToBeHiddenAsync();
            }
            if (type == "Development")
            {
                await Assertions.Expect(page.Locator(developmentsList)).ToBeHiddenAsync();
            }
            if (type == "GUI Design")
            {
                await Assertions.Expect(page.Locator(guiDesignsList)).ToBeHiddenAsync();
            }
        }

        // **************************************************************** //

        // ************************************************** //





        //*****************************************************************




        [Then("the user should be redirected to the homepage")]
        public async Task GoToItemsPage()
        {
            var item = await this.page.Locator("p:has-text('Items')").IsVisibleAsync();
            Assert.IsTrue(item);
        }


        [Then("all card not visible")]
        public async Task ThenAllCardNotVisibleAsync()
        {
            var card = await this.page.Locator("card themes-bg-card ng-star-inserted").IsVisibleAsync();
            Assert.IsFalse(card);
        }

        [Then("the page redirect to sign in page")]
        public async Task ThenThePageRedirectToSignInPage()
        {
            var signInText = await this.page.Locator(SignInButtonSelector).IsVisibleAsync();
            Assert.IsTrue(signInText);
        }

        [Then("the navbar text will be hidden")]
        public async Task ThenTheNavbarTextWillBeHidden()
        {
            var disableNav = await this.page.Locator("nav:has-text(\"InsightsAnalyticsItemsDocumentsSheetsMessagesLinksCalendarProjectsAutomationSupp\")").IsVisibleAsync();
            Assert.IsTrue(!disableNav);
        }


        [Then("the dialog will be open")]
        public async Task ThenTheDialogWillBeOpen()
        {
            var dialog = await this.page.GetByText("135Save Item On Enter Click").IsVisibleAsync();
            Assert.IsTrue(dialog);
        }


        [Then("the dialog is closed")]
        public async Task ThenTheDialogIsClosed()
        {
            bool dialogIsVisible = await this.page.Locator("exact-sidebar-modal").GetByText(titleAddCardToBugsEdited).Nth(1).IsVisibleAsync();
            Assert.IsFalse(dialogIsVisible);
        }


        [Then("the title card is edited")]
        public void ThenTheTitleCardIsEdited()
        {
            string selector = $"#cdk-drop-list-{bugsList} div.title:has-text('{titleAddCardToBugsEdited}')";
            string editedTitleIsVisible =this.page.Locator(selector).First.InnerTextAsync().Result.ToString();
            Assert.AreEqual(editedTitleIsVisible, titleAddCardToBugsEdited);
        }

        [Then("the date is same in card and in dialog")]
        public void ThenTheDateIsEditedInThisCard()
        {
            var editedDate =this.page.Locator($"#cdk-drop-list-{bugsList} exact-date-picker input").First.InputValueAsync().Result.ToString();
            Assert.AreEqual(editedDate, "15/10/2022");
        }


        [Then("the assignee card is edited")]
        public void ThenTheAssigneeCardIsEdited()
        {
            string editedAssignee =this.page.Locator($"#cdk-drop-list-{bugsList} .assignee-name:has-text('{assigneeAddCardToBugsEdited}')")
                    .First
                    .InnerTextAsync()
                    .Result
                    .ToString();

            AssertionException.Equals(editedAssignee, assigneeAddCardToBugsEdited);
        }

        [Then("the item type equal to type of card")]
        public async Task ThenTheItemTypeEqualToTypeOfCard()
        {
            bool sameType = await
            this.page.Locator("exact-sidebar-modal").GetByText(itemTypeDialogCardBug).Nth(1).IsVisibleAsync();
            Assert.IsTrue(sameType);
        }


        [Then("the estimate in the card was edited")]
        public void ThenTheEstimateInTheCardWasEdited()
        {
            string selector = $"#cdk-drop-list-{bugsList} #numericInput";
            string editedEstimate =this.page.Locator(selector).First.InnerTextAsync().Result.ToString();
            AssertionException.Equals(editedEstimate, estimateAddCardToBugsEdited);
        }


        [Then("a new card will be added")]
        public void ThenANewCardWillBeAdded()
        {
            string selector = $"#cdk-drop-list-{bugsList} div.title:has-text('{titleAddCardToBugs}')";
            string editedTitleIsVisible =this.page.Locator(selector).First.InnerTextAsync().Result.ToString();
            string newCardId =this.page.Locator($"#cdk-drop-list-{bugsList}  app-card .cdk-drag-handle").First.InnerTextAsync().Result.ToString();
            this.GetIdCard(newCardId);
            AssertionException.Equals(editedTitleIsVisible, titleAddCardToBugs);
        }

        [Then("the card Id and the id on the top of dialog is equal")]
        public void ThenCtheCardIdAndTheIdOnTheTopOfDialogIsEqual()
        {
            string idIsSame = this.page.Locator("exact-sidebar-modal .themes-color-card-label").First.InnerTextAsync().Result.ToString();
            AssertionException.Equals(idIsSame, this.newCardId);
        }

        [Then("the same tags was enterred is same to card's tag")]
        public void ThenTheSameTagsWasEnterredIsSameToCardsTag()
        {
            this.page.Locator($"#cdk-drop-list-{bugsList} .chips-input").ClickAsync();
            var tags = this.page.Locator($"#cdk-drop-list-{bugsList} .chips-wrapper").First.InnerTextAsync().Result.ToString();
            Assert.AreEqual(tags, "High Priority\nQuality\nProject");
        }

        [Then("the deleted tag will be removed from the card")]
        public void ThenTheDeletedTagWillBeRemovedFromTheCard()
        {
            this.page.Locator($"#cdk-drop-list-{bugsList} .chips-input").ClickAsync();
            var tags = this.page.Locator($"#cdk-drop-list-{bugsList} .chips-wrapper").First.InnerTextAsync().Result.ToString();
            Assert.AreEqual(tags, "Quality");
        }

        [Then("all the ags will be deleted")]
        public void ThenAllTheAgsWillBeDeleted()
        {
           this.page.Locator($"#cdk-drop-list-{bugsList} .chips-input").ClickAsync();
            var tags =this.page.Locator($"#cdk-drop-list-{bugsList} .chips-wrapper").First.InnerTextAsync().Result.ToString();
            Assert.AreEqual(tags, "");
        }

        [Then("the dialog margin to  fullscreen")]
        public async Task ThenTheDialogMarginToFullscreen()
        {
            bool dialogFullScreenAvailable = await this.page.Locator("exact-sidebar-modal div.expand").IsVisibleAsync();
            Assert.IsTrue(dialogFullScreenAvailable);
        }

        [Then("the dialog  in normal size")]
        public async Task ThenTheDialogInNormalSize()
        {
            bool dialogFullScreenAvailable = await this.page.Locator("exact-sidebar-modal div.expand").IsVisibleAsync();
            Assert.IsFalse(dialogFullScreenAvailable);
        }

        [Then("the description is saved in dialog of the same card only")]
        public async Task ThenTheDescriptionIsSavedInDialogOfTheSameCardOnly()
        {
            bool sameDescriptionIsVisible = await this.page.GetByText(descriptionTestInDialog).IsVisibleAsync();
            Assert.IsTrue(sameDescriptionIsVisible);
        }

        [Then("the link is added to text")]
        public async void ThenTheLinkIsAddedToText()
        {
            bool textWithLinkIsVisible = await this.page.Locator("exact-rich-text-editor p a").GetByText(descriptionTestInDialogLink).IsVisibleAsync();
            Assert.IsTrue(textWithLinkIsVisible);
        }


        [Then("the page was redirect to the link entered")]
        public void ThenThePageWasRedirectToTheLinkEntered()
        {
            Assert.AreEqual(testURLDescription, this.testURLDescriptionSaveLink);
        }


        //*****************************************************************

    }
}

```
