# AFI Protocol — Vulnerability Disclosure Summary
## Researcher: pk009900 | April–May 2026

Full public disclosure:
https://paragraph.com/@pk009900/afi-protocol-i-disclosed-7-vulnerabilities-before-the-dollar480k-exploit-76-days-later-still-no-response

---

## Disclosed Findings

### 1. Yield Sandwich Attack — HIGH — April 24
File: test/AfiYieldSandwich.t.sol
Command: forge test --match-test testAsyncYieldSandwich -vvv

### 2. totalAssets() Underflow DoS — HIGH — April 27
File: test/AfiDoS_LogicTest.t.sol
Command: forge test --match-test test_DoS_Underflow_Logic -vvv

### 3. Double-Counting Fee Inflation — CRITICAL — May 1
File: test/EscalationTests.t.sol
Command: forge test --match-test test_DoubleCounting -vvv

### 4. 100% Redemption Fee Burn — HIGH — May 1
File: test/EscalationTests.t.sol
Command: forge test --match-test test_FeeBurn -vvv

### 5. Async Whale Exit Underflow — HIGH — May 1
File: test/EscalationTests.t.sol
Command: forge test --match-test test_WhaleExitUnderflow -vvv

### 6. Sybil maxRedeemCap Bypass — MEDIUM — May 1
File: test/EscalationTests.t.sol
Command: forge test --match-test test_SybilBypass -vvv

### 7. EIP-4626 Composability Violation — MEDIUM — May 1
File: test/EscalationTests.t.sol
Command: forge test --match-test test_EIP4626Violation -vvv

---

## Verification

These tests run against a mainnet fork of the deployed
afiToken.sol contract. Any researcher can verify by:

1. Cloning the afiUSD repo:
   github.com/Artificial-Financial-Intelligence/afiUSD

2. Running the test commands above against the 
   deployed contracts at the addresses listed in 
   test/forkTest.t.sol

3. For access to the full PoC implementation, 
   contact: kumarpiyush01092002@gmail.com

---

## Disclosure Timeline

April 24 — May 1: Private disclosure to AFI team
May 1: Written dismissal received
May 31: Protocol exploited for $480,000
June 20: Formal bounty claim submitted
July 5: Public disclosure published

Full evidence record on Paragraph (link above)
