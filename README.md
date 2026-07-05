# AFI Protocol — Vulnerability Disclosure Summary
## Researcher: pk009900 | Disclosed: April 24 – May 1, 2026

Full public disclosure record:
https://paragraph.com/@pk009900/afi-protocol-i-disclosed-7-vulnerabilities-before-the-dollar480k-exploit-76-days-later-still-no-response

Private PoC repo (pk009900/afi-exploit-poc) was shared 
with AFI engineer Sm4rty-1 on April 26, 2026.
All findings were formally dismissed in writing on May 1, 2026.
Protocol was exploited for $480,000 on May 31, 2026.

---

## Finding 1 — HIGH: Yield Sandwich Attack
File: test/AfiYieldSandwich.t.sol
Command: forge test --match-test testAsyncYieldSandwich -vvv

The requestRedeem function locks asset payout at 
request-time price not execution price. Allows an 
attacker to front-run yield distribution and capture 
disproportionate yield with zero price risk.
Disclosed: April 24, 2026

---

## Finding 2 — HIGH: totalAssets() Underflow DoS
File: test/AfiDoS_LogicTest.t.sol
Command: forge test --match-test test_DoS_Underflow_Logic -vvv

If a loss event causes virtualAssets to drop below 
unvestedAmount, totalAssets() permanently reverts. 
Vault is irreversibly bricked.
Disclosed: April 27, 2026

---

## Finding 3 — CRITICAL: Double-Counting Fees
File: test/FeeDoubleCount.t.sol
Command: forge test --match-path test/FeeDoubleCount.t.sol -vv

Yield.distributeYield hardcodes updateAsset: true when 
minting fee shares, creating unbacked phantom assets and 
guaranteeing protocol insolvency if fee is nonzero.
Disclosed: May 1, 2026

---

## Finding 4 — HIGH: 100% Redemption Fee Burn
File: test/FeeBurnExploit.t.sol
Command: forge test --match-path test/FeeBurnExploit.t.sol -vv

requestRedeem burns 100% of input shares including the 
fee portion, destroying protocol revenue instead of 
routing to treasury.
Disclosed: May 1, 2026

---

## Finding 5 — HIGH: Async Whale Exit Underflow
File: test/EscalationTests.t.sol
Command: forge test --match-test test_WhaleExitUnderflow -vv

Large redemption combined with a market loss permanently 
compromises vault math without requiring extreme conditions.
Disclosed: May 1, 2026

---

## Finding 6 — MEDIUM: Sybil maxRedeemCap Bypass
File: test/SybilAttack.t.sol
Command: forge test --match-path test/SybilAttack.t.sol -vv

Per-address withdrawal cap bypassed by splitting balance 
across multiple wallets, enabling coordinated bank runs.
Disclosed: May 1, 2026

---

## Finding 7 — MEDIUM: EIP-4626 Composability Violation
File: test/EscalationTests.t.sol
Command: forge test --match-test test_EIP4626ComposabilityViolation -vv

Strict equality check in withdraw() breaks compatibility 
with standard yield aggregators including Yearn and Morpho.
Disclosed: May 1, 2026

---

## Additional Architectural Finding — CRITICAL
## Operator Can Drain All Funds via Unrestricted 
## execute() Call Data in Manager.sol

Manager.execute() allows OPERATOR_ROLE to call any 
whitelisted address with arbitrary calldata. A compromised 
OPERATOR key can drain all protocol positions in a single 
transaction. This finding was documented in the private 
PoC repo shared with AFI on April 26, 2026.

NOTE: AFI's security incident update (June 12, 2026) 
confirmed the exploit transaction:
0x0851d24849587f2f2747f5261a0b97c096cc38a2e546bddef33bce619820b495

Attacker wallet (confirmed):
0x64c94CB6aA1ba9257Aad00Ff5DD5ec836533B0f5

---

## Verification

These tests run against a mainnet fork of the deployed
afiToken.sol contracts. To verify independently:

1. Clone the afiUSD repo:
   github.com/Artificial-Financial-Intelligence/afiUSD

2. Run any test command above against deployed contracts
   at addresses in test/forkTest.t.sol

3. For full PoC implementation access:
   kumarpiyush01092002@gmail.com

---

## Disclosure Timeline

Apr 24: Yield sandwich disclosed
Apr 26: AFI engineer Sm4rty-1 given repo access
Apr 27: totalAssets() DoS disclosed
May 1:  5 more findings disclosed — ALL dismissed in writing
May 31: Protocol exploited — $480,000 drained
Jun 7:  Researcher independently identifies attacker wallet
Jun 12: AFI publicly confirms same wallet (5 days later)
Jun 20: Formal bounty claim submitted
Jul 5:  Public disclosure published after 76 days no response
