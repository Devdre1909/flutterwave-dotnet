<p align="center">
    <img title="Flutterwave" height="200" src="https://flutterwave.com/images/logo-colored.svg" width="50%"/>
</p>

# .NET Library for Flutterwave (version 3) APIs
This library makes it easy to consume [Flutterwave API (v3)](https://developer.flutterwave.com/reference#introduction-1) in .Net projects.

## Introduction
This library implements the following services:
1. Banks
    * Get bank branches
    * Get banks
2. Miscellaneous
    * Verify a bank account number
3. Payments
    * Cancel a payment plan
    * Create a payment plan
    * Get a payment plan
    * Get payment plans
    * Initiate payment
    * Update a payment plan
4. Sub accounts
    * Create a sub account
    * Delete a sub account
    * Fetch a sub account
    * Fetch all sub accounts
    * Update a sub account
5. Transactions
    * Get transaction fees
    * Get all transactions
    * Resend transaction webhook
    * Verify a transaction
    
## Installation
* From Nuget <br/>
    ```c#
    Install-Package Flutterwave.Net -Version 1.0.0
    ```
* From .NET CLI
    ```c#
    dotnet add package Flutterwave.Net --version 1.0.0
    ```
* As a package reference
    ```c#
    <PackageReference Include="Flutterwave.Net" Version="1.0.0" />
    ```
    
## Configuration
1. Include the Flutterwave.Net namespace to expose all types
    ```c#
    ...
    using Flutterwave.Net;
    ...
    ```
2. Declare and initialise the [FlutterwaveAPI](src/flutterwave-dotnet/FlutterwaveApi.cs) class with your secret key
    ```c#
    string flutterwaveSecretKey = ConfigurationManager.AppSettings["FlutterwaveSecretKey"];
    var api = new FlutterwaveApi(flutterwaveSecretKey);
    ```

## Usage

### - Banks
1. Get bank branches
    ```c#
    int bankId = 280;

    GetBankBranchesResponse response = api.GetBankBranches(bankId);

    // success
    if (response.Status == "success")
    {
        // Get bank branches
        List<BankBranch> bankBranches = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
    
2. Get banks
    ```c#
    GetBanksResponse response = api.GetBanks(Country.Nigeria);

    // success
    if (response.Status == "success")
    {
        // Get all banks
        List<Bank> banks = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```

### - Miscellaneous
1. Verify a bank account number
    ```c#
    string accountNumber = "0690000032";
    string bankCode = "044";
    VerifyBankAccountResponse response = api.Miscellaneous.VerifyBankAccount(accountNumber, bankCode);

    // success
    if (response.Status == "success")
    {
        // Get the bank account
        BankAccount bankAccount = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```

### - Payments
1. Cancel a payment plan
    ```c#
    int paymentPlanId = 123;
    
    PaymentPlanResponse response = api.CancelPaymentPlan(paymentPlanId);

    // success
    if (response.Status == "success")
    {
        // Get payment plan
        PaymentPlan paymentPlan = response.Data;
        
        // Verify payment plan status
        bool isCancelled = paymentPlan.Status == "cancelled";
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
2. Create a payment plan
    ```c#
    decimal amount = 5000;
    string name = "Monthly Nepa Bill Collection";
    int duration = 24;
    
    PaymentPlanResponse response = api.CreatePaymentPlan(amount, 
                                                         name, 
                                                         Interval.Monthly, 
                                                         duration);

    // success
    if (response.Status == "success")
    {
        // Get payment plan
        PaymentPlan paymentPlan = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
3. Get a payment plan
    ```c#
    int paymentPlanId = 123;
    
    PaymentPlanResponse response = api.GetPaymentPlan(paymentPlanId);

    // success
    if (response.Status == "success")
    {
        // Get payment plan
        PaymentPlan paymentPlan = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
4. Get payment plans
    ```c#
    GetPaymentPlansResponse response = api.GetPaymentPlans();

    // success
    if (response.Status == "success")
    {
        // Get payment plans
        List<PaymentPlan> paymentPlans = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
5. Initiate payment
    ```c#
    string txRef = "hooli-tx-1920bbtytty";
    decimal amount = 100;
    string redirectUrl = "https://webhook.site/9d0b00ba-9a69-44fa-a43d-a82c33c36fdc";
    string customerEmail = "user@gmail.com";
    string customerPhonenumber = "08012345678";
    string customerName = "Yemi Desola";
    string paymentTitle = "Pied Piper Payments";
    string paymentDescription = "Middleout isn't free. Pay the price";
    string brandLogoUrl = "https://assets.piedpiper.com/logo.png";

    InitiatePaymentResponse response = api.Payments.InitiatePayment(txRef,
                                                                    amount,
                                                                    redirectUrl,
                                                                    customerName,
                                                                    customerEmail,
                                                                    customerPhonenumber,
                                                                    paymentTitle,
                                                                    paymentDescription,
                                                                    brandLogoUrl);

    // success
    if (response.Status == "success")
    {
        // Get payment hosted link 
        string hostedLink = response.Data.Link;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
6. Update a payment plan
    ```c#
    int paymentPlanId = 123;
    string name = "January neighbourhood";
    
    PaymentPlanResponse response = api.UpdatePaymentPlan(paymentPlanId,
                                                         name,
                                                         PaymentPlanStatus.Active);

    // success
    if (response.Status == "success")
    {
        // Get payment plan
        PaymentPlan paymentPlan = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```

### - Sub accounts
1. Create a sub account
    ```c#
    string bankCode = "044";
    string accountNumber = "0690000032";
    string businessName = "Eternal Blue";
    string businessEmail = "user@gmail.com";
    double splitValue = 0.5;
    string businessContact = "Yemi Desola";
    string businessContactMobile = "08012345678";
    string businessMobile = "08012345678";

    SubAccountResponse response = api.CreateSubAccount(bankCode,
                                                       accountNumber,
                                                       businessName,
                                                       businessEmail,
                                                       Country.Nigeria,
                                                       SplitType.Percentage,
                                                       splitValue,
                                                       businessContact,
                                                       businessContactMobile,
                                                       businessMobile);

    // success
    if (response.Status == "success")
    {
        // Get sub account
        SubAccount subAccount = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
2. Delete a sub account
    ```c#
    int subAccountId = 12345
    
    SubAccountResponse response = api.DeleteSubAccount(subAccountId);

    // success
    if (response.Status == "success")
    {
        // Get success message
        string successMessage = response.Message;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
3. Fetch a sub account
    ```c#
    int subAccountId = 12345;
    
    SubAccountResponse response = api.GetSubAccount(subAccountId);

    // success
    if (response.Status == "success")
    {
        // Get the sub account
        SubAccount subAccounts = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
4. Fetch all sub accounts
    ```c#
    GetSubAccountsResponse response = api.GetSubAccounts();

    // success
    if (response.Status == "success")
    {
        // Get all sub accounts
        List<SubAccount> subAccounts = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
5. Update a sub account
    ```c#
    int subAccountId = 12345;
    string businessName = "Eternal Blue";
    string businessEmail = "user@gmail.com";
    string bankCode = "044";
    string accountNumber = "0690000032";
    double splitValue = 0.5;

    SubAccountResponse response = api.UpdateSubAccountRequest(subAccountId,
                                                              businessName,
                                                              businessEmail,
                                                              bankCode,
                                                              accountNumber,
                                                              SplitType.Percentage,
                                                              splitValue);

    // success
    if (response.Status == "success")
    {
        // Get updated sub account
        SubAccount subAccount = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
    
### - Transactions
1. Get transaction fees
    ```c#
    decimal amount = 5000;
    
    GetTransactionFeeResponse response = api.Transactions.GetTransactionFee(amount);

    // success
    if (response.Status == "success")
    {
        // Get transaction fees
        TransactionFee transactionFees = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
2. Get all transactions
    ```c#
    GetTransactionsResponse response = api.Transactions.GetTransactions();

    // success
    if (response.Status == "success")
    {
        // Get all transactions
        List<Transaction> transactions = response.Data;
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```
3. Resend transaction webhook
    ```c#
    int transactionId = 1234567;
    
    ResendTransactionWebhookResponse response = api.Transactions.ResendTransactionWebhook(id);
    
    string responseMessage = response.Message;
    
    // success
    if (response.Status == "success")
    {
        // Provide Value
    }
    // error
    else
    {
        // Handle error
    }
    ```
4. Verify a transaction
    ```c#
    int transactionId = 1234567;
    
    VerifyTransactionResponse response = api.Transactions.VerifyTransaction(id);

    // success
    if (response.Status == "success")
    {
        // Get the transaction
        Transaction transaction = response.Data;
        
        // Verify transaction status
        bool isSuccess = transaction.Status == "successful";
        
        // Verify that the transaction reference, currency and charged amount i.e
        // transaction.TxRef, transaction.Currency and transaction.ChargedAmount
        // are what you expect them to be
        
    }
    // error
    else
    {
        // Get error message
        string errorMessage = response.Message;
    }
    ```

## Support
Create a new issue or add a comment to an open issue to request for new features and/or report bugs

[Send a mail](mailto:hello@egahi.so) for further assistance using this library
