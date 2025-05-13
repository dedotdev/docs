---
description: 'RFC-0078: Merkleized Metadata Implementation'
---

# Merkleized Metadata

### Calculating Metadata Hash/Digest

```typescript
import { DedotClient, WsProvider } from 'dedot';
import { MerkleizedMetadata } from 'dedot/merkleized-metadata';

// Create a client
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new(provider);

// Get metadata from the client
const metadata = client.metadata;

// - Or refetch it directly via runtime apis
// const metadata = await client.call.metadata.metadataAtVersion(15);

// Define chain-specific information
const chainInfo = {
  // These can be omitted as they'll be fetched from metadata
  // specVersion: client.runtimeVersion.specVersion,
  // specName: client.runtimeVersion.specName,
  // ss58Prefix: 0, // Polkadot

  // These are required
  tokenSymbol: 'DOT'
  decimals: 10,
};

// Create a merkleizer instance
const merkleizer = new MerkleizedMetadata(metadata, chainInfo);

// Calculate metadata hash
const hash = merkleizer.digest();

console.log('Metadata Hash:', hash);
```

### Generating Proofs

#### Proof for extrinsic

```typescript
import { MerkleizedMetadata } from 'dedot/merkleized-metadata';

// Create a merkleizer instance
const merkleizer = new MerkleizedMetadata(metadata, chainInfo);

// Setup tx
const remarkTx = client.tx.system.remark('Hello World');
// await remarkTx.sign(alice, { tip: 1000_000n }); - sign with signer/keypair

// Generate proof for an extrinsic
const txHex = remarkTx.toHex(); // Hex-encoded extrinsic
const proof = merkleizer.proofForExtrinsic(txHex);

```

#### Proof for extrinsic payload

```typescript
import { ExtraSignedExtension } from 'dedot';
import { MerkleizedMetadata } from 'dedot/merkleized-metadata';
import { HexString } from 'dedot/utils';

// Initialize client ...

// Create a merkleizer instance
const merkleizer = new MerkleizedMetadata(client.metadata, chainInfo);

// Calculate raw tx payload
const extra = new ExtraSignedExtension(client, { signerAddress: 'PUT_ADDRESS' });
await extra.init();

const remarkTx = client.tx.system.remark('Hello World');

const txRawPayload = extra.toRawPayload(remarkTx.callHex).data as HexString;

// Generate proof for extrinsic payload
const proof = merkleizer.proofForExtrinsicPayload(txRawPayload);

```

#### Proof for extrinsic parts

This is equivalent to `proofForExtrinsicPayload`, except that each part of the transaction’s raw payload is calculated individually.

```typescript
import { ExtraSignedExtension } from 'dedot';
import { MerkleizedMetadata } from 'dedot/merkleized-metadata';
import { HexString } from 'dedot/utils';

// Initialize client ...

// Create a merkleizer instance
const merkleizer = new MerkleizedMetadata(client.metadata, chainInfo);

// Calculate raw tx payload
const extra = new ExtraSignedExtension(client, { signerAddress: 'PUT_ADDRESS' });
await extra.init();

const remarkTx = client.tx.system.remark('Hello World');

// Generate proof for extrinsic parts
const callData = remarkTx.callHex; // Hex-encoded call data
const includedInExtrinsic = extra.$Data.tryDecode(extra.data);
const includedInSignedData = extra.$AdditionalSigned.tryDecode(extra.additionalSigned);

const proof = merkleizer.proofForExtrinsicParts(
    callData, includedInExtrinsic, includedInSignedData
);

```

