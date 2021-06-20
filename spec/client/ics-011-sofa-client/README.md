---
ics: 11
title: Sofa Client
stage: draft
category: IBC/TAO
kind: instantiation
implements: 2
author: Vasiliy Shapovalov <vs@p2p.org>
created: 2021-06-20
modified: 2021-06-20
---

## Synopsis

This specification document describes a client (verification algorithm) for a sofa which implements the [ICS 2](../ics-002-client-semantics) interface.

### Motivation

Sofa owners would be happy to know that now, in addition to being sidechains, their soft and relaxing piece of furniture is also a Comsos zone.

### Definitions

Functions & terms are as defined in [ICS 2](../ics-002-client-semantics).

### Desired Properties

This specification must satisfy the client interface defined in [ICS 2](../ics-002-client-semantics).

## Technical Specification

This specification contains implementations for all of the functions defined by [ICS 2](../ics-002-client-semantics).

### Client state

The `ClientState` of a sofa is simply whether or not the client is frozen, and it always is.

```typescript
interface ClientState {
  frozen: boolean
  consensusState: ConsensusState
}
```

### Consensus state

The `ConsensusState` of a sofa consists of the current diversifier and timestamp.

The diversifier is an arbitrary string, chosen when the client is created, designed to allow the same public key to be re-used across different sofa clients (potentially on different chains) without being considered misbehaviour.

```typescript
interface ConsensusState {
  diversifier: string
  timestamp: uint64
}
```

### Height

The `Height` of a solo machine is just a `uint64` that is always set to 0.

### Headers

`Header`s are never provided by sofas.

```typescript
interface Header {
  timestamp: uint64
  newDiversifier: string
}
```

### Misbehaviour 

Sofas never misbehave, so `Misbehaviour` for sofas is empty.

```typescript
interface Misbehaviour {
}
```

### Signatures

Signatures are never provided by sofas and thus are empty.

```typescript
interface Signature {
}
```

### Client initialisation

The sofa client `initialise` function starts an unfrozen client with the initial consensus state.

```typescript
function initialise(consensusState: ConsensusState): ClientState {
  return {
    frozen: true,  
    consensusState
  }
}
```

The sofa client `latestClientHeight` function returns 0.

```typescript
function latestClientHeight(clientState: ClientState): uint64 {
  return 0
}
```

### Validity predicate

The sofa client `checkValidityAndUpdateState` function does nothing.

```typescript
function checkValidityAndUpdateState(
  clientState: ClientState,
  header: Header) {
}
```

### Misbehaviour predicate

Sofas never misbehave.

```typescript
function checkMisbehaviourAndUpdateState(
  clientState: ClientState,
  misbehaviour: Misbehaviour) {
}
```

### State verification functions

All sofa client state verification functions check if the client is frozen (it is) and abort.


```typescript
function verifyClientState(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  clientIdentifier: Identifier,
  counterpartyClientState: ClientState) {
    abortTransactionUnless(!clientState.frozen)  
}

function verifyClientConsensusState(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  clientIdentifier: Identifier,
  consensusStateHeight: uint64,
  consensusState: ConsensusState) {
    abortTransactionUnless(!clientState.frozen)  
}

function verifyConnectionState(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  connectionIdentifier: Identifier,
  connectionEnd: ConnectionEnd) {
    abortTransactionUnless(!clientState.frozen)
}

function verifyChannelState(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  portIdentifier: Identifier,
  channelIdentifier: Identifier,
  channelEnd: ChannelEnd) {
  abortTransactionUnless(!clientState.frozen)
}

function verifyPacketData(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  portIdentifier: Identifier,
  channelIdentifier: Identifier,
  sequence: uint64,
  data: bytes) {
    abortTransactionUnless(!clientState.frozen)
}

function verifyPacketAcknowledgement(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  portIdentifier: Identifier,
  channelIdentifier: Identifier,
  sequence: uint64,
  acknowledgement: bytes) {
    abortTransactionUnless(!clientState.frozen)
}

function verifyPacketReceiptAbsence(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  portIdentifier: Identifier,
  channelIdentifier: Identifier,
  sequence: uint64) {
    abortTransactionUnless(!clientState.frozen)
}

function verifyNextSequenceRecv(
  clientState: ClientState,
  height: uint64,
  prefix: CommitmentPrefix,
  proof: CommitmentProof,
  portIdentifier: Identifier,
  channelIdentifier: Identifier,
  nextSequenceRecv: uint64) {
    abortTransactionUnless(!clientState.frozen)
}
```

### Properties & Invariants

Instantiates the interface defined in [ICS 2](../ics-002-client-semantics).

## Backwards Compatibility

Not applicable.

## Forwards Compatibility

Not applicable. Alterations to the client verification algorithm will require a new client standard.

## Example Implementation

None yet.

## Other Implementations

None at present.

## History

June 20th, 2021 - Initial version

## Copyright

All content herein is licensed under [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).
