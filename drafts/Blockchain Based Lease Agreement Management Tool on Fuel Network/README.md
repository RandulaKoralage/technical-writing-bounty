# Blockchain Based Lease Agreement Management Tool on Fuel Network 

## Introduction
Lease Agreement Management is a crucial part of the real estate industry. It consists of handling every aspect of a lease agreement, from initial negotiation and tenant onboarding to rent collection, maintenance requests, and eventual lease renewal or termination. Just like in many other industries, auditability, immutability, and security are also important in the lease management industry.

In an era marked by rapid technological advancements and growing concerns over transparency, the emergence of blockchain technology has become a pathway for innovative solutions across various industries. Blockchain based Lease Agreement Management tools have introduced to address various weaknesses of traditional Lease Agreement Management approaches.

The main benefit of applying blockchain technologies is, it makes things more transparent  and significantly reduces the chances of fraud. Furthermore, the transactions are made quicker and benefits from trust and security. Since there are fewer middlemen and the process is more straightforward, transactions would also be less costly. 

When discussing blockchain based solutions, the solutions are directly impacted by the capabilities of the blockchain technologies used. In this article, we discuss how the powerful architectural features of Fuel Network, impact Blockchain-Based Lease Agreement Management Applications.

## Problems and Concerns that Lease Agreement Management Systems Deal With

First let’s summarize some of major problems and concerns that come out with Lease Agreement Management.

1.	Transparency
In Lease Agreement management systems, transparency is essential to creating trust, accountability, and smooth operations. Lack of transparency raises the risk of fraud, corruption, and illegal changes in land ownership. 


2.	Security
Ensuring the security of Lease Agreement Management application is important. While blockchain offers transparency, some lease data might be considered confidential (e.g. tenant personal information, financial details). Effective security measures should be taken to mitigate risks such as unauthorized access, data breaches, and tampering, thus upholding the integrity and confidentiality of lease-related information.

3.	Cost Factors
To start a transaction on blockchain, users need to establish a gas limit that is needed for the transaction to be successfully implemented. For a transaction-based application like a Lease Agreement Management System, maintaining low fee is important to keep user retention. Even with higher traffic, it will be important to manage a lower fee.

4.	Scalability
The scalability of a blockchain-based tool refers to its ability to handle an increasing amount of transactions or data without compromising performance or efficiency. In the context of Lease Agreement Management, scalability is essential as the volume of lease-related transactions and data can grow over time. Even in Ethereum blockchain, scalability is a problematic factor.

5.	Problem of Double Spending 
Double spending is a significant challenge in blockchain-based applications, including those used for Lease Agreement Management. It refers to the risk of spending the same cryptocurrency or digital asset more than once, which could lead to fraudulent transactions and undermine the integrity of the system. In the context of Lease Agreement Management, double spending refers to the possibility of a landlord fraudulently leasing the same property to two different tenants at the same time.

6.	Development Level Features
When it comes to a blockchain based solution, reliable and Faster Transaction Processing is very important. In order to provide such, the capabilities of undelaying development tools are also important. 

## How to Fuel Network's Capabilities are Applicable for Dealing with Problems and Concerns of Lease Agreement Management Systems 
1.	Transparency 
Transparency is inherited by default for fuel network as it uses a public ledger which records every transaction, and they are immutable. This creates a permanent and tamper-proof record of land ownership history. This is crucial in Lease Agreement Management as it prevents fraudulent changes to ownership records and ensures a clear audit trail for all stakeholders. 
Fuel uses UTXO fraud proofs. While transactions are generally assumed valid initially, anyone can later challenge a suspicious transaction. If someone attempts to manipulate records, others can step in and prove it with a fraud proof, upholding the integrity of the system.



2. Security
Fuel's unique feature is, its use of predicates. These are pre-defined conditions that must be met for a transaction to be finalized. In Lease Agreement Management, predicates could ensure proper approvals are in place before ownership changes are recorded. This adds an extra layer of security compared to traditional blockchains. 
Using the language features of sway, the smart contracts can generate errors and notifies the relevant Participants. Also can add transaction logs to trace useful intermediate non sensitive data.

3. Cost
Recall how Fuel separates tasks into a dedicated execution layer (FuelVM) and a separate layer for data storage. This modularity has a cost benefit. By offloading complex computations from the main blockchain to FuelVM, transaction fees associated with those computations are significantly lower even when high traffic.
4. Scalability
Unlike monolithic blockchains that handle everything at once, Fuel separates tasks. It has a dedicated execution layer (FuelVM) for processing transactions and a separate layer for data storage and security. This division allows FuelVM to handle many more transactions without slowing down the entire network.
Also the fraud proof indirectly causes the lower costs. It assumes transactions are valid by default but allows anyone to challenge them later if they're fraudulent. This eliminates the need for everyone to verify every transaction, reducing the load on the network.

5.	Problem of double spending 
Fuel relies on a system called the Unspent Transaction Output (UTXO) model. Imagine each coin on the network has a unique fingerprint. To spend a coin, you need to prove you own it by showing where it came. This makes it very hard to fake ownership or spend a coin you don't have.

6.	Development Level Features
Fuel network comes with sway language which targets FuelVM which also a powerful tool for development. Developers can apply the advantages of parallel transaction execution and fraud-proofing capabilities to Lease Agreement Management System.

---

## Sample Lease Agreement Management Application Based on Fuel
Following tutorial demonstrates how to apply capabilities and tools on fuel network to address these challenges.

With this tutorial, we will develop a full-stack DApp that has the following features which can demonstrate some fundamental functions of lease business.

1. Land Lords can initiate a lease
2. Tenants can view the status of the lease
3. Tenants can pay lease
4. Tenants can request for termination or renewal

### Technology Used

**Sway**: Rust based programming language and also the domain specific language for FuelVMs 

**Fuels**: A fully purpose-built, custom virtual machine for executing smart contracts   

**Fuel Typescript SDK** : SDK to interact with fuel contracts 

**Angular**: A popular typescript based frontend framework  
                                               
### Contract for Lease Management Application

#### Prerequisites

##### Tools Check List
1. You'll need to use a Linux Operating System or Windows Linux Subsystem (WLS) installed on your Windows Operating System in order to run fuels toolchain.
2. Make sure you have installed `fuelup`

```tsx
 fuelup --version
```
---

📝 **NOTE**

 How to install fuel toolchain [guide](https://docs.fuel.network/guides/installation/)

---

3. Confirm whether Node and Angular Installed on your development environment
```tsx
node --version
ng --version
```
---

📝 **NOTE**

 How to install [Node](https://angular.io/guide/setup-local) and [Angular](https://angular.io/guide/setup-local)

---

##### Initiate the project

1. Create a fresh folder inside your working directory
```tsx
 mkdir LeaseChain
```
2. Start the new project inside the above-created folder. I use name `contract` as the project name since I am going to write the contract inside it.
```tsx
cd LeaseChain
forc new contract
```
3. Confirm your project structure
```tsx
tree contract

├── Forc.toml
└── src
    └── main.sw
```

---

📝 **NOTE**

 `Forc.toml` is the manifest file and `main.sw` is where we going to write the contract. It autogenerates with `forc new` command with `.sw` extension stands for sway.

---

4. Open the code from VS code inorder to make the development easy.

Let's move to smart contract development.


##### Structuring the Contract

Let's try to structure our contract definition first.

The very first line of the file should mention the Sway Program Type. It can be either `contarct;` `library;` `predicate;` or `script;` 

On the top we will mention the program type as `contarct;` as we are going to write a contract to manage a lease agreement.

```tsx
contarct;
```

Then let's do all the imports of methods and types from The Sway standard library.

```tsx

use std::{
    asset::transfer,
    auth::msg_sender,
    call_frames::msg_asset_id,
    constants::BASE_ASSET_ID,
    context::{
        msg_amount,
        this_balance,
    },
    hash::Hash,
};
```


Thirdly we will declare the data validation errors. Here we will use Enumerations (Enums) which can use to declare multiple variations for a given type.
 
```tsx
enum InvalidError {
    IncorrectAssetId: AssetId,
    OnlyLandOwner: Identity,
    OnlyTenant: Identity,
    LeaseIsNotActive: u8, //1
    RequestPending: u8, // RR:2 TR:3
    NoDueAmount: u64,
}
```
Then let's define the Lease Agreement structure. With this, we will declare the object structure of a lease with its properties. To do this let's use `struct` keyword available in sway language.

```tsx
struct LeaseAgreement {
    id: u64,
    leaseAmount: u64,
    dueAmount: u64,
    leaseDuration: u32,
    secutrityDeposit: u64,
    leaseMaximumDue: u64,
    illegalActivity: bool,
    damagedToProperty: bool,
    leaseStartDate: u64,
    leaseRenewalDate: u64,
    landLoard: Identity,
    tenant: Identity,
    leaseStatus: str[1], // A T 
    requestStatus: str[2], //RR TR
    evictionWarning: bool,
}
```
---

📝 **NOTE**

 Checkout [here](https://fuellabs.github.io/sway/v0.19.2/basics/built_in_types.html) for more details about built-in data types

---

As my plan to store the lease objects inside a `StorageMap`, it needed to declare some persistent storage to store data. `StorageMap` is a collection type of sway language.

```tsx
storage {
    // counter for total leases listed
    lease_counter: u64 = 0,
    // map of lease IDs to leases
    lease_map: StorageMap<u64, LeaseAgreement> = StorageMap {},
}
```
Next, let's move to defining the ABI (Application Binary Interface)


##### Application Binary Interface (ABI)

Here we will define the function structure for the application. The concept of ABI is somewhat similar to defining interfaces in Java or JavaScript. Here we can declare the functions, their return types, parameter types, storage access, and playability. 

```tsx
abi LeaseHandler {
    //Land load can create a lease agreement
    #[storage(read, write)]
    fn add_lease(agreement: LeaseAgreement) -> u64;

    #[storage(read)]
    fn get_lease(lease_id: u64) -> LeaseAgreement;

    // return the number of leases listed
    #[storage(read)]
    fn get_count() -> u64;

    //Pay lease amount
    #[storage(read, write), payable]
    fn pay_leases(lease_id: u64);

    //Request renew contract request
    #[storage(read, write)]
    fn request_renew_lease(lease_id: u64) -> LeaseAgreement;

    //Request terminate contract request
    #[storage(read, write)]
    fn request_terminate_lease(lease_id: u64) -> LeaseAgreement;

    //Renew contract approve
    #[storage(read, write)]
    fn approve_renew_lease(lease_id: u64) -> LeaseAgreement;

    //Terminate contract approve
    #[storage(read, write)]
    fn approve_terminate_lease(lease_id: u64) -> LeaseAgreement;
}
```


####  Developing Contract


Now let's implement the contract functions based on the ABI written.

**1. Add Lease**

This function is to create and store a lease agreement. 

```tsx
 //Land load can create a lease agreement
    #[storage(read, write)]
    fn add_lease(agreement: LeaseAgreement) -> u64 {
        storage
            .lease_counter
            .write(storage.lease_counter.try_read().unwrap() + 1);

        // get the message sender
        let sender = msg_sender().unwrap();

        // configure the lease
        let durationInMonths = agreement.leaseDuration / 30;
        let new_lease: LeaseAgreement = LeaseAgreement {
            id: storage.lease_counter.try_read().unwrap(),
            leaseAmount: agreement.leaseAmount,
            dueAmount: agreement.leaseAmount * durationInMonths,
            leaseDuration: agreement.leaseDuration,
            secutrityDeposit: agreement.secutrityDeposit,
            leaseMaximumDue: 30,
            illegalActivity: false,
            damagedToProperty: false,
            leaseStartDate: agreement.leaseStartDate,
            leaseRenewalDate: agreement.leaseRenewalDate,
            landLoard: sender,
            tenant: agreement.tenant,
            leaseStatus: __to_str_array("A"), // A T 
            requestStatus: __to_str_array("RA"), //RR TR
            evictionWarning: false,
        };

        // save the new lease to storage using the counter value
        storage
            .lease_map
            .insert(storage.lease_counter.try_read().unwrap(), new_lease);
        return storage.lease_counter.try_read().unwrap();
    }

```
The function will receive a Lease Agreement structure as the parameter and based on that it'll create a new Lease Agreement and store it in the Storage Map.

Here you'll notice the following syntax,
```tsx
 __to_str_array("A")
```
As the data type for ` leaseStatus: str[1]` is an array, it'll need to convert the `string` value to an array using `__to_str_array` function.

**2. Get Lease**

```tsx
   #[storage(read)]
    fn get_lease(lease_id: u64) -> LeaseAgreement {
        return storage.lease_map.get(lease_id).try_read().unwrap();
    }
```
This function will read the Storage Map and return the lease object based on the id value.

**3. Pay Lease**
```tsx
    //Pay lease amount
    #[storage(read, write), payable]
    fn pay_leases(lease_id: u64) {
        let mut lease_agreement = storage.lease_map.get(lease_id).try_read().unwrap();
        let asset_id = msg_asset_id();

        let dueAmount = lease_agreement.dueAmount;
        let payment = msg_amount();
        require(
            from_str_array(lease_agreement.leaseStatus) != "T",
            InvalidError::LeaseIsNotActive(1),
        );
        require(dueAmount > 0, InvalidError::NoDueAmount(dueAmount));
        require(
            asset_id == BASE_ASSET_ID,
            InvalidError::IncorrectAssetId(asset_id),
        );
        let balance = lease_agreement.dueAmount - payment;
        lease_agreement.dueAmount = balance;
        storage.lease_map.insert(lease_id, lease_agreement);

        transfer(lease_agreement.landLoard, asset_id, payment);
    }
```
Tenant can pay the lease amount using this function.

You can see some conditions declared as below,
```tsx
require(dueAmount > 0, InvalidError::NoDueAmount(dueAmount));
```

```tsx
 require(
            asset_id == BASE_ASSET_ID,
            InvalidError::IncorrectAssetId(asset_id),
        );
```
The first parameter represent the condition and if it fails the right hand parameter is the output. As you see `InvalidError` is the `Enum` we declared at the beginning

**4. Request to Renew a Lease**

```tsx
    //Request renew contract request
    #[storage(read, write)]
    fn request_renew_lease(lease_id: u64) -> LeaseAgreement {
        let mut lease_agreement = storage.lease_map.get(lease_id).try_read().unwrap();
        let sender = msg_sender().unwrap();
        require(
            sender == lease_agreement
                .tenant,
            InvalidError::OnlyTenant(sender),
        );
        require(
            from_str_array(lease_agreement.leaseStatus) != "T",
            InvalidError::LeaseIsNotActive(1),
        );
        require(
            from_str_array(lease_agreement.requestStatus) != "RR",
            InvalidError::RequestPending(2),
        );
        lease_agreement.requestStatus = __to_str_array("RR");
        storage.lease_map.insert(lease_id, lease_agreement);
        return lease_agreement;
    }
```
This function will set the `requestStatus` function value to `RR`, which represents a `Renewal Request`

**5. Request to Terminate a Lease**

```tsx
  //Request terminate contract request
    #[storage(read, write)]
    fn request_terminate_lease(lease_id: u64) -> LeaseAgreement {
        let mut lease_agreement = storage.lease_map.get(lease_id).try_read().unwrap();
        let sender = msg_sender().unwrap();
        require(
            sender == lease_agreement
                .tenant,
            InvalidError::OnlyTenant(sender),
        );
        require(
            from_str_array(lease_agreement.leaseStatus) != "T",
            InvalidError::LeaseIsNotActive(1),
        );
        require(
            from_str_array(lease_agreement.requestStatus) != "TR",
            InvalidError::RequestPending(3),
        );
        lease_agreement.requestStatus = __to_str_array("TR");
        storage.lease_map.insert(lease_id, lease_agreement);
        return lease_agreement;
    }
```
As same as renewal function, this function sets `requestStatus` to `TR` status which implies Termination Requested.

With the next step, let's try to build and deploy the contract to testnet.


#### Deploying Contract

In this step, we will build and deploy our newly developed smart contract to fuel testnet.

1. Navigate to the `src` folder.

```sh
cd src
```

2. Format the code to get cleaner content.

```sh
forc fmt
```

3. Build the contract.

```sh
forc build
```

During the build, you may able to find the possible syntax and type errors of your code. If it is built successfully, it's more likely your smart contract will work fine.

You can find the output files inside `out` directory which located as the same level as `src` directory.

```sh
├── debug
    ├── contract-abi.json
    ├── contract.bin
    └── contract-storage_slots.json
```

4. Deploy the contract

```sh
forc deploy --testnet
```

It outputs your contract address and you can copy the address to use to access the contract from the frontend.

---

📝 **NOTE**

Pleasee refer [this](https://docs.fuel.network/guides/quickstart/building-a-smart-contract/#deploy-the-contract) guide on how to deploy a contract to fuel blockchain.

---
This will also store the deployment details as a JSON file inside `out/deployments` directory.

Now we are ready to integrate this contract with a frontend.

---

### Lease App Frontend and Contract Function Integration

In this tutorial, we are focusing on integrating the sway smart contract into an Angular frontend. Other than the core libraries of Angular, we will use fuel typescript SDK and wallet SDK for this task.

#### Initiating Frontend Project
The following steps are written assuming you have already installed Angular latest version on your machine. 

1. First you'll need to create a new folder inside your `LeaseChain` directory.

```sh
mkdir ui
```

2. Navigate to the `ui` folder and initiate a new angular project.
```sh
cd ui
ng new --no-standalone frontend
```
This will create a project with the standard file structure. You'll need to design a simple interface to interact with  each contract. Or else use the below repository containing the basic unintegrated UI for this tutorial.

---

💻  **GITHUB**

[Lease App Frontend](https://github.com/RandulaKoralage/LeaseChain_Frontend)

---

We will make some amendments to the above code during the integration phase.

Let's move to set up fuel-related libraries and configs with the angular project.


#### Installing Fuel Libraries

1. Navigate to project folder

```tsx
cd frontend
```
2. Install npm modules

```tsx
npm i @fuel-wallet/sdk fuels
```
Check your `package.json` and you should be able to find the following imports.

```sh
"@fuel-wallet/sdk": "0.15.2",
"fuels": "0.73.0",
```
##### Generating Types from ABIs

ABI JSON facilitates connecting to an already deployed contract. Inorder to generate a typescript form of Sway ABI JSON, follow the below steps.

1. Add fuels to the project

```sh
npm add fuels
``` 
2. Navigate to `src` directory

```sh
cd src
```

3. Create `abis` directory to copy the compiled ABIs from sway project.
```sh
mkdir abis
``` 
4. Create `contract` directory to save the typescript form of ABIs
```sh
mkdir contract
``` 
5. Copy and paste all files inside your sway project `out/debug` directory to `frontend/src/abis` directory.

```sh
tree abis
├── abis
    ├── contract-abi.json
    ├── contract.bin
    └── contract-storage_slots.json
```
6. Generate types for ABIs to `contract` directory that we created at step 4

```sh
npx fuels typegen -i ./abis/*-abi.json -o ./contract

```
This will generate following output
```sh
Generating files..
 - ./contract/ContractAbi.d.ts
 - ./contract/factories/ContractAbi__factory.ts
 - ./contract/ContractAbi.hex.ts
 - ./contract/index.ts
 - contract/common.d.ts
Done.⚡
```

You can simply import the above files to your typescript code to access the contract functions.

Now we are all set to start integration. 


#### Connecting the Fuel Wallet to App

App needs   to be connected to a fuel wallet in order to authorize the transactions. Under this title, we discuss how we are going to establish the wallet connection using fuel wallet SDK and fuels libraries.

Let's start with the Owner's module.

1. In `owner.component.ts` file, declare the following values for golbal scope,

```tsx
 CONTRACT_ID = "0xdxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

  fuel = new Fuel({
    connectors: [new FuelWalletConnector()],
  });

```

`CONTRACT_ID` : the deployed contract ID
`fuel` : the fuel wallet connector object

2. Add the following content to `connectWallet()` function.

```tsx
  async connectWallet() {
    await this.fuel.hasConnector();
    console.log("Connection state", await this.fuel.hasConnector());
    await this.fuel.connect();
    this.isConnected = await this.fuel.isConnected();
    console.log("Connection state", this.isConnected);

    const accounts = await this.fuel.accounts();
    console.log("Accounts", accounts);
    const currentAccount = await this.fuel.currentAccount();
    this.currentAcc = currentAccount ? currentAccount : ""
    console.log("Current Account", currentAccount);
    const wallet = currentAccount != null ? await this.fuel.getWallet(currentAccount) : null
  }
```

`fuel.hasConnector()` function checks whether any existing wallet connector available.
`fuel.connect()` will connect to the wallet and during execution, it'll open user's wallet installed as the extenstion.
`fuel.isConnected()` this will confirm whether the wallet is connected.
`fuel.accounts()` this function list down the available accounts for the wallet and `fuel.currentAccount()` function will pick the current account address used in the wallet.
`fuel.getWallet(currentAccount)` creates a wallet instance from the currentAcount we picked.

##### Connecting to the Contract

1. Declare a `ContractAbi` object in gloable scope.

```tsx
 contractO: ContractAbi | null = null;
```
2. Use `connectToContract` function to establish the connection with the contract. 
```tsx
    this.contractO = connectToContract(wallet, this.CONTRACT_ID)
    console.log(this.contractO)
```

Here I have set the contract instance to `contractO` so that I can use it anywhere over the file.


#### Accessing Contract Functions

Now we are going to create a new lease contract. 

**1. add_lease**

First we will extract the data captured by text fields and later will send it to `add_lease` function as a an Lease Agreeement object.

`dialog.component.ts`
```tsx
   async submit() {
    if (this.form && this.form.valid) {
      /*Preparing data inputs*/
      const tenant = this.form.value.tenant;
      this.tenant = tenant ? { Address: { value: new Address(tenant).toB256() }, } : { Address: { value: "", } };
      const leaseStartDateValue = this.form.get('leaseStartDate');
      const leaseRenewalDateValue = this.form.get('leaseRenewalDate');
      const leaseStartDateTimestamp = leaseStartDateValue ? leaseStartDateValue.value.getTime() : null;
      const leaseRenewalDateTimestamp = leaseRenewalDateValue ? leaseRenewalDateValue.value.getTime() : null;

      const date1 = new Date(leaseStartDateTimestamp);
      const date2 = new Date(leaseRenewalDateTimestamp);
      const differenceMs = Math.abs(date2.getTime() - date1.getTime());
      const differenceDays = Math.ceil(differenceMs / (1000 * 60 * 60 * 24));

      /*Setting up Agreement Object in a way that fits with LeaseAgreement struct of contract*/
      let agreement = {
        id: 1,
        leaseAmount: this.form.value.leaseAmount,
        dueAmount: 0.00,
        leaseDuration: differenceDays,
        secutrityDeposit: this.form.value.securityDeposit ? this.form.value.securityDeposit : parseInt(this.form.value.leaseAmount) * 0.10,
        leaseMaximumDue: this.form.value.leaseAmount * 0.80,
        illegalActivity: false,
        damagedToProperty: false,
        leaseStartDate: leaseStartDateTimestamp,
        leaseRenewalDate: leaseRenewalDateTimestamp,
        leaseStatus: "A", // A T 
        requestStatus: "RA",//RR TR
        evictionWarning: false,
        landLoard: tenant ? { Address: { value: new Address(this.data.landLoard).toB256() }, } : { Address: { value: "", } },
        tenant: this.tenant
      }

      /*Calling the contract function*/
      try {
        if (this.contractO != null) {
          let value = await this.contractO.functions
            .add_lease(agreement)
            .txParams({
              gasPrice: 1,
              gasLimit: 500_000,
            }).call()
          let formattedValue = new BN(value.value).toNumber();
          console.log(formattedValue)
          this.snackBar.open('Lease ID : ' + formattedValue, 'Close', {
            duration: 3000,
            panelClass: ['success-snackbar']
          })
        }
      } catch {
        this.snackBar.open('Error Occured', 'Close', {
          duration: 3000,
          panelClass: ['success-snackbar']
        })
      }
      this.dialogRef.close();
    }
  }
  ```

  In our smart contract, we expected tenant address to be in `Identity` type. To convert the `string` types tenant's address you can use following format.

  ```tsx
   { Address: { value: new Address(tenant).toB256() } } 
  ```

  value of `tenant` we input is something like `fuel1rs25xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

Also, the returned leaseId is taken and displayed to front user.

**2. get_lease**
  
  This function call to simply fetch the lease by it's id. Once the lease id is given, it'll fetch the lease details and display them in an readable manner.

`payment-dialog.component.ts`
  ```tsx
  async getLease(leaseId: number) {
    if (this.contractO != null) {
      let value = await this.contractO.functions
        .get_lease(leaseId)
        .txParams({
          gasPrice: 1,
          gasLimit: 500_000,
        }).call()

      this.summary = {
        damagedToProperty: value.value.damagedToProperty,
        dueAmount: new BN(value.value.dueAmount).toNumber(),
        evictionWarning: value.value.evictionWarning,
        id: new BN(value.value.id).toNumber(),
        illegalActivity: value.value.illegalActivity,
        leaseAmount: new BN(value.value.leaseAmount).toNumber(),
        leaseDuration: new BN(value.value.leaseDuration).toNumber(),
        leaseMaximumDue: new BN(value.value.leaseMaximumDue).toNumber(),
        leaseRenewalDate: new BN(value.value.leaseRenewalDate).toNumber(),
        landLoard: value.value.landLoard.Address?.value,
        leaseStartDate: new BN(value.value.leaseStartDate).toNumber(),
        leaseStatus: value.value.leaseStatus,
        requestStatus: value.value.requestStatus,
        secutrityDeposit: new BN(value.value.secutrityDeposit).toNumber(),
        tenant: value.value.tenant.Address?.value,
      }
      return value
    } else {
      return null
    }
  }
  ```
  
**3. request_renew_lease**

Once the lease data is displayed, the tenant is allowed to pay the lease, request to renew it or terminate the lease. The request is recorded in the contract.

`payment-dialog.component.ts`
```tsx
 async requestRenewal(): Promise<void> {
    console.log('Lease ID entered:', this.leaseId);
    try {
      if (this.contractO != null) {
        let value = await this.contractO.functions
          .request_renew_lease(this.leaseId)
          .txParams({
            gasPrice: 1,
            gasLimit: 500_000,
          }).call()

        console.log(value)
        console.log(value.value)
        this.snackBar.open('Renewal Requested. Status : ' + value.value.requestStatus, 'Close', {
          duration: 3000,
          panelClass: ['success-snackbar']
        });
      }
    } catch (_e) {
      console.log(_e)
      this.snackBar.open('Validation Error Occured. Lease May be already terminated or renewal pending', 'Close', {
        duration: 3000,
        panelClass: ['success-snackbar']
      });
    }

```
With a similar approach, you can call the `request_terminate_lease` contract function as well.

**4. pay_leases**

To implement `pay_leases` function, you'll need to pass the amount to transfer and the asset Id under,

```tsx
.callParams({
          forward: [this.data.leaseAmount, this.assetId],
        })
```

We only pass the lease id from frontend and the contract will pick the associated `landLoard` of the lease. Also contract will restrict paying unrelated leases to tenant.

`payment-dialog.component.ts`
```tsx
 async submit(): Promise<void> {
    console.log('Payment Amount:', this.data.leaseAmount);
    console.log('Sender:', this.data.landLoard);
    console.log('Lease Id ' + this.data.leaseId)
    if (this.contractO != null) {
      let value = await this.contractO.functions
        .pay_leases(this.data.leaseId)
        .txParams({
          variableOutputs: 2,
          gasPrice: 1,
          gasLimit: 500_000,
        })
        .callParams({
          forward: [this.data.leaseAmount, this.assetId],
        })
        .call()

      this.snackBar.open('Transaction Id : ' + value.transactionId, 'Close', {
        duration: 3000,
        panelClass: ['success-snackbar']
      });
    }
    this.dialogRef.close();
  }
```
The Readme contains only a few details on the integration, hence kindly refer to the frontend project available in GitHub repository.

#### Start the Angular Application

To deploy this project locally run

```bash
  ng s
```
If you are using the default angular post, you should be able to access the frontend from,

```tsx
https://localhost:4200/
```
---

## Conclusion
In conclusion, the adoption of blockchain technology in Lease Agreement Management offers numerous advantages, including increased transparency, reduced fraud risks, improved transaction efficiency, and enhanced security. However, several challenges such as scalability, security concerns, and regulatory frameworks need to be addressed for widespread implementation. The Fuel Network presents a promising solution to many of these challenges with its powerful architectural features. By leveraging public ledger technology and unique mechanisms like UTXO fraud proofs, modular execution and predicates, Fuel enhances transparency, security, and scalability in Lease Agreement Management Applications. Furthermore, its modular design and development tools provide developers with the flexibility and efficiency needed to build robust and secure Lease Agreement Management Systems. Overall, the Fuel Network demonstrates great potential in revolutionizing Lease Agreement Management practices and addressing the key concerns faced by traditional systems and tools based on Ethereum network. 

## References
[Sway Language and FuelVM](https://docs.fuel.network/docs/intro/what-is-fuel/)

[Scaling Ethereum with Fuel](https://fuel.mirror.xyz/Bbh8mlvJCY-kcfXmo2Urikv0qp3XCLBN3OkQ16sopr0)

[Modular Execution](https://fuel.mirror.xyz/elyKPXSlOMAl8pHlwx4ZLaz5Z6RK849gWypJLQaZMHg)
