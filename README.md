#### Overview

This example contains a src folder and the `Cargo.toml` file.

The src folder contains several Rust smart contracts, which are then imported into **contract.rs.**

The functionality present in **contract.rs** is derived from other contracts, creating a system where certain features can be inherited.

The structure of these contracts draws inspiration from the existing contract structure of fungible tokens in Soroban, as there is no established standard to follow for NFTs.

To adapt this for NFTs, we have made some modifications and introduced functions to enhance each contract's capabilities.

**admin.rs**
- has_administrator(env: &Env) -> bool: Checks if the system has an administrator by
querying the storage using a specific key.
- read_administrator(env: &Env) -> Identifier: Reads and returns the administrator's
identifier from the system's storage.
- write_administrator(env: &Env, id: Identifier): Writes the provided administrator's
identifier to the system's storage.
- check_admin(env: &Env, auth: &Signature): Verifies if a given authentication
signature corresponds to the system's administrator by comparing the identifier from
the signature with the one stored in the system's storage.
- In summary, these functions enable checking for the existence of an administrator,
managing the administrator's identifier in the system's storage, and ensuring that specific
actions are authorized by the system's administrator.

**approval.rs**
- read_approval(env: &Env, id: i128) -> Identifier: Reads and returns the approval
identifier for a specific action with the given id. If the approval doesn't exist, it returns
a default "zero address" identifier.
- read_approval_all(env: &Env, owner: Identifier, operator: Identifier) -> bool: Checks if
a specific operator has approval from a specific owner for all actions. It returns true if
the approval exists, indicating the operator has approval; otherwise, it returns false.
- write_approval(env: &Env, id: i128, operator: Identifier): Writes the approval identifier
for a specific action with the given id and operator to the system's storage.
- write_approval_all(env: &Env, owner: Identifier, operator: Identifier, approved: bool):
Writes the approval status (approved or not) for all actions from a specific owner to a
specific operator in the system's storage.

**balance.rs**
- read_balance(env: &Env, owner: Identifier) -> i128: Reads and returns the balance of
a specific owner in the system. If the balance doesn't exist, it returns 0.
- write_balance(env: &Env, owner: Identifier, write_type: WriteType): Writes the
balance of a specific owner in the system based on the provided write_type (add or
remove).
- read_supply(env: &Env) -> i128: Reads and returns the total supply in the system. If
the supply doesn't exist, it returns 0.
- increment_supply(env: &Env): Increments the total supply in the system by 1.
- read_minted(env: &Env, owner: Identifier) -> bool: Reads and returns whether the
specific owner has been minted. If not minted, it returns false.
- write_minted(env: &Env, owner: Identifier): Sets the minted status for the specific
owner to true.
- check_minted(env: &Env, owner: Identifier): Checks if the specific owner has already
been minted and panics with an error message if it has.

**contract.rs**
- This is the main function which uses imported functionalities of other contracts and it holds the all basic functions of nft. From minting to transfer(xfer) of the nft contract.rs is responsible. You will find all basic functions like ERC721 standard.

**event.rs**
- transfer(e: &Env, from: Identifier, to: Identifier, id: i128): Publishes a transfer event
with the sender (from) and receiver (to) information along with the id of the transfer.
- set_admin(e: &Env, admin: Identifier, new_admin: Identifier): Publishes a set_admin
event with the current administrator (admin) information, notifying that a new
administrator (new_admin) has been set.
- mint(e: &Env, to: Identifier, id: i128): Publishes a mint event with the receiver (to)
information and the id of the mint operation.
- burn(e: &Env, from: Identifier, id: i128): Publishes a burn event with the owner (from)
information and the id of the burn operation.
- approve(e: &Env, operator: Identifier, id: i128): Publishes an approval event with the
approved operator (operator) information and the id of the approval.
- approve_all(e: &Env, operator: Identifier, owner: Identifier): Publishes an approval
event for all actions with the approved operator (operator) and the owner's (owner)
information.
- In summary, these functions allow the system to notify and record events related to
transfers, setting administrators, minting, burning, and approvals.

**interface.rs**
- This file has been well explained over the code itself.

**lib.rs**
- Modular implementation for a Non-Fungible Token (NFT) system designed for
environments.

**metadata.rs**
- read_name(env: &Env) -> Bytes: Reads and returns the token name from the
system's storage.
write_name(env: &Env, name: Bytes): Writes the provided token name to the
system's storage.
- read_symbol(env: &Env) -> Bytes: Reads and returns the token symbol from the
system's storage.
- write_symbol(env: &Env, symbol: Bytes): Writes the provided token symbol to the
system's storage.
- read_token_uri(env: &Env, id: i128) -> Bytes: Reads and returns the URI (Uniform
Resource Identifier) associated with the token ID from the system's storage.
- write_token_uri(env: &Env, id: i128, uri: Bytes): Writes the provided URI for the given
token ID to the system's storage.

**owner.rs**
- zero_address(env: &Env) -> Identifier: Returns a default Identifier representing a
zero address. This function is used as a placeholder for non-existent or
unspecified owners.
- read_owner(env: &Env, id: i128) -> Identifier: Reads and returns the owner's
identifier for a specific token id from the system's storage. If the owner doesn't
exist, it returns the default zero address.
- write_owner(env: &Env, id: i128, owner: Identifier): Writes the owner's identifier
for a specific token id to the system's storage.
- check_owner(env: &Env, auth: &Identifier, id: i128): Checks if a given auth
identifier matches the owner's identifier for a specific token id. It panics with an
error message if the auth identifier doesn't match the owner's identifier.

**storage_types.rs**
- This code defines contract types (ApprovalAll, ApprovalKey, and DataKey) used for
managing approvals and data keys in the system. Here's a summary of each contract
type:
- ApprovalAll: Represents a combination of an operator and an owner identifier for
approvals.
- ApprovalKey: An enum representing different types of approval keys: either
All(ApprovalAll) or ID(i128).
- DataKey: An enum representing different types of data keys used in the system,
such as Balance, Nonce, Minted, Admin, Name, Symbol, URI, Approval, and
Owner.

Each contract type is derived with the `Clone` trait and annotated with` #[contracttype]`.

These contract types provide a structured way to manage approvals and access data
keys within the system.

### Steps to compile the contract

1. Setup soroban-cli following the steps mentioned on this page - https://soroban.stellar.org/docs/getting-started/setup
2. If you are a Windows user that wants to leverage Linux please refer to this video as a guide - (https://www.youtube.com/watch?v=PQA5_7m3OjQ&t=2s)
3. After the setup and installation of soroban-cli, run the command
4. `soroban contract build`
5. After the code compilation, you will see a newly created folder named **‘target’** which contains the wasm executable.
