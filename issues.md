# Issues Found and Fixed

Tools: Pylint, Bandit, Flake8. Line numbers refer to the original file.

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| Shell injection via `os.system` | Security, High (Bandit B605) | 63 | Filename is concatenated into a shell command | Replaced with `print()` |
| Weak MD5 hash | Security, High (Bandit B324) | 52 | MD5 is broken for security use | Switched to `hashlib.sha256` |
| `eval()` on `discount_expr` | Security, Medium (Bandit B307, Pylint W0123) | 47 | Arbitrary code execution through the expression string | Replaced with a numeric `discount_rate`, validated to 0..1 |
| Hardcoded secret, printed to console | Security, Low (Bandit B105) | 75, 84 | Credential in source and leaked in program output | Read from `REGISTRAR_AUTH_TOKEN` env var, print only whether it is set |
| Insecure `random` for OTP | Security, Low (Bandit B311) | 56 | Predictable PRNG | Used `secrets.randbelow` |
| Mutable default argument | Bug (Pylint W0102) | 15 | `audit_trail=[]` is shared across calls | Default `None`, module-level `AUDIT_LOG` |
| Bare `except: pass` | Bug (Pylint W0702, Flake8 E722, Bandit B110) | 31 | Silently hides all errors | Explicit `None` check, returns a status |
| `== None` comparison | Style (Flake8 E711, Pylint C0121) | 37 | Should use identity check | `is not None` |
| `global` statement, non-constant name | Design (Pylint W0603, C0103) | 12, 16 | Mutable global state | `TUITION_LEDGER` dict |
| Unused import and variables | Quality (F401, F841, W0611, W0612) | 4, 36, 46, 68 | Dead code | Removed |
| camelCase names, missing docstrings | Style (C0103, C0116, C0114) | many | PEP 8 violations | snake_case names and docstrings |
| File handling | Quality (Pylint W1514, R1732) | 60 | No encoding, file not in `with` | `with open(..., encoding="utf-8")` |
| Long lines, missing final newline | Style (E501, W292, C0301, C0304) | 7-9, 15, 23, 79, 87 | PEP 8 violations | Wrapped lines, added newline |

## Results

- Pylint: 4.00/10 before, 10.00/10 after
- Bandit: 6 findings (2 High, 1 Medium, 3 Low) before, 0 after
- Flake8: 11 findings before, 0 after
- Program behavior: same output after the changes, except the hash value (SHA-256 now) and the token line

## Logic problem no tool flags

`drop_course` raises `seats` without limit, so dropping a course nobody enrolled in creates a free seat. The code never stores original capacity. Not fixed, because it needs a design change.