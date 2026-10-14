# Reflection

1. What is static code analysis?

It examines source code without running it to find bugs, security
vulnerabilities, and style problems early.


2. How do Pylint, Bandit, and Flake8 differ?

Pylint checks code quality and logic issues, Bandit checks security
vulnerabilities, and Flake8 enforces PEP 8 style. Some issues overlap,
for example the bare `except` was flagged by all three.


3. Why fix issues by severity?

High-severity security issues such as `os.system` and MD5 can be exploited,
so they matter more than formatting. Fixing them first reduces real risk.


4. How did you verify the fixes?

I reran all three tools after each stage, compared the before and after
reports, and ran the program again to confirm the output was unchanged
apart from the intended hash and token changes.


**What surprised me**

I learned that one unsafe coding practice can be detected by multiple static analysis tools. For example, removing `eval()` addressed both a code-quality warning from Pylint and a security warning from Bandit. I also learned that a high Pylint score alone does not guarantee that code is secure, which is why using all three tools is important.