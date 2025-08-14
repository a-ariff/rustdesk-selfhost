# Cloudflare DNS Records

This document contains the required DNS records for the RustDesk self-hosted server deployment.

## Required DNS Records

For `ariff.aglobaltec.com` domain:

### A Records
```
Type: A
Name: ariff.aglobaltec.com (or @)
Value: YOUR_SERVER_IP_ADDRESS
TTL: Auto
Proxy Status: Proxied (Orange cloud)
```

```
Type: A
Name: *.ariff.aglobaltec.com
Value: YOUR_SERVER_IP_ADDRESS
TTL: Auto
Proxy Status: Proxied (Orange cloud)
```

### Additional Records (Optional)

#### For additional subdomains:
```
Type: A
Name: rustdesk.ariff.aglobaltec.com
Value: YOUR_SERVER_IP_ADDRESS
TTL: Auto
Proxy Status: Proxied (Orange cloud)
```

## Cloudflare Settings

### SSL/TLS Settings
- **SSL/TLS encryption mode**: Full (strict)
- **Always Use HTTPS**: Enabled
- **HTTP Strict Transport Security (HSTS)**: Enabled
- **Minimum TLS Version**: 1.2

### Security Settings
- **Security Level**: Medium
- **Challenge Passage**: 30 minutes
- **Browser Integrity Check**: Enabled

### Speed Settings
- **Auto Minify**: HTML, CSS, JS enabled
- **Brotli**: Enabled
- **Rocket Loader**: Disabled (can interfere with websockets)

### Network Settings
- **HTTP/2**: Enabled
- **HTTP/3 (with QUIC)**: Enabled
- **0-RTT Connection Resumption**: Enabled
- **IPv6 Compatibility**: Enabled

## Port Configuration

Cloudflare proxies the following ports for HTTP/HTTPS traffic:
- **HTTP**: 80, 8080, 8880
- **HTTPS**: 443, 2053, 2083, 2087, 2096, 8443

### RustDesk Specific Ports
For direct TCP/UDP connections (not proxied):
- **TCP 21116**: ID Server (set proxy status to DNS Only/Grey cloud)
- **UDP 21117**: Relay Server (set proxy status to DNS Only/Grey cloud)

### Configuration Example
```
# For web interface (proxied)
Type: A
Name: ariff.aglobaltec.com
Value: YOUR_SERVER_IP
Proxy Status: Proxied

# For RustDesk direct connections (DNS only)
Type: A
Name: rustdesk-direct.ariff.aglobaltec.com
Value: YOUR_SERVER_IP
Proxy Status: DNS Only
```

## Environment Variables

Update your `.env` file with:
```env
CLOUDFLARE_EMAIL=your-cloudflare-email@example.com
CLOUDFLARE_API_KEY=your-cloudflare-api-key
CLOUDFLARE_ZONE_ID=your-zone-id
```

## Testing

1. Test web interface: `https://ariff.aglobaltec.com`
2. Test RustDesk connection: Use `ariff.aglobaltec.com:21116` in RustDesk client
3. Verify SSL: `curl -I https://ariff.aglobaltec.com`
