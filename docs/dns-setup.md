# DNS Configuration

## Domain: arvind.codes

### Registrar: Spaceship (spaceship.com)

#### Current Records (as of 2026-10-06)

| Record | Value | TTL | Notes |
|--------|-------|-----|-------|
| A (apex) | 200.234.46.131 (myvps) | 300 | IPv4 primary |
| AAAA (apex) | 2a01:4f9:c014:c516::1 | **1 min** ⚠️ | IPv6 primary — TTL TOO LOW |
| CNAME (www) | arvind.codes | 300 | HTTPS redirect configured |
| MX | (none) | — | Email handled externally |

#### IPv6 TTL Optimization

**Status:** PENDING  
**Issue:** #2

Current TTL is 1 minute (60 seconds) — excessive DNS queries from clients, especially mobile (Jio/Airtel IPv6-first networks).

**Action:** Update AAAA record TTL to 30 minutes (1800 seconds) at Spaceship DNS panel.

**Verification:**
```bash
dig arvind.codes AAAA +nocmd +noall +answer
# Expected: TTL should show ~1800
```

### Hosting: myvps (200.234.46.131)

- **OS:** Ubuntu 24.04 (Hostinger, Mumbai)
- **Nginx:** Proxy → :4100 (Next.js app)
- **SSL:** Let's Encrypt wildcard (`*.arvind.codes`)
- **HTTPS Redirect:** HTTP → HTTPS configured

### IPv6 Setup

- Full expanded IPv6: `2a01:4f9:c014:c516::1`
- **Note:** Spaceship validator requires full expanded form (no compressed `::` notation)
- Verified by: Spaceship DNS validation, IPv6 ping test from external host

---

## Maintenance

### Cert Renewal
Certbot auto-renewal via systemd timer (`certbot.timer`):
```bash
sudo systemctl status certbot.timer
sudo journalctl -u certbot.service
```

### DNS Propagation
- Check: `nslookup arvind.codes` (should resolve both A + AAAA)
- Global: `mxtoolbox.com` or `whatsmydns.net`
