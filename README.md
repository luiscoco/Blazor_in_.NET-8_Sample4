# Blazor in .NET 8: Sample 4

**Example: Survey Form**

Features Demonstrated:

**Forms and Input Binding**: Creating multi-field forms with different input types

**Validation**: Enforcing rules on user input

**Component Communication**: Passing data and triggering events between components

**Setup**

Create a new Blazor Server project (or reuse an existing one). You can name it "SurveyApp"

**Models**

Create a Models folder. Add a SurveyResponse.cs class:

```csharp
namespace SurveyApp.Models;

public class SurveyResponse
{
    public string Name { get; set; }
    public string Email { get; set; }
    public int Age { get; set; }
    public string FavoriteFramework { get; set; }
    public string Feedback { get; set; }
}
```

**Components**

**SurveyForm.razor: (Inside Pages)**

```cshtml
@page "/survey"

<h3>Survey Form</h3>

<EditForm Model="@survey" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator /> 

     <div class="form-group">
         <label for="name">Name:</label>
         <InputText id="name" class="form-control" @bind-Value="survey.Name" />
         <ValidationMessage For="@(() => survey.Name)" /> 
     </div>

    <div class="form-group">
        ... (Similar input fields for Email, Age, etc.)
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private SurveyResponse survey = new SurveyResponse();

    private void HandleValidSubmit()
    {
        // Logic to submit the survey data (e.g., send to a server)
        Console.WriteLine("Survey submitted!"); // Placeholder for now
    }
}
```

**SurveyConfirmation.razor (Inside Pages)**

```cshtml
@page "/survey/confirmation"

<h3>Thank you for your feedback!</h3>

<p>Your input is valuable. Here's the summary of your submission:</p>

<dl>
    <dt>Name:</dt>
    <dd>@SurveyData.Name</dd>
    <dt>Email:</dt>
    <dd>@SurveyData.Email</dd>
    ... 
</dl>

@code {
    [Parameter] public SurveyResponse SurveyData { get; set; }
}
```

**Updating App.razor**

Make a small change to your App.razor file to include routing for the survey form and its confirmation:

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```


