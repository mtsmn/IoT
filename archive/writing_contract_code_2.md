---

copyright:
  years: 2016

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# OBSOLETE - SEE trade_lane_contract_blockchain.md - Writing Contract Code for {{site.data.keyword.iot_short}} {{site.data.keyword.blockchain}} integration
{: #writing_contract_code_2}

Last updated: 13 June 2016
{: .last-updated}

## The beta sample contract
{: #sample_beta_contract}

So far, we have seen a very simple contract that writes asset data for a single asset to the blockchain ledger. The beta sample contract that is discussed in this document expands on the basic contract that supports Create, Read, Update, Delete (CRUD) contract features by adding a number of additional tasks:
1. CRUD transactions of multiple assets at a time, with timestamps and data extends
2. Partial updates of asset data
3. Includes Rules, Compliance, and Alerts management for excessive temperature
4. Includes Asset State history and Recent states management
5. Custom logging capabilities

## Before you begin
{: #before_you_begin}

- Information about how to set up and configure an {{site.data.keyword.IBM_notm}} {{site.data.keyword.Bluemix_notm}} Hyperledger environment that is needed to start writing and testing smart contract chain code  is in the  *IoT Blockchain Beta Exploring smart contracts* document that are available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81).

- To follow along in the description of the beta contract that follows you can download the contract from the IBM Blockchain contracts GitHub repository at:  
 https://github.ibm.com/Watson-IoT/blockchain-starter/tree/master/chaincode/iot_blockchain_example_02

- Before we start with the definitions, there is the go generate prompt for schema and sample generation.
```
//go:generate go run scripts/generate_go_schema.go
```
The 'go' tool introduced the 'generate' command in version 1.4. Generate is an explicit preprocessing step in the golang toolchain for things like code generation. Before building the contract, do not forget to run go generate!  You can find more details [here](https://blog.golang.org/generate). Here we are generating the schema using [generate_go_schema.go] in the scripts folder. This is to generate a schema corresponding to the event and function signature of the contract. We will see how this is used in the 'query' section of the contract.

## The beta sample contract: Expanding on the simple contract
{: #expanding_on_simple_contract}

The first thing you will notice is that there are a lot more files in the contract code folder. This is because this contract performs a number of additional tasks. We will look closer at all of these features as we go through the contract.

Let's start with the main contract file [iot_blockchain_tradelane_beta_contract]

## Definitions
{: #definitions}

Lets see what is new in the definitions section. Because contract state and related aspects are handled in [contractState.go] we do not see a contract state definition or version constant like we did in the simple contract.

The section does contain a new struct for AssetID and additional fields in the AssetState struct for timestamp, Compliance, Alerts, Events and an 'Extension' section.
```
type SimpleChaincode struct {
}
type Geolocation struct {
	Latitude	float64 `json:"latitude"`
	Longitude	float64 `json:"longitude"`
}
type AssetID struct {
	AssetID        string		`json:"assetID,omitempty"`        // all assets must have an ID, primary key of contract
}

type AssetState struct {
	AssetID        string		`json:"assetID"`        // all assets must have an ID, primary key of contract
	timestamp      time.Time	`json:"timestamp"`      // all state changes have timestamps
	Location       Geolocation	`json:"location"`       // current asset location
	Temperature    float64		`json:"temperature"`    // asset temp
        InCompliance   bool             `json:"inCompliance"`   // contract-specific rules for compliance
	Alerts         AlertStatus	`json:"alerts"`         // struct of arrays for alerts status
        Event          string           `json:"event"`          // the event that made the state change
        Carrier        string           `json:"carrier"`        // the name of the carrier
        Extension      interface{}       `json:"extension"`      // can carry object or opaque JSON state
}
```

ArgsMap is introduced to hold unmarshaled input data. In the simple contract, we used a struct to unmarshal input. Here we are using a map. The reason is that when we use a struct, the values of the attributes that are not sent in are initialized by go to their default values by go. These values may be valid in the context of the use case. For example, a latitude of 0 is a valid number. This introduces the risk of data corruption. This also means that with structs, partial updates are not possible. If you send in, for example,  just the asset id and temperature in an update request, unless we carefully write code to handle it, the other attributes could get set to their default values when using a struct.  

Next we see an instance of NewContractLogger. There are limitations to logging from within the contract, in the current implementation of IBM Blockchain. [contractLogger] addresses this problem by giving us a very good logging implementation.
```
type ArgsMap map[string]interface{} // holds unmarshaled input
var log *ContractLogger= NewContractLogger(DEFAULTNICKNAME, DEFAULTLOGGINGLEVEL)
```
The usual main function provides the starting point for the shim.
```
err := shim.Start(new(SimpleChaincode))
```
## Deploy
### The init function
init is the only deploy function in this contract. It employs a 'Version' struct for the version number (3.0.4) and Nickname, which are the input parameters. Nickname is an optional parameter, whereas version number is mandatory. A nickname helps easy reference of the contract instance, especially while inspecting the log. The Default value of nickname is "TRADELANE" for this contract, as defined by [contractState], which we will visit later.  

We are familiar with the version checking from the simple contract. In addition we will see that the log module is being set with the nickname and an 'initializeContractState' call is made. initializeContractState, defined in [contractState], checks for the version in the stub. If the version sent in matches the version defined in the contract (in the const MYVERSION), it checks if the contract version in the stub is correct. If not, it gets set to the correct value.

The contractState struct, define in [contractState] has version, Nickname and ActiveAssets, which is a list of assets currently active in the shim. The initializeContractState function also checks if  ActiveAssets is empty and if so, initializes it.
```
(*log).SetModule(stateArg.Nickname)

err = initializeContractState(stub, stateArg.Version, stateArg.Nickname)
```
## Callbacks
{: #callbacks}

The 'Run' and 'Query' callbacks are familiar from the simple contract. Run, as you know, handles deploy and invoke function invocations.
Run handles a couple more functions here - deleteAllAssets and setCreateOnUpdate. Query deals with a number of additional functions - readRecentStates, readAssetHistory, readContractObjectModel and readContractState

## Deploy
{: #deploy}

Let's now look at the deploy functions
### createAsset function
createAsset takes 1 or 2 parameters. Asset data is referred to as event for the simple reason that in this Tradelane contract, asset data represents events with respect to the asset in question. event, programmatically is an interface, into which the input json gets Unmarshaled.
```
eventBytes := []byte(args[0])
    log.Debugf("createAsset arg: %s", args[0])

    err = json.Unmarshal(eventBytes, &event)
    if err != nil {
        log.Errorf("createAsset failed to unmarshal arg: %s", err)
		return nil, err
    }
```
If event is successfully retrieved, it gets cast as a map of string interface and assigned to argsMap, an instance of ArgsMap.A call is made to assetIsActive, again defined in [contractState]. This function retrieves the contract state from the ledger and determines if the current asset is in the list of Active Assets. If the asset is already present, it cannot be created, so an error is returned and a graceful exit executed. If the timestamp is not sent in, the current timestamp is set.
```
found := assetIsActive(stub, assetID)
...
...
 // test and set timestamp
    timeIn, found = argsMap["timestamp"].(time.Time)
    // adjust the timestamp if necessary
    if !found || timeIn.IsZero() {
        timeIn = txnTime
    }
     argsMap["timestamp"] = timeIn
```
Now let us look at Alerts. Alerts are an interesting concept that is being introduced in this contract, in [alerts]. The Tradelane contract tracks the progress of assets as it moves from source to destination. There will be rules that the transit has to comply to. For example, suppose the rule is that the temperature should never exceed 0 degrees. If this rule is violated, based on incoming data, that fact needs to be flagged. This is where alerts come in. Alerts can have three states - active, raised and cleared. If there are no active alerts, the contract is in compliance.

Alerts are defined in [alerts] and handles rules defined in [rules]. executeRules runs the check for all rules defined for the contract data. This contract has a simple rule  - a temperature check. If the temperature exceeds 0, an alert is raised. It is also ensured that the absence of a violation clears the latest status. According to the result of this check, if all alerts are clear, the alerts element is removed from argsMap and compliance is set to true.
```
    // run the rules and raise or clear alerts
    alerts := AlertStatus{}
    if argsMap.executeRules(&alerts) {
        // NOT compliant!
        argsMap["alerts"] = alerts
        argsMap["incompliance"] = false
    } else {
        if alerts.AllClear() {
            // all false, no need to appear
            delete(argsMap, "alerts")
        } else {
            argsMap["alerts"] = alerts
        }
        argsMap["incompliance"] = true
    }
```
The incoming state is now copied to the outgoing state. This contract accepts partial state updates, as would be the case in a real trade scenario. The contract, like in the simple contract, has two sections to its schema for events - common and custom. If the call to createAsset was through a redirect from updateAsset, that is also captured. The net result is marshaled and uploaded to the stub.
```
stateOut := argsMap

    // save the original event
    stateOut["lastEvent"] = make(map[string]interface{})
    stateOut["lastEvent"].(map[string]interface{})["function"] = "createAsset"
    stateOut["lastEvent"].(map[string]interface{})["args"] = args[0]
    if len(args) == 2 {
        // in-band protocol for redirect
        stateOut["lastEvent"].(map[string]interface{})["redirectedFromFunction"] = args[1]
    }

    // marshal to JSON and write
    stateJSON, err := json.Marshal(&stateOut)
...
 err = stub.PutState(assetID, []byte(stateJSON))
 ...
```
Now, we add the asset to the ContractState. This is again implemented in [contractState] and simply pulls down the current contract state from the ledger and adds this asset to the activeAssets that the contract is tracking.
```
...
 err = addAssetToContractState(stub, assetID)
 ....
 ```
 Next is a call to createStateHistory. AssetStateHistory, defined in [assethistory], keeps track of the processed, but jsonified data of assets. The createStateHistory function from [assethistory] creates a ledgerkey (by appending the STATEHISTORYKEY, a const string of value ".StateHistory" to the asset id ), and puts the asset history by ledgerkey, into the stub.
 ```
 ...
err = createStateHistory(stub, assetID, string(stateJSON))
...
```
Finally, we have the call to pushRecentState. This keeps track of the 20 most recent active assets that the contract has managed.
```
...
 err = pushRecentState(stub, string(stateJSON))
```
### updateAsset function
updateAsset has a number of similarities to createAsset. However, we have the flexibility to dictate the behavior of updateAsset in the eventuality that the asset is not found in ActiveAssets.
```
  found = assetIsActive(stub, assetID)
    if !found {
        // redirect to createAsset with same parameter list
        if canCreateOnUpdate(stub) {
            log.Noticef("updateAsset redirecting asset %s to createAsset", assetID)
            var newArgs = []string{args[0], "updateAsset"}
            return t.createAsset(stub, newArgs)
        } else {
            err = errors.New(fmt.Sprintf("updateAsset asset %s does not exist", assetID))
            log.Error(err)
            return nil, err
        }
    }
```
In the eventuality that the asset is not found in the ledger, it checks the 'canCreateOnUpdate' setting. If this is enabled, the createAsset function is called with the input data and the execution behavior follows a regular createAsset flow.

Once the asset is found in the ledger, and minimal cleansing - like timestamp setting, if needed, is done, and data retrieved from the ledger, a call to deepMerge, defined in [mapUtils] is made.
```
stateOut := deepMerge(argsMap, ledgerMap)
```
deepMerge takes two maps, one the source and the other the destination, merges source into destination and returns the destination. We use deepMerge here to merge the marshaled and refined assetMap into the ledgerMap. This allows for effective mamagement of partial updates. For example, if only a temperature update is sent in, this ensures that the temperature alone is updated, while retaining all other attributes of the asset as was maintained in the ledger previously.
Now we see a repeat of the Alerts management, recent state management and asset history management.

### deleteAsset function
deleteAsset essentialy implement the deletion of asset data from the stub, effectively undoing what we did in createAsset - removing the asset record from the stub and removing it from the contractState, stateHistory and RecentStates, if presents.

### deletePropertiesFromAsset function
This function provides us with the option to delete the properties that have currently been associated with the asset, the exception being the asset id and timestamp. You will also see a section on QualPropsToDelete in pyloadSchema.json under assetIDandPropertyArray. The event, deletePropertiesFromAsset is also captured in the state
```
ledgerMap["lastEvent"].(map[string]interface{})["function"] = "deletePropertiesFromAsset"
```
 We also see the Alert, Compliance, RecentState and StateHistory invocations here.

### deleteAllAssets
In deleteAllAssets, we parse through the ActiveAssets in the stub, remove them from Contract State, State History and Recent state.

### setLoggingLevel
[contractLogger] has logging levels defined from "CRITICAL" to "DEBUG". This function allows the appropriate logging level to be set.

## Query
{: #query}

Let's look at the query functions now.
### readAsset
readAsset is quite similar to the one in the simple contract - except that a map is used in place of  a struct. It checks the ActiveAssets for the assetID and if it is not found, throws an error. If successful, it returns the asset details from the ledger.
### readAllAssets
The readAllAssets function queries the ActiveAssets and retrieves the details of the assets in the ActiveAsset list from the ledger
### readAssetHistory
readAssetHistory, as the name implies, is a query function to return asset history. It expects as input the number of history instances and the asset id. To hold these, we have the arg struct, into which the input data gets marshaled
THe
```
var arg struct {
        Count int           `json:"count"`
        AssetID string      `json:"assetID"`
    }
    var result struct {
        States []string `json:"states"`
    }
```
Now we check the ActiveAssets for this asset id. Once it is confirmed as a valid asset, the readStateHistory call in [assethistory] to retrieve the AssetStateHistory.
```
stateHistory, err := readStateHistory(stub, arg.AssetID)
```
The function assigns 'return' above with all the history of the asset or the number requested, whichever is smaller and returns the same.

### readContractState
readContractState retrieves the Contract state details from the stub and returns the same

### readAssetSamples
The asset samples generated as part of the go generate call, from samples.go is returned. This is essentially an example of schema, an instance that helps user understand the json structure expected.

### readAssetSchemas
The assetSchemas, also generated as part of the go generate call, outlines the contract's schema in detail. This is invaluable when trying to build components that can consume the contract like a UI component.
