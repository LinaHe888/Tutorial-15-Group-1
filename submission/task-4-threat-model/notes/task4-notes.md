# Task 4 Notes

## Main weakness
- Session token generated with `java.util.Random`

## Attacker model
- Local attacker
- Reverse engineer / analyst
- Possible access to local app state in a testing or compromised environment

## Main impact
- Weakens session integrity
- Reduces unpredictability of authentication state

## Supporting context
- Plaintext credentials in `credentials.txt`
- Local session storage in `SessionPrefs`
