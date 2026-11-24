# C# SDK (/sdks/csharp)

[Download SDK](/__downloads/sdk/csharp.zip) · [View source](https://github.com/ombrevix/billing-sdk-dotnet) · [View on NuGet](https://www.nuget.org/packages/Ombrevix.Billing.Sdk)

## Intro [#intro]

The Ombrevix Billing SDK provides access to the Ombrevix Billing REST APIs.

Ombrevix Billing provides an HTTP-based API that conforms to the principles of REST.
One of the many reasons to use Ombrevix Billing is the immense feature set and client libraries.
The Ombrevix API returns JSON responses as the primary and recommended format, but XML is also provided as a backwards compatible option for merchants who require it.

## Steps to make your first Ombrevix Billing API call [#steps-to-make-your-first-ombrevix-billing-api-call]

1. Sign-up or log-in to your test workspace account.
2. Setup authentication credentials.
3. Submit an API request and verify the response.
5. Test the Ombrevix Billing integrations.

Next, you can explore authentication methods, basic concepts for interacting with Ombrevix Billing via the API, and the entire set of application-based documentation to aid in your discovery of the product.

### Request Example [#request-example]

The following example uses the curl command-line tool to make an API request.

**Request**

    curl -u <api_key>:x -H Accept:application/json -H Content-Type:application/json https://acme.ombrevix.example/subscriptions

## Installation [#installation]

To add the SDK to your project from NuGet:

```bash
dotnet add package Ombrevix.Billing.Sdk
```

To build against the SDK source instead, add it as a project reference into your solution:

```bash
dotnet add reference <path-to-sdk>/src/OmbrevixBilling/OmbrevixBilling.csproj
```

The project takes its build settings from the files around it, so reference it in place.

## Quick Start [#quick-start]

### Dependency Injection [#dependency-injection]

Register the client with `IServiceCollection` and resolve it from the container. The `HttpClient` is managed by `IHttpClientFactory`. Configure the client's behavior through `OmbrevixBillingClientOptions`.

```csharp
services.AddOmbrevixBillingClient(options =>
{
    options.BasicAuth = new BasicAuthCredentials { Username = "YOUR_USERNAME", Password = "YOUR_PASSWORD" };
    options.BearerAuth = "YOUR_BEARER_TOKEN";
    options.Environment = ServerEnvironment.Us;
    // TODO: configure more client options here
});
```

### Direct Instantiation [#direct-instantiation]

Create the client by passing an `HttpClient` you manage yourself. Configure the client's behavior through `OmbrevixBillingClientOptions`.

```csharp
var httpClient = new HttpClient();
// TODO: configure more client options here
var options = new OmbrevixBillingClientOptions
{
    BasicAuth = new BasicAuthCredentials { Username = "YOUR_USERNAME", Password = "YOUR_PASSWORD" },
    BearerAuth = "YOUR_BEARER_TOKEN",
    Environment = ServerEnvironment.Us,
};
var client = new OmbrevixBillingClient(httpClient, options);
```