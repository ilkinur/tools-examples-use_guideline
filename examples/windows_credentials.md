# Credential Stores Summary


| Store | What It Holds | Why It Exists | Access Method | Tools & Commands |
|------|---------------|---------------|---------------|------------------|
| **LSASS Memory** | NTLM hashes, Kerberos tickets, cleartext (sometimes) | Enables seamless logon across services | Dump live memory of `lsass.exe` | `mimikatz`<br>`sekurlsa::logonpasswords`<br>`sekurlsa::minidump` |
| **SAM + SYSTEM Hives** | Local account hashes | Authentication for local logons (e.g. local admin) | Export registry hives, extract with boot key | `mimikatz`<br>`lsadump::sam`<br>`vssadmin` to extract |
| **LSA Secrets** | Cached domain creds, plaintext service creds | Enables offline logon, stores scheduled task passwords, and RDP secrets | RPC via LSARPC named pipe | `secretsdump.py` with local admin creds |
| **DPAPI Vault** | Saved passwords from apps (RDP, browsers, WiFi) | User-level secure storage for creds | Access via user token or decrypted master key | `mimikatz`<br>`vault::list`<br>`vault::cred /export` |
| **NTDS.dit (DC only)** | Full domain DB: usernames, NTLM & Kerberos keys | Domain authentication and replication | Replicate over MS-DRSR or parse an offline file | `secretsdump.py -just-dc`<br>`lsadump::dcsync` |
