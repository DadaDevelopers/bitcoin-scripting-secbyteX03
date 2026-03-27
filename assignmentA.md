# Bitcoin Scripting Assignment A submission

### Given script
`OP_DUP OP_HASH160 <PubKeyHash> OP_EQUALVERIFY OP_CHECKSIG`

### 1) Purpose of each opcode

`OP_DUP` copies the public key that the spender provides. This is needed because the script must use the same public key in two different checks: one check for the hash and one check for the signature.

`OP_HASH160` takes the copied public key and hashes it. The script does not compare the full public key directly. It compares the hash of the public key to the stored public-key hash.

`<PubKeyHash>` is the value that was locked into the output when the coin was created. It tells the script which public key is allowed to spend the coin.

`OP_EQUALVERIFY` checks whether the hashed public key matches the stored hash. If the two values are not the same, the script stops immediately and the spend fails.

`OP_CHECKSIG` checks whether the signature is valid for the transaction and the public key. If the signature is valid, the script can finish successfully. If it is not valid, the spend fails.

### 2) Data flow diagram

```text
Spender gives:
    [signature] [public key]

Script runs:
    OP_DUP
      ↓
    public key is copied
      ↓
    OP_HASH160
      ↓
    hash(public key) is created
      ↓
    compare with <PubKeyHash>
      ↓
    OP_EQUALVERIFY
      ↓
    if match, continue
      ↓
    OP_CHECKSIG
      ↓
    check signature with the public key
      ↓
    if signature is valid, the spend succeeds

````

### 3) What happens if signature verification fails

If OP_CHECKSIG does not get a valid signature, it leaves a false result on the stack, and the transaction does not pass script validation. In simple words, the coin cannot be spent. The network rejects the transaction.

### 4) Security benefit of hash verification
- Prevents public key substitution attacks
- Ensures only the correct public key can be used
- Adds an extra validation layer before signature verification
- Reduces attack surface by locking to hash instead of raw key
