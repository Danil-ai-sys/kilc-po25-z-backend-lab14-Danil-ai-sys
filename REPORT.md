# Report, lab 14

Fill this in as you go. Write in your own words.

**Name: Danil Sobar**
**Group: PO 25-Z**
**Date: 6.11.26**

## What works

Tick only what works. "Almost works" is not ticked.

- [x] M1: `hash_password` and `verify_password`
- [x] M2: registration stores a hash, the answer has no password
- [x] M3: my test of the salt
- [ ] Bruno: ___ of 38 green
- [x] `pytest`: 52 passed, 1 failed

## What the database keeps

After the Bruno run, copy Aida's row from `SELECT id, name, password_hash FROM users`:

```
```

Mark in your copy: the algorithm, the cost settings, the salt, the hash.

## Two users, one password

Aida and Timur have the same password. Why are their hashes different, and how does `verify_password` still know that the password is right?

SHA-256 is too fast. It is designed to hash data as quickly as possible. A modern GPU can try billions of SHA-256 hashes per second, so if the database leaks, most ordinary passwords can be guessed in minutes or hours. Argon2 is deliberately slow and memory-hungry, and its cost is tunable. Each guess becomes expensive for an attacker, while a legitimate login only takes a fraction of a second.

## SHA-256 and Argon2

Why is `hashlib.sha256(password.encode()).hexdigest()` not good enough for passwords? Give two reasons.

SHA-256 has no salt. hashlib.sha256(password.encode()).hexdigest() always gives the same output for the same password. Users with the same password get identical hashes, which is visible straight from the database. An attacker can also precompute a table of password → hash pairs (a rainbow table) and crack many accounts at once. Argon2 adds a random salt per user, so precomputed tables are useless and every password has to be attacked separately.