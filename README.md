# A DeFi Project

A decentralized spot and perpetual exchange that supports low swap fees and zero price impact trades. It's next crypto revolution and it is Accelerates DeFi Adoption With Trusted Blockchain Protocol and Unique Token Economics.

## I. Overview Project

Blockchain: BNB Chain

Project's information: [Bscscan](https://www.bscscan.com/), [Coinmarketcap](https://coinmarketcap.com/), [Defillama](https://defillama.com/)

- Total supply: 8M
- Market cap
- 24h volume
- Price
Exchanges:

- CEXs: MEXC, Bitget, Gate.io
- DEXs: PancakeSwap

Audit: [Certik](https://skynet.certik.com/), [CyberScope](https://www.cyberscope.io/)
- Code Security: > 80%
- Community, Market
- Total score (80%) and Rank
## II. Roadmap

Phase 1:

- Webapp Developer
- Launch on BNB Chain Testnet
- BNB Chain ecosystem engagement
Phase 2:

- Private,Public Sale & Token Launch
- Listing on Top DEXs
- Launch Staking Program
- Certik Audit
Phase 3:
- Lauch Lotery Program
- AI Chatbot new version
- Farming and Restaking program

## III. Features

**Swap, Bridge, Chatbot AI**:

- Swap: exchange one cryptocurrency for another directly
- Bridge: transfer of assets between different blockchain networks
- Chatbot AI: interacts with users, providing assistance and information
**Staking program**:

Check the  `_stakingToken`, when it's correct, emit a   `Staking Contract` for user
```swift
function setStakingToken(address _stakingToken) external onlyOwner {
    require(
        stakingToken != _stakingToken,
        "Staking token already that address"
    );
    require(!isStart, "Cannot set Staking Token after pool start");
    require(
        _stakingToken != address(0),
        "Staking token cannot be the zero address"
    );
    require(
        _stakingToken.code.length > 0,
        "Only contract can set as Staking Token"
    );

    stakingToken = _stakingToken;

    emit StakingTokenUpdated(stakingToken);
}
```

**Claim contract**:

Set the `fee` is 5% for each time of claim for user and check the `block.timestamp`.
```swift
function getClaimedReward(address account) external view returns (uint256) {
    return _stakers[account].claimedReward;
}
```

```swift
function claimReward(
    address[] calldata account,
    uint256[] calldata amount
) external {
    require(
        msg.sender == address(backendWallet),
        "Only Backend wallet can claim reward for user"
    );

    require(
        account.length == amount.length,
        "Account and ammount length must be equal"
    );

    for (uint256 i = 0; i < account.length; i++) {
        address thisAccount = account[i];
        uint256 thisAmount = amount[i];

        uint256 fee = (thisAmount * 5) / 100;

        uint256 claimAmount = thisAmount - fee;

        IERC20 ERC20token = IERC20(stakingToken);
        ERC20token.safeTransfer(treasuryWallet, fee);
        ERC20token.safeTransfer(thisAccount, claimAmount);

        totalClaimedReward += thisAmount;

        _stakers[thisAccount].checkPoint = block.timestamp;
        _stakers[thisAccount].claimedReward += thisAmount;
    }

    emit RewardClaimed(account, amount);
}
```

## IV. API Endpoints

Health check: 
- GET: https://defi-app/api/healthcheck
- Response: 
    {
  "status": "text"
    }
Wallet information: 
- GET: https://defi-app/api/wallet/{wallet}
- Response: 
    {
  "wallet": "text",
  "depositAmount": 1,
  "availableClaim": 1,
  "level": 1,
  "isSpecial": true,
  "startDeposit": "2025-04-01T13:23:36.666Z",
  "endPeriod": "2025-04-01T13:23:36.666Z"
    }
Claim reward: 
- POST: https://defi-app/api/claimreward
- Body: 
    {
  "txhash": "text"
    }
- Response: 
    {
  "status": "Successfull"
    }

## V. App links and socials

Join the Project Community
- Twitter
- Telegram
- Discord 
