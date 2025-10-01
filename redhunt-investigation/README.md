# ⭐ OSINT Challenge - SecurityBlue Team | Redhunt Investigation ⭐

![SecurityBlue](https://img.shields.io/badge/SecurityBlue-OSINT_Course-blue)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green)
![Score](https://img.shields.io/badge/Score-10%2F10-brightgreen)
![Category](https://img.shields.io/badge/Category-OSINT-red)
![Status](https://img.shields.io/badge/Status-Completed-success)

A comprehensive OSINT investigation into a fictional MSP (Managed Service Provider) breach scenario. This project demonstrates practical open-source intelligence gathering techniques to identify, profile, and link a person of interest to malicious activities.

**Level:** Easy  
**Category:** OSINT Investigation / Educational Challenge  
**Platform:** SecurityBlue Team  
**Tags:** OSINT, Digital Forensics, Social Engineering, DNS Analysis, Steganography, Data Breach Investigation

---

## Table of Contents

- [Overview](#-overview)
- [Challenge Scenario](#-challenge-scenario)
- [Objectives](#-objectives)
- [Tools & Methodology](#-tools--methodology)
- [Investigation Process](#-investigation-process)
- [Key Findings](#-key-findings)
- [Lessons Learned](#-lessons-learned)
- [Disclaimer](#%EF%B8%8F-disclaimer)

---

## Overview

This write-up documents my approach to the **Introduction to OSINT Course Challenge** from SecurityBlue Team. The investigation required tracking a person of interest (POI) suspected of being involved with a hacking group responsible for compromising an MSP and attempting to sell stolen credentials.

**Starting Point:** Twitter handle `@sp1ritfyre`

**Final Score:** 10/10 ✅

---

## Challenge Scenario

You work for a law enforcement organization investigating a hacking group that recently:
- Compromised a Managed Service Provider (MSP)
- Attempted to sell stolen credentials on both the clear net and dark web

**Mission:** Use OSINT techniques to:
1. Build a comprehensive profile of the person of interest
2. Identify their online presence
3. Find evidence linking them to the MSP breach

> ⚠️ **Note:** This is a fictional scenario created for educational purposes. Any resemblance to real persons is purely coincidental.

---

## Tools & Methodology

### OSINT Tools Used

| Category | Tools |
|----------|-------|
| **Search & Discovery** | Google Dorks, Twitter/X, TinyEye |
| **Domain Analysis** | VirusTotal, ANY.RUN, SecurityTrails, DNSDumpster |
| **Historical Data** | Wayback Machine |
| **Decoding** | CyberChef, xxd (Kali Linux), Python3, RapidTables |
| **Frameworks** | OSINT Framework, Maltego |

### Methodology Approach

```
Initial Lead (Twitter) → Profile Discovery → Blog Analysis → Domain Investigation
→ DNS Records → Credential Recovery → Evidence Collection
```

---

## Investigation Process

### Phase 1: Profile Discovery

**Starting Point:** `@sp1ritfyre` on Twitter

#### Discovery Chain:
1. **Google Search** revealed Blogger profile
2. **Hexadecimal location** decoded to first blog URL
3. **First blog** contained another hex string leading to second blog

```bash
# Decoding location from Blogger profile
echo -n '68747470733a2f2f73616d6d6965776f6f647365632e626c6f6773706f742e636f6d' | xxd -r -p
# Output: https://sammiewoodsec.blogspot.com
```

### Phase 2: Blog Analysis

**Blogs Identified:**
- `https://sp1ritfyrehackerstories.blogspot.com/`
- `https://sammiewoodsec.blogspot.com/`

#### Key Information Extracted:
- Full name, age, location
- Employment details
- Professional contacts
- Interests and hobbies
- Email address: `d1ved33p@gmail.com`

#### Hidden Flags Found:
In GitHub repository commits, discovered binary-encoded messages:

```
01011001 01101111 01110101 00100111 01110110 01100101...
Decoded: "You've Found Flag Three! ERIQOQMAOPQ"
```

### Phase 3: Twitter Deep Dive

**Suspicious URL Found:** `https://cmvkahvudc5uzxqk.xyz/`

#### Security Analysis:
- ✅ **VirusTotal:** Flagged as phishing/malware
- ✅ **ANY.RUN:** Detected malicious activity

#### Domain Decoding:
The domain name was Base64 encoded:

```bash
echo 'cmVkaHVudC5uZXQK' | base64 -d
# Output: redhunt.net
```

### Phase 4: Domain Investigation - redhunt.net

**Evidence of Ownership:**
- Website featured the same red neon lightbulb image from Twitter/blogs
- Copyright footer: `sp1ritfyre`
- Protected `/contact` page discovered

### Phase 5: Credential Recovery via DNS

**Challenge Hint:** "DNS TXT records can be used to show text strings. Don't forget to check if the owner has left a comment!"

#### DNS TXT Record Discovery:
Using **SecurityTrails**, found historical TXT record:

```
70 61 73 73 3a 32 71 33 34 73 65 72 64 74 66 79 76 67 75 68 62 30 30 39 69 75 68 6a 62
```

#### Decoding the Password:

```bash
echo "70 61 73 73 3a 32 71 33 34 73 65 72 64 74 66 79 76 67 75 68 62 30 30 39 69 75 68 6a 62" | xxd -r -p
# Output: pass:2q34serdtfyvguhb009iuhjb
```

**Password:** `2q34serdtfyvguhb009iuhjb`

### Phase 6: Protected Page Access

Accessed `/contact` page using recovered credentials:
- Confirmed email address
- Established direct link to MSP breach infrastructure

---

## Key Findings

### Target Profile

| Attribute | Information |
|-----------|-------------|
| **Full Name** | Sam Woods |
| **Age** | 23 years |
| **Location** | Reading, United Kingdom |
| **Employer** | PhilmanSecurityInc |
| **Position** | Junior Penetration Tester |
| **Email** | d1ved33p@gmail.com |

### Interests Identified
- Security & Programming
- Gaming & Photography
- Camping
- Malware Analysis
- Technology

### Digital Footprint

**Owned Domain:**
- `https://redhunt.net/`

**Blogs Used:**
- `https://sp1ritfyrehackerstories.blogspot.com/`
- `https://sammiewoodsec.blogspot.com/`

**Social Media:**
- Twitter: `@sp1ritfyre`

### Evidence of Malicious Activity

| Evidence | Type | Significance |
|----------|------|--------------|
| `cmvkahvudc5uzxqk.xyz` | Phishing URL | Flagged by VirusTotal & ANY.RUN |
| `redhunt.net` | Owned Domain | Linked via Base64 encoding |
| DNS TXT Records | Hidden Credentials | Indicates OPSEC awareness |
| Protected Pages | Restricted Access | Suggests sensitive content |

---

## Investigation Timeline

```mermaid
graph TD
    A[@sp1ritfyre Twitter] --> B[Blogger Profile]
    B --> C[Hex Decode]
    C --> D[Blog 1: Hacker Stories]
    D --> E[Hex Decode]
    E --> F[Blog 2: SammieWoodSec]
    F --> G[Personal Info]
    A --> H[Suspicious URL]
    H --> I[VirusTotal Analysis]
    H --> J[Base64 Decode]
    J --> K[redhunt.net]
    K --> L[DNS TXT Records]
    L --> M[Hex Decode Password]
    M --> N[Contact Page Access]
    N --> O[MSP Breach Evidence]
```

---

## 💡 Lessons Learned

### Technical Skills Enhanced
- Advanced Google Dorking techniques
- Multi-layer encoding detection (Base64, Hex, Binary)
- DNS record analysis for hidden information
- Social media pivoting strategies
- Correlation of multiple data sources

### OPSEC Observations
The target demonstrated:
- Use of pseudonyms and aliases
- Multi-layer encoding for sensitive data
- Password-protected content

However, critical mistakes included:
- Reusing distinctive imagery (red lightbulb)
- Linking accounts through breadcrumbs
- Storing credentials in DNS records
- Poor encoding practices (security through obscurity)

### Best Practices Applied
1. **Systematic Documentation:** Every step recorded for reproducibility
2. **Legal Compliance:** No unauthorized access attempts
3. **Multiple Source Verification:** Cross-referenced findings
4. **Ethical Boundaries:** Respected challenge guidelines

---

## Resources

- [SecurityBlue Team](https://www.securityblue.team/)
- [Introduction to OSINT Course](https://www.securityblue.team/courses/)
- [OSINT Framework](https://osintframework.com/)
- [SecurityTrails](https://securitytrails.com/)
- [VirusTotal](https://www.virustotal.com/)

---

## ⚠️ Disclaimer

This investigation was conducted as part of an educational challenge provided by SecurityBlue Team. The scenario, individuals, and events are entirely **fictional** and created for learning purposes only.

**Legal Notice:**
- All techniques demonstrated were used in a controlled, authorized environment
- No actual systems were compromised
- Any resemblance to real persons or entities is purely coincidental

**Ethical Considerations:**
This write-up is shared for educational purposes to help others learn OSINT techniques responsibly and legally.

---

## 📝 License

This write-up is shared under the MIT License for educational purposes.

---

## ⭐ Acknowledgments

Special thanks to **SecurityBlue Team** for creating this engaging and educational OSINT challenge.

---

*Last Updated: September 2025*
