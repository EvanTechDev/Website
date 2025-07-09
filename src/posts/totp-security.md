
---
title: "TOTP: Time-Based One-Time Password"
description: "TOTP (Time-Based One-Time Password) enhances 2FA security by generating a temporary 6-digit code based on the current time and a shared secret key. It offers offline password generation but requires time synchronization and secure key storage to mitigate risks."
date: "2025-02-19"
type: "blog"
sitemap: true
---

## **What is TOTP?**

**TOTP**, or **Time-Based One-Time Password**, is a security mechanism used for two-factor authentication (2FA). It generates a temporary password (usually a 6-digit number) that changes based on the current time, expiring after a fixed interval (typically 30 seconds). TOTP is widely used to enhance the security of online accounts, such as those of Google, Dropbox, and GitHub.

## **Principles of TOTP**

### **Basic Concepts**

The implementation of TOTP is based on the following core concepts:

- **Shared Secret Key**: A secret shared between the user and the server. Typically, this key is entered into an authenticator app via QR code or manual input.
- **Time Step**: By default, a new password is generated every 30 seconds.
- **Hash Function**: Commonly, the HMAC-SHA1 algorithm is used to generate the password.

### **Steps to Generate a Password**

1. **Get Current Time**:
    - The client (e.g., a TOTP app on a smartphone) retrieves the current time, typically in UNIX timestamp format.
2. **Calculate Time Step**:
    - Divide the current time by the time step (e.g., 30 seconds) to get an integer *T* , which represents the current time step.
3. **Generate HMAC with Shared Key and Time Step**:
    - Use the shared secret key and the current time step *T*  to compute a hash value using the HMAC-SHA1 algorithm.
4. **Dynamic Truncation**:
    - Select 4 bytes from the HMAC result and use an algorithm (like the dynamic truncation defined in RFC 4226) to generate a 32-bit integer.
5. **Generate OTP**:
    - Modulo this 32-bit integer by *10^6* (since OTPs are generally 6 digits) to obtain the final OTP.

**Formula:**

The calculation of the TOTP value can be simplified as:

$$
TOTP = \text{Truncate}(HMAC-SHA1(K, T)) \mod 10^6
$$

Where:

- `K` is the shared secret key.
- `T` is the current time step.
- `\text{Truncate}` represents dynamic truncation.

**Verification Process**

- **The client** generates a TOTP value and inputs it.
- **The server** calculates the TOTP based on the shared secret key and the server's current time. If the TOTP provided by the client falls within a reasonable time window (usually one or several time steps before or after), the verification is successful.

**Advantages and Challenges**

- **Advantages**:
    - **Enhanced Security**: Even if the password is compromised, attackers have a very small window to use it.
    - **No Network Required**: The client can generate passwords offline.
- **Challenges**:
    - **Time Synchronization**: Client and server times must be synchronized, usually through the NTP protocol.
    - **Secure Storage of Shared Keys**: If the key is compromised, security is at risk.

By understanding the basic principles of TOTP, you can better leverage this mechanism to secure your online accounts, ensuring that even if passwords are stolen, the account remains difficult to access illegally.

**Implementation Best Practices**

When implementing TOTP in your applications, consider using established libraries and following security standards like RFC 6238. Always generate cryptographically secure shared keys, implement rate limiting for verification attempts, and provide backup codes for account recovery. These measures ensure a robust and user-friendly two-factor authentication system.
