# Data Collection

Data collection occures by requesting `events` from the Mina blockchain, which are fired from `SmartContract`.

## Smart Contract

Define _names_ and _types_ of your events:

```ts
events = {
  "arbitrary-event-key": Field,
};
```

In order to send data to the blockchain with use the following method:

```ts
this.emitEvent("arbitrary-event-key", data);
```

## Off-chain

The most convenient way to pull `events` off the blockchain is by [making graphql request](https://berkeley.graphql.minaexplorer.com/):

**Request**

```gql
query getEvents($zkAppAddress: String!) {
  zkapps(
    query: {
      zkappCommand: { accountUpdates: { body: { publicKey: $zkAppAddress } } }
      canonical: true
      failureReason_exists: false
    }
    sortBy: BLOCKHEIGHT_DESC
    limit: 1000
  ) {
    hash
    dateTime
    blockHeight
    zkappCommand {
      accountUpdates {
        body {
          events
          publicKey
        }
      }
    }
  }
}
```

The response depends on the state of the smart contract, but it will be something like this:

**Response**

```json
{
  "data": {
    "zkapps": [
      {
        "blockHeight": 17459,
        "dateTime": "2023-02-21T13:15:01Z",
        "hash": "CkpZ3ZXdPT9RqQZnmFNodB3HFPvVwz5VsTSkAcBANQjDZwp8iLtaU",
        "zkappCommand": {
          "accountUpdates": [
            {
              "body": {
                "events": ["1,0"],
                "publicKey": "B62qkzUATuPpDcqJ7W8pq381ihswvJ2HdFbE64GK2jP1xkqYUnmeuVA"
              }
            }
          ]
        }
      },
      {
        "blockHeight": 17458,
        "dateTime": "2023-02-21T13:09:01Z",
        "hash": "CkpaEP2EUvCdm7hT3cKe5S7CCusKWL2JgnJMg1KXqqmK5J8fVNYtp",
        "zkappCommand": {
          "accountUpdates": [
            {
              "body": {
                "events": [],
                "publicKey": "B62qkzUATuPpDcqJ7W8pq381ihswvJ2HdFbE64GK2jP1xkqYUnmeuVA"
              }
            }
          ]
        }
      },
      {
        "blockHeight": 17455,
        "dateTime": "2023-02-21T12:48:01Z",
        "hash": "CkpZePsTYryXnRNsBZyk12GMsdT8ZtDuzW5rdaBFKfJJ73mpJbeaT",
        "zkappCommand": {
          "accountUpdates": [
            {
              "body": {
                "events": ["13,12"],
                "publicKey": "B62qkzUATuPpDcqJ7W8pq381ihswvJ2HdFbE64GK2jP1xkqYUnmeuVA"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Events

It is possible to send up to **16 fields** in events in a single transaction, and each field can be up to **255 bits**.