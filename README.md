# EWB Phishing URL Awareness Checker

## Overview
Phishing is a trick where fake links or messages try to make people share passwords, card numbers or OTPs. This command-line tool reads the text of a URL and points out six visible warning signs. It never opens the URL or sends data anywhere.

## Features
- Detects an @ symbol in the URL
- Flags unusually long URLs (over 100 characters)
- Flags IP-address hostnames
- Flags many hostname segments (5 or more)
- Flags punycode-style hostnames (xn--)
- Flags the HTTP scheme
- Loop: check many URLs in one run and type `q` to quit
- Safe handling of malformed input, plus a "REVIEW CAREFULLY" summary when warnings are found

## Requirements
Python 3.10 or later. No extra packages.

## How to Run
Windows: `python phishing_checker.py`
Mac/Linux: `python3 phishing_checker.py`

## Example Output
```
EWB Phishing URL Awareness Checker
Text checks only - this program does not open URLs.

Enter a sample URL (or type q to quit): http://192.0.2.10/account

Result:
- Uses an IP address instead of a familiar domain.
- Uses HTTP. HTTPS alone does not prove trust.

REVIEW CAREFULLY: 2 warning sign(s) found.
Never enter passwords from unexpected links.
```

## Testing
| # | Input | Expected | Actual | Result |
|---|-------|----------|--------|--------|
| 1 | https://www.example.com/login | No warning signs | No listed warning signs found. This does NOT prove safety. | PASS |
| 2 | http://192.0.2.10/account | IP address + HTTP | IP address and HTTP warnings, 2 total | PASS |
| 3 | http://example.com@sample.invalid/login | @ symbol + HTTP | @ symbol and HTTP warnings, 2 total | PASS |
| 4 | https://a.b.c.d.e.example.invalid/path | Many hostname segments | Many hostname segments warning | PASS |
| 5 | https://xn--example.invalid/ | Punycode-style hostname | Punycode-style hostname warning | PASS |

The full table is in `tests/test_samples.md`.

## Screenshots
![Sample run](screenshots/sample_run.png)
![Test results](screenshots/test_results.png)

## Improvements
1. **Loop with quit (Improvement A)** - the program now repeats until the user types `q`, so several URLs can be checked in one run. I tested it by entering two URLs in a row and then `q`.
2. **Malformed input handling (Improvement C)** - `http://[bad` used to crash with a `ValueError`. I wrapped the parsing line in `try / except` so the program prints a friendly message instead. I tested it with `http://[bad` and re-ran all five sample tests.
3. **Review carefully summary (Improvement B)** - a helper function tells real warnings apart from information messages, and the program prints how many warning signs were found.

## Limitations
This is an educational tool. It only reads URL text. It does not visit URLs, scan for malware or prove that a URL is safe. No warning signs found does NOT mean a URL is safe.

## Ethical Use
Use sample strings only. Do not collect credentials. Do not use this project to deceive anyone or to test websites without permission.

## AI Use
I used Claude (Anthropic) to help draft the code, the test table and this README from the EWB workbook. I am responsible for running, testing and understanding the project.


## Author
Gantela Sai Sharanya, [batch or course], EWB Courses
