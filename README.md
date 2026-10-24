# PRIMEx-AUTH

Authentication and licensing database used by PRIMEx tools (ORBIT OPTIMIZER and related projects). It stores user accounts, license tiers, expiry dates, optional HWID binding and ban flags in a single flat JSON file.

![Format](https://img.shields.io/badge/Format-JSON-2DD4BF?style=for-the-badge&logo=json&logoColor=white)
![Type](https://img.shields.io/badge/Type-License%20Backend-8B98A5?style=for-the-badge)

## Schema

Top level:

| Field  | Type   | Description                        |
|--------|--------|------------------------------------|
| `users`| array  | List of user entries (see below)   |

Each user entry:

| Field      | Type    | Description                                            |
|------------|---------|--------------------------------------------------------|
| `id`       | string  | Unique user / license identifier                       |
| `password` | string  | SHA-256 hash of the user's password (never plaintext)  |
| `tier`     | string  | License level, e.g. `free`                             |
| `expiry`   | string  | ISO date (`YYYY-MM-DD`) when the license expires       |
| `hwid`     | string  | Hardware ID binding (empty string = not bound yet)     |
| `banned`   | boolean | `true` blocks authentication for this user             |

## Example entry

```json
{
  "id": "USERNAME",
  "password": "e1ba4807a15d8579f79cfd90a07fc015e6125565c9271eb94aded0b2ebf86163",
  "tier": "free",
  "expiry": "2026-09-29",
  "hwid": "",
  "banned": false
}
```

## Security notice

- This repository currently contains **live user entries** (hashed passwords, tiers, expiry dates). Treat it accordingly.
- Only SHA-256 hashes are stored, never plaintext passwords.
- If you fork or reuse this format, keep your **real** database in a **private** repository and publish only the schema here.
- For production use, prefer a proper database with salted/adaptive password hashing (bcrypt, argon2) instead of a public flat file.
