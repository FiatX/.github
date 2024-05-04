# FiatX
FiatX is an unstoppable onchain fiat on/off-ramp P2P protocol with decentralized adjudication.

Built with ❤️
- D K [@delken_](https://twitter.com/delken_)
- 0xVincent.eth [@0xvincent_eth](https://twitter.com/0xvincent_eth)
- bae jae kka [@BroJustKiddinn](https://twitter.com/BroJustKiddinn)

*Link to organization: https://github.com/FiatX*
*Link to [prototype]([url](https://www.figma.com/proto/3R50iekhx7heSsngNIl4sk/FiatX-v0?page-id=93%3A10&type=design&node-id=93-11&viewport=75%2C223%2C0.04&t=aU95BMLlMabchvtP-1&scaling=min-zoom&mode=design))*

## Description
Crypto is now a trillion-dollar asset class. Billions of dollars of economic activity are facilitated by smart contracts deployed across various blockchain networks. For the two billion people living under autocratic regimes, crypto is the only unstoppable lifeline that allows them to take back control and have sovereignty over their wealth and finances. Crypto is the free world’s answer to the increasing encroachment of the traditional financial system.

However, the rails for people to onboard into crypto remains extremely brittle. As of now, all fiat-crypto on/off-ramps are operated or maintained by centralized entities (e.g., CEXs, payment processors, banks, etc.). This current status quo presents an existential risk to crypto access for the people who need it the most — people living under despotic governments are at the mercy of their regulatory bodies. If these regulators ever decide to completely block off crypto access, all they need to do is to go after the centralized fiat on/off-ramp chokepoints, and it’d become extremely difficult for people under their jurisdiction to be able to even onboard to crypto, let alone at scale.

Enter FiatX, an unstoppable onchain fiat on/off-ramp P2P protocol. FiatX is the on/off-ramp that everyone will turn to during the darkest times. When the going gets tough, we ensure that crypto will always remain accessible to everyone part of the traditional financial system, irrespective of who they are or where they’re based in. FiatX allows everyone to live in peace, knowing that their fiat can always be exchanged for crypto (and vice versa) regardless of the prevailing world circumstances.

### Onramp flow
- Merchant decides on their exchange rate (e.g., 1.9162 AUD/USDT) and max transaction size. The merchant will then be asked to deposit collateral slightly above the intended transaction size (101%) before their ad gets registered onchain on FiatX registry. Anyone can then enter into an onramp P2P trade with any registered merchant.
- Upon P2P trade initialization, a new one-time escrow is deployed. The merchant’s collateral is then locked, and the user may proceed to transfer fiat to the merchant’s bank account.
- The merchant checks fiat receipt and whether the amount transferred by the user is correct. Once satisfied, the merchant settles the onramp P2P trade (the collateral is released and the escrow is then dissolved).
- In the event of conflict (e.g., non-receipt of fiat funds, wrong amount), any party from the user or merchant can raise a dispute. The onramp escrow goes into “dispute” status, and the merchant’s collateral is temporarily frozen.
- A random set of 5 adjudicators are randomly chosen from the FiatX onchain adjudicators list via Chainlink VRF. Any individual / entity may submit candidacy for election to the list, to be approved by FiatX tokenholder governance (quadratic voting) via WorldID Sybil verification. FiatX governance may also vote to remove a bad-faith adjudicator from the adjudicators list.
- User and merchant submits file evidence corresponding to the dispute (e.g., screenshots, bank statements, screen recording, etc.) — files are encrypted with the pubkeys of the 5 selected adjudicators, then is stored on AvailDA for the duration of the dispute (<30d).
- Adjudicators retrieve data from AvailDA, then decrypt it with their private keys to see the submitted file evidence of both parties. Based on the provided evidence, adjudicators individually come to a decision and vote for the party they deem to be truthful.
- Votes are tallied onchain, and collateral is either unlocked back to the merchant or reimbursed to the user depending on majority decision. The overcollateralized portion of the collateral (1%) is then distributed prorata to the adjudicators as incentive for continuing good-faith voting.

### Offramp flow
- Merchant decides on their exchange rate (e.g., 1.9162 AUD/USDT) and max transaction size. The merchant gets their ad registered onchain on FiatX registry, and anyone can then enter into an offramp P2P trade with any registered merchant.
- Upon P2P trade initialization, the user is asked to deposit crypto slightly above their intended transaction size (101%), and a new one-time escrow is deployed. The user’s collateral is then locked, and the merchant may proceed to transfer fiat to the user’s bank account.
- The user checks fiat receipt and whether the amount transferred by the merchant is correct. Once satisfied, the user settles the onramp P2P trade (the collateral is released and the escrow is then dissolved).
- In the event of conflict (e.g., non-receipt of fiat funds, wrong amount), any party from the user or merchant can raise a dispute. The onramp escrow goes into “dispute” status, and the user’s collateral is temporarily frozen.
- The rest of the offramp dispute flow will be exactly similar with the onramp dispute flow.

## How it’s made
To prevent Sybil games arising from market participants (user, merchant, voter), we implement Worldcoin’s WorldID proofs to ensure that a pseudonymous address owner is part of Worldcoin’s Orb-verified humans before registering each address into our verifiedHumanAddrRegistry contract.

We leverage Chainlink VRF to generate a random set of 5 adjudicators out of FiatX’s adjudicator list (each adjudicator may be elected by governance and registered onchain).

We use AvailDA to ensure data availability of uploaded encrypted file evidence relevant to each disputeID within the dispute period (<30d). Selected adjudicators of that dispute can then use their private keys to individually decrypt the data for private viewing, before voting for an outcome based on said evidence.

FiatX’s set of Solidity contracts are deployed on Mantle, Base, and Polygon Cardona zkEVM.

We sprinkle a bit of Nounish identity with Nouns-themed UI to elevate the user experience on using the FiatX on/off-ramp.
