# Dev Note - Nginx

## Optimization

### Improve Latency

#### Enable HTTP2

```bash
# Change
listen 443 ssl;
# TO
listen 443 ssl http2;

# Check network package header's protocol property is "h2" or curl response header include HTTP/2.
```

#### Cipher priority

No need to modify if using Let's Encrypt.

/etc/letsencrypt/options-ssl-nginx.conf

```bash
# Manual chiper
ssl_prefer_server_ciphers on;  # prefer a list of ciphers to prevent old and slow ciphers
ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
```

#### OCSP Stapling

Let's Encrypt OCSP is slow in China.

After enable OCSP Stapling, Nginx will not response OCSP immediately, wait few minutes.

```bash
ssl_stapling on;
ssl_stapling_verify on;
ssl_trusted_certificate /path/to/full_chain.pem;
```

Test

```bash
openssl s_client -connect yourdomain.com:443 -servername yourdomain.com -status -tlsextdebug < /dev/null 2>&1 | grep -i "OCSP response"

# Example
openssl s_client -connect bitdove.elecdove.com:443 -servername bitdove.elecdove.com -status -tlsextdebug < /dev/null 2>&1 | grep -i "OCSP response"

# Success
OCSP response: 
OCSP Response Data:
    OCSP Response Status: successful (0x0)
    Response Type: Basic OCSP Response

# Failed
OCSP response: no response sent
```

#### Tunning ssl_buffer_size

```bash
# Default = 16k
# Big file = 16k
# Web or REST API = 4k
ssl_buffer_size 4k;
```

#### Enable SSL Session cache

No need to modify if using Let's Encrypt.

/etc/letsencrypt/options-ssl-nginx.conf

1MB = 4000 connections

```bash
# Enable SSL cache to speed up for return visitors
ssl_session_cache shared:SSL:50m; # speed up first time.
ssl_session_timeout 4h;
```

