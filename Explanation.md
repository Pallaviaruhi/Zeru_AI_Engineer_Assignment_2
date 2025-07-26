### Data Collection Method

Transaction data for the wallet addresses was collected using the Covalent API. This API gives reliable access to historical blockchain data. The script pulled all transactions for each wallet on the Ethereum mainnet and filtered them to show only those that interacted with the Aave V2 Lending Pool smart contract. This method was selected after finding out that the public API endpoints for The Graph's Aave V2 subgraph were no longer available.

### Feature Selection Rationale

To create a risk profile, several key features were derived from the raw transaction data. The most important features include:

Liquidation Count: This represents how many times a wallet has been liquidated. It is the most direct and severe indicator of high-risk behavior.

Borrow-to-Deposit Ratio: This ratio compares the total amount borrowed to the total amount supplied. A high ratio means a user is heavily leveraged and has less of a safety cushion.

Repayment Ratio: This shows the percentage of borrowed assets that have been paid back. A low ratio indicates outstanding debt, which poses a risk.

Wallet Age: This tracks the time since the wallet's first Aave transaction. Older wallets are usually seen as more stable and less speculative.

### Scoring Method

A rule-based model was created to assign a risk score, where a higher score means greater risk. Each wallet starts with a risk score of 0. Points are added for risky behaviors and subtracted for positive ones:

+ 400 points for each liquidation event.

+ points for a high borrow-to-deposit ratio.

+ points for a low repayment ratio (i.e., a high amount of unpaid debt).

- points for a longer wallet age.

The final raw score was scaled to a standard 0-1000 range using a MinMaxScaler.

### Justification of Risk Indicators

The scoring method places significant emphasis on liquidations since they are clear on-chain events that show a user's position became insolvent. This is a definite indicator of risk-taking. Secondary indicators, such as high leverage (borrow-to-deposit ratio) and unpaid debt (low repayment ratio), are early signs of potential future liquidations. Therefore, they are crucial parts of the risk score.