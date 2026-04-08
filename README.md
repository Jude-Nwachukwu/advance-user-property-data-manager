# 📘 **Advance User Data Manager – GTM Custom Tag Template**

** Here is the advance version of the [https://github.com/Jude-Nwachukwu/advance-user-property-data-manager](https://github.com/Jude-Nwachukwu/advance-user-property-data-manager)

## Overview
The **Advance User Data Manager** is a powerful Google Tag Manager custom tag template designed to collect, normalize, hash, merge, store, and optionally push user data into the dataLayer.

It supports **manual user inputs**, **user property object mapping**, **custom user-defined properties**, **secure hashing**, and a robust **merge-and-restore logic** that ensures durable and consistent user data across sessions.

This template is built for advanced, production-grade user data handling with maximum flexibility, control, and persistence.

---

# 🚀 Features

### ✔ Multiple Data Source Methods
- Manual user attribute input
- User property object mapping
- Custom user-defined attributes (fully flexible)
- Hybrid modes:
  - Manual + Object
  - Manual + Custom

---

### ✔ Advanced Custom User Properties
- Define **any user attribute dynamically**
- Control hashing per attribute (row-level control)
- Supports:
  - No hashing
  - SHA-256 Hexadecimal
  - SHA-256 Base64
- Unlike other modes:
  - Custom attributes **store only final value (hashed or raw)**
  - No `_sha` duplication

---

### ✔ Secure Hashing Support
- SHA-256 hashing
- Manual hashing (first name, last name, email)
- Object hashing (selected keys only)
- Custom hashing (per-row control)
- Output formats:
  - Hexadecimal
  - Base64
- Hashes regenerate **only when values change**

---

### ✔ Persistent Storage Layer
- Cookie-based storage (primary)
- Optional localStorage (secondary backup)
- Custom cookie name, domain, path
- Custom localStorage key
- Automatic cookie refresh on update

---

### ✔ Advanced Merge & Restore Logic
- Cookie = **Primary source**
- localStorage = **Fallback**
- If cookie exists → merge cookie + LS (cookie wins)
- If cookie missing → restore from LS
- New values merge safely
- Empty/null/undefined values **never overwrite existing data**

---

### ✔ Optional DataLayer Push
- Push user data into dataLayer
- Default or custom event name
- Clean and structured payload
- Includes clear source attribution

---

### ✔ Fully Async-Safe Execution
- Waits for all hashing operations
- No premature firing
- Proper success/failure callbacks:
  - `data.gtmOnSuccess()`
  - `data.gtmOnFailure()`

---

# 📥 Installation (GTM)

1. Open **Google Tag Manager**
2. Navigate to **Templates → Tag Templates**
3. Click **New → Import**
4. Upload the `.tpl` file
5. Save the template
6. Create a new tag using **Advance User Data Manager**
7. Configure using the guide below

---

# ⚙️ Configuration Guide

All field names below match **exact GTM UI display names**.

---

## 1️⃣ Select Data Source Method
**Field:** *Select the User Property Data Source Method*

### Available Options:

- **Manual Input Only**
- **User Property Object Only**
- **Both User Property Object and Manual Input**
- **Custom User Property Object Only**
- **Both Custom & Manual Property**

### Behavior Summary:

| Mode | Description |
|------|------------|
| Manual Only | Uses only manual inputs |
| Object Only | Uses mapped object |
| Manual + Object | Combines both |
| Custom Only | Uses only custom-defined attributes |
| Custom + Manual | Combines manual + custom attributes |

---

## 2️⃣ Configure Manual User Data
**Group:** *Configure Your User Data/Property Fields*

### Manual User Property Table

| Column | Description |
|--------|------------|
| User Data | Select predefined attribute |
| Enter Value | GTM variable or static value |

---

### ✅ Supported Manual Attributes

| Display Name | Stored Key |
|-------------|-----------|
| First Name | `firstName` |
| Last Name | `lastName` |
| Email | `email` |
| Phone | `phone` |
| Date of Birth | `dateOfBirth` |
| User ID | `userId` |
| Country | `country` |
| Region | `region` |
| City | `city` |
| Postal Code | `postalCode` |
| Address | `address` |
| Gender | `gender` |
| Full Name | `fullName` |

---

### 🔐 Manual Hashing
**Toggle:** *Enable hashing for first name, last name, and email address if collected*

Applies to:
- First Name
- Last Name
- Email

Formats:
- Hexadecimal
- Base64

Stored as:
```json
{
  "email": "john@example.com",
  "email_sha": "abc123..."
}
````


## 3️⃣ Configure User Property Object

**Group:** *Configure the User Object*
*(Visible in Object modes)*

### Fields:

* **User Property Object Variable**

### Mapping Table

| Column                      | Description               |
| --------------------------- | ------------------------- |
| Key Name                    | Output key (spaces → `_`) |
| Corresponding Attribute Key | Object key                |

---

### 🔐 Object Hashing

* Enable hashing
* Select format (Hex/Base64)
* Specify keys to hash

---

## 4️⃣ Configure Custom User Property

**Group:** *Custom User Property*
*(Visible in Custom modes)*

### Custom Property Table

| Column                | Description                  |
| --------------------- | ---------------------------- |
| Custom User Attribute | Output key name              |
| Hashing Status        | No Hashing / Hex / Base64    |
| Value                 | GTM variable or static value |

---

### 🔥 Important Behavior

* Each row is processed independently
* Hashing is applied **per row**
* Output behavior:

| Hashing Type | Stored Value      |
| ------------ | ----------------- |
| No Hashing   | Raw value         |
| Hexadecimal  | Only hashed value |
| Base64       | Only hashed value |

---

### ✅ Example

| Attribute | Hashing    | Value    |
| --------- | ---------- | -------- |
| job_role  | No Hashing | Designer |
| user_id   | Hex        | 12345    |

### Output:

```json
{
  "job_role": "Designer",
  "user_id": "a7f5f35426..."
}
```

> ⚠️ No `_sha` fields are created for custom properties.

---

## 5️⃣ Cookie Configuration

**Group:** *Cookie Configuration*

Options:

* Custom cookie name
* Duration unit:

  * Seconds / Minutes / Hours / Days
* Lifespan value
* Advanced:

  * Domain
  * Path

**Default Cookie Name:**

```
dd_cookie_gtm_user_data
```

---

## 6️⃣ LocalStorage Configuration

**Group:** *LocalStorage Config*

Options:

* Enable localStorage storage
* Custom localStorage key

**Default Key:**

```
dd_cookie_ls_user_data
```

---

## 7️⃣ DataLayer Configuration

**Group:** *DataLayer Configuration*

Options:

* Push data to dataLayer
* Custom event name

---

### Default Push

```js
{
  event: "gtm_dd_updm_user_data",
  datalayer_source: "dd gtm custom tag template",
  user_data: { ...merged user data... }
}
```

---

# 📊 Example Setups

## Example 1 — Custom Only Mode

| Attribute | Hashing    | Value   |
| --------- | ---------- | ------- |
| plan      | No Hashing | premium |
| user_id   | Hex        | 12345   |

### Output:

```json
{
  "plan": "premium",
  "user_id": "5994471abb..."
}
```

---

## Example 2 — Custom + Manual

### Manual:

| User Data | Value         |
| --------- | ------------- |
| Email     | {{dlv-email}} |

### Custom:

| Attribute | Hashing    | Value        |
| --------- | ---------- | ------------ |
| job_role  | No Hashing | {{dlv-role}} |

---

### Output:

```json
{
  "email": "john@example.com",
  "email_sha": "abc123...",
  "job_role": "Designer"
}
```

---

# 🧠 Execution Logic Summary

1. Load cookie (primary)
2. Load localStorage (fallback)
3. Merge cookie + LS
4. Apply:

   * Object mapping (if enabled)
   * Manual inputs (if enabled)
   * Custom attributes (if enabled)
5. Hash values (async-safe)
6. Write cookie + localStorage
7. Push to dataLayer (optional)
8. Trigger success/failure callbacks

---

# 🔗 Useful Links

### DumbData GTM Templates

[https://dumbdata.co/gtm-custom-templates/](https://dumbdata.co/gtm-custom-templates/)

---

# 📄 License

**Apache License 2.0**
Free for personal and commercial use.

```
```
