## PHP Built-in Server

bash

```bash
# HTTP (default port 8000)
php -S localhost:8000

# HTTP (custom port)
php -S localhost:8080

# Specify document root
php -S localhost:8000 -t /path/to/your/project

# Listen on all interfaces
php -S 0.0.0.0:8000
```

**For HTTPS with PHP:** You need to set up a reverse proxy (like Nginx) or use a tunnel service like ngrok, as PHP's built-in server doesn't support HTTPS directly.

## Python Built-in Server

bash

```bash
# Python 3 - HTTP
python -m http.server 8000

# Python 3 - custom port
python -m http.server 8080

# Python 2 (legacy)
python -m SimpleHTTPServer 8000
```

**For HTTPS with Python:**

bash

```bash
# Create a simple HTTPS server (Python 3)
python -m http.server 8000 --bind 0.0.0.0

# Or use this script for HTTPS:
```

python

```python
#import http.server
import ssl
import socket
import os

server_address = ('0.0.0.0', 4443)
httpd = http.server.HTTPServer(server_address, http.server.SimpleHTTPRequestHandler)

context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain(certfile='./cert.pem', keyfile='./key.pem')

httpd.socket = context.wrap_socket(httpd.socket, server_side=True)

# Get real network IP
def get_local_ip():
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        s.connect(("8.8.8.8", 80))
        ip = s.getsockname()[0]
        s.close()
        return ip
    except:
        return "Unable to determine"

local_ip = get_local_ip()

print("=" * 50)
print("HTTPS Server Running!")
print("=" * 50)
print(f"Local access:    https://localhost:4443")
print(f"Network access:  https://{local_ip}:4443")
print(f"Serving from:    {os.getcwd()}")
print("=" * 50)
print("Press Ctrl+C to stop")
print("=" * 50)

httpd.serve_forever()```

Generate self-signed certificate:

bash

```bash
openssl req -new -x509 -keyout key.pem -out cert.pem -days 365 -nodes
```

## FTP Server (Python)

# Install from Kali repos
sudo apt install python3-pyftpdlib

# Then run
python3 -m pyftpdlib -p 2121 -w
- **Python**: [http://localhost:8000](http://localhost:8000)
- **FTP**: ftp://localhost:21

These are lightweight development servers