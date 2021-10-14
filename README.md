# Algorand Android Payroll Smart Contract using the Java SDK

This solution will guide you in developing and deploying android application using the 
Algorand blockchain atomic transfer and smart contract that addresses the following use case:

* Account creation

* Funding accounts

* Create and compile the teal program

* Atomic transfer signed by the sender

* Atomic transfer signed by a smart contract

# Demo
![BACKIMAGE](https://user-images.githubusercontent.com/23031920/136575585-d728260a-2566-4bca-875d-81f03331e709.png)


# Requirements

* Android studio setup

* Familiarity with the Java and Kotlin programming language and its use within the Android Environment.

* Basic understanding of some blockchain terminologies.

* Basic understanding of teal and stateless smart contract

# Tools/ Libraries used
  Below are some of the important libraries used:
  - Algorand SDK
  - Coroutines
  - Glide
  - Databinding

# Setup Development Environment
To get started, your android studio should be up and running. To get the code on your android studio, simply click the clone button to clone the project or download the the project. Then from Android studio click on file and  select import to import the project from your local machine.

To successfully run this program, you need to generate/create four different accouts one for the contract owner and the remaining three for the employees. You can  create accounts using [myalgo](https://wallet.myalgo.com/access).

# App Installation Guide

  To install the app, here is the link to the [apk](https://github.com/gconnect/AlgorandPayrollContract/blob/master/app/app-debug.apk)
  
# File Structure
- `EmployeeAdapter` handles the recyclerview for the list of employees
- `constants` handles constant variables used in the MainActivity and DetailActivity
- `MainActivity` handles the main logic of the application
- `DetailActivity` handles the detail page for each employees
- `Employee` handles the data model
- `EmployeeDataSource` handles dummy data/list of empployees

## Teal Program/Smart Contract
```teal
 val tealSource = """  
               // Check the Fee is resonable, In this case 10,000 microalgos
                txn Fee
                int 10000
                <=
                //Check that the first group transaction is equal to 2000000
                gtxn 0 Amount
                int 2000000
                ==
                &&

                //Check that the second group transaction is equal to 1000000
                gtxn 1 Amount
                int 1000000
                == // same here, need to add an evaluation
                &&

                //Check that the third group transaction is equal to 2000000
                gtxn 2 Amount
                int 2000000
                ==
                &&

                //Check that the transaction amount is less than 5000000 or equal to 5000000
                txn Amount
                int 5000000
                <=
                &&
                //CloseRemainderTo should be the intended recipient or equal to global ZeroAddress.
                 // CloseRemainder can be used to reset sender account to 0.
                 // WARNING! all remaining funds in the sender
//                account will be sent to the closeRemainderTo Account, omit RECEIVER account when
//                in use otherwise all funds from the sender account will be sent to that account.
               
                txn CloseRemainderTo 
                global ZeroAddress
                ==
                &&
                //Always verify that the RekeyTo property of any transaction is set to the ZeroAddress
                //unless the contract is specifically involved ina rekeying operation.
                //Rekeying is a powerful protocol feature which enables an Algorand account holder to
                //maintain a static public address while dynamically rotating the authoritative private
                //spending key(s).
                //This check to prevent the transaction from been assigned to a new private key.
//                txn RekeyTo
//                global ZeroAddress
//                ==
//                &&
//                && 
            """.trimIndent()
  ```
# How the app works
  After installation..
  
  - It takes you to the `MainActivity`. The `MainActivity` contains a list of employees, two fabs and two buttons
  - The `green fab` enables you to fund the contract/senders account
  - The `purple fab` enables you to copy the contract/senders address
  - The explore button takes you to the algoexplorer page
  - The `pay employees` button calls the atomic transfer method to send algo to the employees either using the smart contract logic sig option or the sender option
   
# License
  Distributed under the MIT License. See  for more information. [LICENSE](https://github.com/gconnect/AlgorandPayrollContract/blob/master/LICENSE)
  
# Blog and Video Tutorial
For more details you can checkout the blog post [here](https://developer.algorand.org/solutions/building-an-android-payroll-dapp-using-algorand-smart-contract/) . And here is the link to the [youtube demo](https://www.youtube.com/watch?v=ps8Tvmq6zl8&t=2s)


# Disclaimer
 This project is not audited and should not be used in a production environment.
 


