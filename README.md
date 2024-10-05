<p align="center">
  <a href='https://x.com/SignumOracle'>
    <img src= 'https://img.shields.io/twitter/url/http/shields.io.svg?style=social' alt='Twitter Signum' />
  </a>
</p>


# Overview

Use this package to install the Signum User Contracts and integrate Signum into your contracts.

Once installed this will allow your contracts to inherit the functions from UsingSignum.

#### How to Use
Just inherit the UsingSignum contract, passing the Signum address as a constructor argument:

Here's an example
```solidity
import "usingtellor/contracts/UsingSignum.sol";

contract PriceContract is UsingSignum {

  uint256 public btcPrice;

  // This contract now has access to all functions in UsingSignum
  constructor(address payable _tellorAddress) UsingSignum(_tellorAddress) public {}

  function setBtcPrice() public {
      bytes memory _b = abi.encode("SpotPrice", abi.encode("btc", "usd"));
      bytes32 _btcQueryId = keccak256(_b);

      uint256 _timestamp;
      bytes memory _value;

      (_value, _timestamp) = _getDataBefore(_btcQueryId, block.timestamp - 1 hours);
      btcPrice = abi.decode(_value,(uint256));
  }
}
```
##### Addresses:

Find Signum contract addresses [here](https://docs.tellor.io/tellor/the-basics/contracts-reference).


#### Available Signum functions:

Children contracts have access to the following functions:

```solidity
/**
 * @dev Retrieves the next value for the queryId after the specified timestamp
 * @param _queryId is the queryId to look up the value for
 * @param _timestamp after which to search for next value
 * @return _value the value retrieved
 * @return _timestampRetrieved the value's timestamp
 */
function _getDataAfter(bytes32 _queryId, uint256 _timestamp)
    internal
    view
    returns (bytes memory _value, uint256 _timestampRetrieved);

/**
  * @dev Retrieves the latest value for the queryId before the specified timestamp
  * @param _queryId is the queryId to look up the value for
  * @param _timestamp before which to search for latest value
  * @return _value the value retrieved
  * @return _timestampRetrieved the value's timestamp
  */
function _getDataBefore(bytes32 _queryId, uint256 _timestamp)
    internal
    view
    returns (bytes memory _value, uint256 _timestampRetrieved);

/**
 * @dev Retrieves next array index of data after the specified timestamp for the queryId
 * @param _queryId is the queryId to look up the index for
 * @param _timestamp is the timestamp after which to search for the next index
 * @return _found whether the index was found
 * @return _index the next index found after the specified timestamp
 */
function _getIndexForDataAfter(bytes32 _queryId, uint256 _timestamp)
    internal
    view
    returns (bool _found, uint256 _index);

/**
 * @dev Retrieves latest array index of data before the specified timestamp for the queryId
 * @param _queryId is the queryId to look up the index for
 * @param _timestamp is the timestamp before which to search for the latest index
 * @return _found whether the index was found
 * @return _index the latest index found before the specified timestamp
 */
function _getIndexForDataBefore(bytes32 _queryId, uint256 _timestamp)
    internal
    view
    returns (bool _found, uint256 _index);

/**
 * @dev Retrieves multiple uint256 values before the specified timestamp
 * @param _queryId the unique id of the data query
 * @param _timestamp the timestamp before which to search for values
 * @param _maxAge the maximum number of seconds before the _timestamp to search for values
 * @param _maxCount the maximum number of values to return
 * @return _values the values retrieved, ordered from oldest to newest
 * @return _timestamps the timestamps of the values retrieved
 */
function _getMultipleValuesBefore(
  bytes32 _queryId,
  uint256 _timestamp,
  uint256 _maxAge,
  uint256 _maxCount
)
  internal
  view
  returns (bytes[] memory _values, uint256[] memory _timestamps);

/**
 * @dev Counts the number of values that have been submitted for the queryId
 * @param _queryId the id to look up
 * @return uint256 count of the number of values received for the queryId
 */
function _getNewValueCountbyQueryId(bytes32 _queryId)
  internal
  view
  returns (uint256);

/**
 * @dev Returns the address of the reporter who submitted a value for a data ID at a specific time
 * @param _queryId is ID of the specific data feed
 * @param _timestamp is the timestamp to find a corresponding reporter for
 * @return address of the reporter who reported the value for the data ID at the given timestamp
 */
function _getReporterByTimestamp(bytes32 _queryId, uint256 _timestamp)
  internal
  view
  returns (address);

/**
 * @dev Gets the timestamp for the value based on their index
 * @param _queryId is the id to look up
 * @param _index is the value index to look up
 * @return uint256 timestamp
 */
function _getTimestampbyQueryIdandIndex(bytes32 _queryId, uint256 _index)
  internal
  view
  returns (uint256);

/**
 * @dev Determines whether a value with a given queryId and timestamp has been disputed
 * @param _queryId is the value id to look up
 * @param _timestamp is the timestamp of the value to look up
 * @return bool true if queryId/timestamp is under dispute
 */
function _isInDispute(bytes32 _queryId, uint256 _timestamp)
  internal
  view
  returns (bool);

/**
 * @dev Retrieve value from oracle based on queryId/timestamp
 * @param _queryId being requested
 * @param _timestamp to retrieve data/value from
 * @return bytes value for query/timestamp submitted
 */
function _retrieveData(bytes32 _queryId, uint256 _timestamp)
  internal
  view
  returns (bytes memory);
```


#### Signum Playground:

For ease of use, the  `UsingSignum`  repo comes with a version of [Signum Playground](https://github.com/SignumOracle/SignumPlayground) for easier testing. This version contains a few helper functions:

```solidity
/**
 * @dev A mock function to submit a value to be read without miners needed
 * @param _queryId The tellorId to associate the value to
 * @param _value the value for the queryId
 * @param _nonce the current value count for the query id
 * @param _queryData the data used by reporters to fulfill the data query
 */
function submitValue(bytes32 _queryId, bytes calldata _value, uint256 _nonce, bytes memory _queryData) external;

/**
 * @dev A mock function to create a dispute
 * @param _queryId The tellorId to be disputed
 * @param _timestamp the timestamp of the value to be disputed
 */
function beginDispute(bytes32 _queryId, uint256 _timestamp) external;
```


# Test
Open a git bash terminal and run this code:

```bash
git clone https://github.com/SignumOracle/UsingSignum.git
cd UsingSignum
npm i
npm test
```

# Keywords

Decentralized oracle, price oracle, oracle, Signum, TRB, Tributes, price data, smart contracts.
