# Security Review Checklist

## Input Validation
- [ ] All user inputs validated (type, range, format)
- [ ] Parameterized queries used (no string interpolation in SQL)
- [ ] User-controlled data never used in `eval()`, `exec()`, or system commands
- [ ] File uploads validated (type, size, content)
- [ ] Path traversal prevented (no `../` in file paths)

## Authentication & Authorization
- [ ] Authentication required for sensitive endpoints
- [ ] Authorization checks on every protected resource
- [ ] Passwords hashed with bcrypt/argon2 (never MD5/SHA1)
- [ ] Session tokens are random, long, and rotate on login
- [ ] Failed login attempts are rate-limited

## Data Protection
- [ ] No hardcoded secrets, API keys, or passwords
- [ ] Sensitive data in environment variables
- [ ] Secrets not logged or exposed in error messages
- [ ] HTTPS enforced for all data in transit
- [ ] Sensitive data encrypted at rest

## Injection Prevention

### SQL Injection
```python
# BAD
query = f"SELECT * FROM users WHERE id = {user_id}"

# GOOD
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))
```

### XSS
```javascript
// BAD
element.innerHTML = userInput;

// GOOD
element.textContent = userInput;
```

### Command Injection
```python
# BAD
os.system(f"convert {filename} output.png")

# GOOD
subprocess.run(["convert", filename, "output.png"], check=True)
```

## Dependencies
- [ ] No known vulnerable packages (check CVE databases)
- [ ] Dependencies pinned to specific versions
- [ ] Minimal dependency footprint

## API Security
- [ ] Rate limiting on public endpoints
- [ ] CORS properly configured (not `*` in production)
- [ ] Request size limits enforced
- [ ] Sensitive data not in URL parameters
