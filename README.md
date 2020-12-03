# blockchain-bridging-api

## Light Client API

```solidity
interface iLightClient {
    event BlockAdded(uint256 indexed number, bytes32 indexed hash);
    event FinalBlockChanged(uint256 indexed number, bytes32 indexed hash);

    function getMinBatchCount() view external returns (uint256 count);
    function getMaxBatchCount() view external returns (uint256 count);
    function getConfirmationDelay() view external returns (uint256 delay);
    function getLastConfirmed() view external returns (uint256 blockNumber);
    function getLastVerifiable() view external returns (uint256 blockNumber);

    function getBlockHash(uint256 number) view external returns (bytes32 hash);
    function getConfirmedBlockHash(uint256 number) view external returns (bytes32 hash);

    function tick() external;
    function updateLastVerifiableHeader(EthereumDecoder.BlockHeader memory header) external;
    function addBlocks(EthereumDecoder.BlockHeader[] memory headers) external;
}
```

## Prover API

```solidity
interface iProver {
    function lightClient() view external returns (address _lightClient);

    function verifyTrieProof(MPT.MerkleProof memory data) pure external returns (bool);

    function verifyHeader(
        EthereumDecoder.BlockHeader memory header
    ) view external returns (bool valid, string memory reason);

    function verifyTransaction(
        EthereumDecoder.BlockHeader memory header,
        MPT.MerkleProof memory txdata
    ) view external returns (bool valid, string memory reason);

    function verifyReceipt(
        EthereumDecoder.BlockHeader memory header,
        MPT.MerkleProof memory receiptdata
    ) view external returns (bool valid, string memory reason);

    function verifyAccount(
        EthereumDecoder.BlockHeader memory header,
        MPT.MerkleProof memory accountdata
    ) view external returns (bool valid, string memory reason);

    function verifyLog(
        MPT.MerkleProof memory receiptdata,
        bytes memory logdata,
        uint256 logIndex
    ) view external returns (bool valid, string memory reason);

    function verifyTransactionAndStatus(
        EthereumDecoder.BlockHeader memory header,
        MPT.MerkleProof memory receiptdata
    ) view external returns (bool valid, string memory reason);

    function verifyCode(
        EthereumDecoder.BlockHeader memory header,
        MPT.MerkleProof memory accountdata
    ) view external returns (bool valid, string memory reason);

    function verifyStorage(
        MPT.MerkleProof memory accountProof,
        MPT.MerkleProof memory storageProof
    ) view external returns (bool valid, string memory reason);
}
```

## Projects implementing this API

- https://github.com/loredanacirstea/goldengate
