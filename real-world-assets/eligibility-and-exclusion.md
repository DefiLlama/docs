# Eligibility & Exclusion

This page explains how DeFiLlama decides whether an asset belongs in the RWA universe.

The goal is simple: include assets that have a real link to something offchain, and exclude assets that do not.

This page is about eligibility. It does not replace the [category taxonomy](definitions-and-taxonomy.md), [access model](methodology-and-metrics.md), or [evidence flags](methodology-and-metrics.md). Instead, it sits above them and answers a basic question: should this asset be listed as part of the RWA universe at all?

---

## Why This Page Exists

Not every RWA looks the same.

Some tokens give a direct claim on an underlying asset. Others give economic exposure through a fund, trust, note, SPV, or other legal structure. Some are transferable and self-custodial. Others can only be held through a platform or approved wallet.

That means an asset should not be excluded just because its structure is different. Instead, the focus should be on whether the asset has a real offchain basis, a clear explanation of how the token connects to it, and enough public information to support the listing.

---

## Scope

The inclusion criteria below apply to entries that claim to represent real-world asset exposure.

A small number of entries such as Non-RWA Stablecoins and Crypto Funds are tracked on the dashboard for context and comparison purposes even though they are not themselves RWA-backed. These entries are governed by their own category definitions rather than the inclusion criteria on this page.

Similarly, entries typed as Platform or Wrapper are eligible provided they have a meaningful connection to the RWA ecosystem. A platform that facilitates issuance, custody, or trading of tokenized real-world assets belongs on the dashboard. A wrapper that repackages one or more assets that themselves meet the inclusion criteria is also eligible, provided the wrapper's structure and underlying holdings are publicly documented. A token with no meaningful RWA involvement should not be listed, regardless of how it is submitted.

---

## Inclusion Criteria

An asset can be included in the RWA universe if all of the following are true.

### 1. It Points to a Real Offchain Asset or Exposure

The token must represent one of the following: a real-world asset, a claim on a real-world asset, a fund or trust interest, a commodity holding, a property interest, a receivable or credit exposure, a security or private-market position, or another clearly explained offchain economic exposure.

If there is no real offchain reference point, it does not belong in the RWA universe.

### 2. The Connection Is Explained Publicly

There must be a clear public explanation of how the token connects to the offchain asset or exposure. Examples include custody of the underlying asset, reserve backing, SPV exposure, fund structure, note or certificate terms, a redemption process, a servicing structure, or a property or project structure.

The exact structure can vary. What matters is that the connection is real and explained.

### 3. A Responsible Entity Can Be Identified

There must be a party that can be identified as responsible for the product, such as an issuer, operator, manager, SPV, trust, fund, platform entity, or other accountable legal or operating party.

This does not require perfect regulatory clarity. It does require that the product is not anonymous or impossible to attribute.

### 4. There Is Enough Public Evidence to Support the Listing

At minimum, an includable asset should have an official product page or issuer page (or both), a live token or contract where applicable, a description of what the token represents, and enough supporting material to understand the structure.

Supporting material can include legal terms, reserve pages, redemption terms, offering documents, custody disclosures, fund documents, prospectuses, proof-of-reserves pages, or other category-appropriate documentation.

Different asset types will have different kinds of proof. A commodity token and a tokenized fund should not be expected to prove themselves in exactly the same way. Whether the available evidence is sufficient is assessed by the DeFiLlama RWA team on a case-by-case basis.

### 5. The Product Is Live

The asset must be a live tokenized product or a live onchain exposure. Prelaunch pages, abandoned products, dead contracts, placeholders, and discontinued instruments should not be included as active RWA assets.

---

## Exclusion Criteria

An asset should be excluded if any of the following are true.

### 1. No Real Offchain Asset or Exposure Can Be Identified

If the token does not map to a real-world asset, claim, fund interest, commodity holding, property interest, security, receivable, or clearly explained offchain exposure, it should be excluded.

### 2. No Credible Linkage Mechanism Is Disclosed

If there is no public explanation of how token value links to the offchain asset or exposure, the asset should be excluded. Claims such as "backed" or "linked" are not enough on their own. There must be enough information to understand what that means in practice.

### 3. No Responsible Entity Can Be Identified

If no issuer, operator, SPV, trust, fund, manager, or platform entity can be identified, the asset should be excluded.

### 4. The Public Evidence Is Too Thin

If there is no official product page, no issuer information, no legal terms, no product description, and no meaningful supporting documentation, the asset should be excluded.

### 5. The Product Is Materially Misleading

If the public presentation materially misstates the nature of the asset — such as implying direct ownership when the product only provides indirect exposure — the asset should be excluded until the listing can be corrected.

### 6. The Product Is Not Live

If the product is prelaunch, discontinued, inactive, abandoned, or represented only by a dead or obsolete contract, it should be excluded.

---

## What Does Not Automatically Exclude an Asset

The following should not be treated as automatic reasons to exclude an asset: no direct shareholder rights, no voting rights, no dividend rights, SPV-backed structure, note-based or certificate-based structure, permissioned minting or redemption, allowlisting, lack of self-custody, lack of transferability, and lack of public attestation.

These features may change how an asset is classified or displayed. They do not, by themselves, make the asset ineligible.

---

## Expulsion Criteria

An asset may be removed after listing if new information shows that it no longer meets the standard for inclusion.

An asset should be expelled if any of the following become true: the underlying exposure is shown to be false or unsupported, the responsible entity disappears or disclaims responsibility, the token no longer maps to the underlying asset or exposure, the product is terminated or becomes economically inactive, material deception becomes evident, or the listing becomes a dead, duplicate, or obsolete contract.

Assets that materially change their structure, backing, or responsible entity may be reclassified or placed under review rather than expelled outright.

Where the issue is temporary or still under review, the asset can be placed in a pending or review state instead of being removed immediately. Expelled assets are moved to an internal excluded list and are not deleted from the data. They can be reinstated if the conditions that led to expulsion are resolved.

---

## Summary

The RWA universe aims to be broad enough to capture the real range of tokenized real-world assets, while still being strict about substance.

That means an asset should not be excluded just because it uses a trust, SPV, note, platform account, or other non-standard structure. It does mean that assets lacking a real offchain basis, a clear linkage mechanism, an identifiable responsible entity, a live product, or enough public information to support the listing should be excluded.
