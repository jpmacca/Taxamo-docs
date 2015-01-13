### Welcome

Welcome to the Taxamo documentation page. [Taxamo](http://www.taxamo.com/how-it-works) is a service that simplifies the complex world of EU VAT compliance, and eases the burden of staying compliant with EU tax law for the developers, and the owners, of e-commerce websites.

**If you are a merchant** and wish to use Taxamo in your online store, please review the following topics:

*   [Dashboard](https://dashboard.taxamo.com/merchant/app.html). Taxamo allows instant access to its Dashboard, with the ability to create and save your account later. This dashboard lets you preview what you will see once you have Taxamo integrated with your website, such as recent transactions, settlement stats and other useful information.
*   [Test mode](/docs/#d-test-mode). Taxamo's test mode enables you to get a better understanding of how the service works. We have provided a test mode (with test tokens) so you can see the service in action straight away with no changes to your production site (or a requirement to sign up for the production service).

Later on, depending on the software you use, you can either:

*   **Enhance your shopping cart page with VAT calculation** - by adding lines of HTML code to your shopping cart template.
*   **Process payments with Taxamo** - in this scenario, you can connect your merchant account from one of the popular Payment Service Providers (PSPs) to Taxamo and use Taxamo's payment forms for simple scenarios.
*   **Use 3rd party plugins** - if you are using more complex e-commerce software, our easy-to-use REST API can be seamlessly integrated with pretty much any e-commerce package available.

**If you are a developer** and you wish to integrate your e-commerce solutions with Taxamo, you should first read the following topics:

*   [Integration guide](/docs/integration-guide/#introduction) - technical guidelines dedicated to integrating e-commerce solutions with Taxamo.
*   [Test mode](/docs/#d-test-mode) - Test mode allows you to make all the API calls without actually settling for VAT. It also allows you to connect to popular payment solutions in test/sandbox mode.

### Sign up

Taxamo users can instantly access [Taxamo's dashboard](https://dashboard.taxamo.com/merchant/app.html). When ready, you can enter your email address and create a unique password for accessing your account.

### Test mode

Test mode is the default mode of operation for every newly-created account. Test mode allows you to familiarise yourself with the service in a 'sandbox' mode.

You can perform the same operations in test mode that you can in production mode, such as:

*   Login to the Taxamo dashboard
*   Use the [JavaScript API](/docs/taxamo.js/#javascript_api) or [REST API](/docs/rest-api/#usage) to:

    *   Calculate customer location and VAT on items
    *   Test transaction flow
    *   Browse reports and transactions for test data
    *   Integrate with PSPs in test/sandbox mode.

Transactions performed using test mode tokens are stored within Taxamo, but are not settled with tax authorities (nor are you charged in any way for producing them). Feel free to perform as many transactions as you want while in test mode in order to evaluate the Taxamo platform.

To enable production mode, you need to [register your business details](https://dashboard.taxamo.com/auth.html#/login), select your preferred package (based on transaction volume) and enter payment information.

Note: Even with production mode enabled, you still have access to two sets of access tokens: test mode and production mode. Depending on the tokens that you use, the appropriate mode will be applied.

### Transactions

Taxamo is built around the sales transactions that happen on a merchants e-commerce website. The 'transaction' represents the customer order and contains the following data:

*   Customer data - declared VAT number, or billing address.
*   Payment data - currency and amount of the processed transaction.
*   Order data - items ordered, type, quantity and cost.
*   VAT location evidence - IP address, credit card prefix, billing address or declared country of residence.
*   VAT data - VAT rates applied and total VAT amount(s).
*   Optionally, invoice data such as the date and billing address.
*   Optionally, additional data such as the customer's email address or other customer information.

The transaction flow is as follows:

1.  **Create transaction:** This can either happen from a user's browser, using our [JavaScript API](/docs/taxamo.js/#javascript_api) or can be performed directly via a [REST API](/docs/rest-api/#usage) call.
2.  **Confirm transaction:** This needs to be performed directly with a [REST API](/docs/rest-api/#usage) call. There are some simple scenarios (e.g. Stripe integration) where transactions can be confirmed automatically.
3.  **Settle transactions:** At the end of every quarter confirmed transactions for that period are aggregated as a [Settlement report](/docs/#d-settlement).

In addition, unconfirmed transactions can be cancelled and updated with the appropriate [REST API](/docs/rest-api/#usage) call. Confirmed transactions can be refunded if necessary.

### MOSS Registration

MOSS stands for the Mini One Stop Shop. EU regulations governing the MOSS system come into force on January 1, 2015. This system allows companies supplying cross-border digital services (such as telecommunication services, television and radio broadcasting and electronic services) to non-taxable persons in the EU to account for VAT on their EU sales via a web portal. 

This supply is deemed to be a business-to-consumer (B2C) transaction, and it is these transactions that come within the scope of the 2015 EU VAT rules.

Additional information on the MOSS system can be found on the [European Commission](http://ec.europa.eu/commission_2010-2014/semeta/headlines/news/2012/01/20120113_en.htm) website.

The MOSS system allows companies to avoid filing individual registrations in every EU Member State where they have B2C sales. Learn more on Taxamo's blog .

The MOSS system will be open for registrations from October 2014. Taxamo's reporting suite provides downloadable SAF-MOSS reports showing the merchant's VAT liability for each EU Member State, which can be uploaded by the merchant to their local tax authority for filing.

### Settlement

As a registered company under the MOSS system, merchants are required by EU law to submit VAT returns every quarter. As part of the Taxamo service, a quarterly EU MOSS VAT return is generated, which can be used by the merchant to file their return.

It is important to note that, when a company is already established in an EU Member State, its B2C digital sales in that country are to be returned via its normal domestic VAT returns, and not via the MOSS system.

Late returns or failure to settle on time may result in penalties and charges for late submission. Each EU Member State has its own rules and procedures for late returns. Remember, failure to settle is putting your company at risk of an audit by the tax authorities.

### Integration Guide

This section provides basic guidelines and technical information for developers who want to connect their e-commerce software to Taxamo.

Please note that the features provided by Taxamo are broader than those described below. Please visit our more comprehensive documentation section within the [Dashboard](https://dashboard.taxamo.com/merchant/app.html) for additional detail.

### 1. Connecting to Taxamo

The Taxamo service is accessible through two channels:

*   Direct communication using the [REST API](/docs/rest-api/#usage).
*   Via the JavaScript API provided by [taxamo.js](/docs/taxamo.js/#javascript_api).

Every request issued needs to contain either a public token (for operations executed directly within a customer's browser) or a private token (for operations that are securely invoked server-side).

### 2. Updating prices

The most common way to update prices on a website using Taxamo is to use [taxamo.js](/docs/taxamo.js/#javascript_api) to [detect customer's country of residence](/docs/taxamo.js/#country_detection). For customers who do not wish to use taxamo.js, it is possible to simply invoke the appropriate [REST API endpoint](/docs/rest-api/#usage), providing the relevant parameters that allow Taxamo to detect the user's country in a way that is compliant with the 2015 EU VAT rules.

Both the VAT rate look-up and the customer location detection requests can be run either with the public or private token.

### 3. Storing transactions

For a transaction to be recorded, it needs to be stored in Taxamo. This can be done using the [taxamo.js Taxamo.storeTransaction()](/docs/taxamo.js/#storing_transaction) function or, alternatively, by directly invoking the [POST /api/v1/transactions](https://dashboard.taxamo.com/apidocs/api/v1/transactions/docs.html!POST) REST API endpoint.

### 4. Confirming transactions

Once the order has been completed/processed on your website (i.e. once the customer's payment method has been debited), it needs to be confirmed within Taxamo. This must be performed using the private token to invoke the [POST /api/v1/transactions/:key/confirm](https://dashboard.taxamo.com/apidocs/api/v1/transactions/docs.html!POSTkeyconfirm) REST API endpoint.

You can also use appropriate REST API endpoints to cancel and modify existing transactions.

### API documentation

Comprehensive REST APIful documentation is available on [Taxamo's Dashboard](https://dashboard.taxamo.com/apidocs/docs.html).

Taxamo.js guide is available at [/docs/taxamo.js/#javascript_api](/docs/taxamo.js/#javascript_api)

Python integration guide (with example Stripe connectivity) is available at [/docs/integration-guide/#python_guide](/docs/integration-guide/#python_guide).
