---
layout: single
title: "NPM Supply Chain Attack: Compromised debug and chalk packages affect billions of downloads"
tags:
  - npm
  - security
  - supply-chain
  - malware
  - crypto
  - web3
categories:
  - security
  - tech
excerpt: "On September 8, 2025, a widespread supply chain attack compromised 18 popular NPM packages including debug and chalk, affecting over 2 billion downloads per week. This attack targets crypto and web3 users by intercepting wallet interactions and rewriting payment destinations."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/npm-security.png
  overlay_image: /assets/2025/09/npm-security.png
---

**TL;DR**:
- **Date**: September 8, 2025
- **Affected Packages**: 18 popular NPM packages including `debug` (357.6M downloads/week) and `chalk` (299.99M downloads/week)
- **Impact**: Over 2 billion downloads per week affected
- **Malware Behavior**: Silently intercepts crypto/web3 activity, manipulates wallet interactions, and rewrites payment destinations
- **Infection Vector**: Phishing email from `support@npmjs.help` to package maintainers
- **Remediation**: Check affected versions, clean npm cache, reinstall with lock files, use security tools

# Massive NPM Supply Chain Attack Targets Crypto Users

On September 8, 2025, the JavaScript and Node.js ecosystem was hit by a significant supply chain attack that compromised 18 popular NPM packages. This attack is particularly concerning due to the massive scale of the affected packages and their widespread use in the development community.

## Compromised Packages and Versions

The following table lists the compromised packages and their affected versions:

| Package Name | Clean Version | Compromised Version | Downloads/Week |
|--------------|---------------|---------------------|----------------|
| debug | 4.4.1 | 4.4.2 | 357.6M |
| chalk | 5.6.0 | 5.6.1 | 299.99M |
| ansi-styles | 6.2.1 | 6.2.2 | 299.99M |
| simple-swizzle | 0.2.2 | 0.2.3 | 25M |
| strip-ansi | 7.1.0 | 7.1.1 | 65M |
| ansi-regex | 6.0.1 | 6.0.2 | 25M |
| supports-color | 9.4.0 | 9.4.1 | 25M |
| wrap-ansi | 8.1.0 | 8.1.1 | 25M |
| slice-ansi | 5.0.0 | 5.0.1 | 3M |
| color-convert | 2.0.1 | 2.0.2 | 25M |
| color-name | 2.0.0 | 2.0.1 | 25M |
| color-string | 1.9.1 | 1.9.2 | 15M |
| is-arrayish | 0.3.2 | 0.3.3 | 25M |
| error-ex | 1.3.2 | 1.3.3 | 25M |
| has-ansi | 4.0.1 | 4.0.2 | 1M |
| supports-hyperlinks | 2.3.0 | 2.3.1 | 1M |
| chalk-template | 0.4.0 | 0.4.1 | 200K |
| backslash | 0.1.1 | 0.1.2 | 100K |

Combined, these compromised packages affect over 2 billion downloads per week, making this one of the largest NPM supply chain attacks to date.

## Malware Behavior

The malicious code embedded in these packages is designed to target cryptocurrency and web3 users specifically. The malware:

1. Silently intercepts crypto and web3 activity in the browser
2. Manipulates wallet interactions and rewrites payment destinations to attacker-controlled addresses
3. Uses string-matching logic and Levenshtein distance calculations to replace legitimate wallet addresses with visually similar attacker addresses
4. Hooks into browser APIs like `fetch`, `XMLHttpRequest`, and wallet interfaces (`window.ethereum`, Solana) to alter network traffic and transactions before they are signed
5. Remains stealthy by avoiding UI changes when a wallet is detected

This type of attack is particularly dangerous because it operates silently and can redirect cryptocurrency payments without the user's knowledge.

## How the Attack Works

The attack follows these steps:

1. Injects into browser functions and APIs
2. Watches for sensitive data (wallet addresses across Ethereum, Bitcoin, Solana, etc.)
3. Replaces targets with attacker-controlled addresses using Levenshtein distance algorithm
4. Hijacks transactions at the signing stage
5. Remains undetected by avoiding UI changes when wallets are present

## Infection Vector

According to reports, the initial infection vector was a sophisticated phishing attack targeting package maintainers:

1. **Phishing Email**: Sent from the domain `support@npmjs.help` that appeared legitimate
2. **Fake 2FA Setup**: Directed maintainers to a fake page claiming 2FA needed updating
3. **TOTP Proxy Attack**: The malicious site captured usernames, passwords, and 2FA codes in real-time
4. **Account Takeover**: Attackers used the captured credentials to gain access to maintainer accounts
5. **Package Compromise**: With maintainer access, attackers published malicious versions of the packages

The domain `npmjs.help` resolved to IP `185.7.81.108` and used BunnyCDN buckets for infrastructure.

## Indicators of Compromise

Watch for these specific indicators:

- **Phishing domain**: `npmjs.help`
- **Malicious IP**: `185.7.81.108`
- **Compromised versions**: As listed in the table above
- **Malware signature**: Look for the string `checkethereumw` in package files
- **CDN domains**: `static-mw-host.b-cdn.net` and `img-data-backup.b-cdn.net`
- **C2 server**: `websocket-api2.publicvm.com`

## Remediation Steps

If you're a developer or organization using NPM packages, here are the recommended steps to protect yourself:

1. **Check if you're using affected versions** of the listed packages
2. **Clean your npm cache** to remove potentially compromised packages:
   ```bash
   npm cache clean --force
   ```
3. **Reinstall packages** using a lock file with pinned versions
4. **Use security tools** like Aikido SafeChain to scan packages before installation
5. **Monitor your applications** for unusual network activity or crypto transactions
6. **Audit dependencies** using tools like `npm audit` or Socket

## Broader Implications

This attack highlights several critical issues in the NPM ecosystem:

1. **Dependency Chain Vulnerabilities**: With packages having millions of downloads, a single compromise can affect a massive number of projects
2. **Maintainer Security**: The attack shows how targeting maintainers directly can be an effective way to compromise widely-used packages
3. **Supply Chain Risks**: Even trusted packages can become attack vectors, emphasizing the need for continuous monitoring
4. **Crypto Targeting**: The specific focus on crypto/web3 interactions shows how attackers are adapting their methods to target high-value financial transactions
5. **2FA Vulnerabilities**: The use of TOTP proxy attacks demonstrates that SMS/APP-based 2FA is still vulnerable to phishing

## Protection Strategies

To protect against similar attacks in the future:

1. **Use Security Keys**: Implement FIDO2/WebAuthn security keys instead of TOTP 2FA
2. **Pin Dependencies**: Use exact versions in your package-lock.json or yarn.lock files
3. **Audit Dependencies**: Regularly check your dependencies for known vulnerabilities
4. **Use Security Tools**: Implement tools that scan packages before installation
5. **Monitor Network Activity**: Watch for unusual outbound connections from your applications
6. **Enable Automation**: Use tools like Dependabot to automatically update dependencies with security patches
7. **Verify Maintainers**: Be cautious of sudden changes in package maintainers or unusual release patterns
8. **Implement CI/CD Scanning**: Integrate security scanning into your deployment pipeline

## Further Reading

For more information about this incident and related security topics:

- [GitHub Security Advisory GHSA-8mgj-vmr8-frr6](https://github.com/advisories/GHSA-8mgj-vmr8-frr6)
- [Aikido.dev: NPM debug and chalk packages compromised](https://www.aikido.dev/blog/npm-debug-and-chalk-packages-compromised)
- [Security Alliance: 2025-09 NPM Supply Chain Attack](https://www.securityalliance.org/news/2025-09-npm-supply-chain)
- [NPMDiff: simple-swizzle 0.2.2 to 0.2.3 comparison](https://npmdiff.dev/simple-swizzle/0.2.2/0.2.3/package/index.js/)
- [Hacker News Discussion](https://news.ycombinator.com/item?id=45169657&trk=public_post_comment-text)
- [LinkedIn Post on the Incident](https://www.linkedin.com/posts/advocatemack_malware-npm-supplychain-activity-7370829639537291264-jxZD/)
- [Twitter/X: InsiderPhD Analysis](https://x.com/InsiderPhD/status/1965110610972250550)
- [Twitter/X: WhichbufferArda Report](https://x.com/WhichbufferArda/status/1965139425475907774)
- [Twitter/X: hackerfantastic Commentary](https://x.com/hackerfantastic/status/1965143491539063169)

## Conclusion

This NPM supply chain attack serves as a stark reminder of the vulnerabilities inherent in modern software development practices. With millions of downloads per week, even popular and trusted packages can become attack vectors when maintainers are compromised.

Developers and organizations should take this incident seriously and implement robust security practices for their dependency management. The targeting of crypto and web3 users particularly emphasizes the need for constant vigilance in an increasingly connected and financially driven development landscape.

As the JavaScript ecosystem continues to grow, so do the potential attack surfaces. Staying informed about these threats and implementing proactive security measures is essential for maintaining the integrity and security of our applications.

The incident also highlights the importance of moving beyond traditional 2FA methods to more secure authentication mechanisms like hardware security keys, which would have prevented the TOTP proxy attack used in this compromise.