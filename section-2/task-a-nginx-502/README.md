# Section 2 - Task A: 502 Bad Gateway (Nginx + Node.js)

## Problem summary

The reverse proxy returns **502 Bad Gateway** because Nginx is forwarding traffic to the wrong backend port.

## Given configuration

The assignment shows:

- the Node.js application listens on port `3000`
- Nginx is configured with:

```nginx
proxy_pass http://web:8080;
```

Because the backend application is not listening on port 8080, Nginx cannot connect to it.

### Root cause

The root cause is a port mismatch between the Nginx upstream configuration and the actual Node.js application port.

- ```server.js``` listens on port ```3000```

- ```nginx.conf``` proxies requests to ```web:8080```

As a result, Nginx tries to reach a service endpoint that does not exist.

### Fix

Update the Nginx configuration to use the correct backend port:
```proxy_pass http://web:3000;```
### Corrected configuration example
```
server {
    listen 80;

    location / {
        proxy_pass http://web:3000;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
### How I would verify the fix
#### 1. Check container status:
```docker compose ps```

#### 2. Check backend logs:
```docker compose logs web```

#### 3. Check Nginx logs:
```docker compose logs nginx```

#### 4. Test backend connectivity from inside the Nginx container:

```wget -qO- http://web:3000```

#### 5. Verify the application through the reverse proxy:
```curl http://localhost```

### Prevention in production

1. To prevent this problem in production, I would:

2. Centralize port configuration using environment variables or shared configuration.

3. Add health checks for the backend service.

4. Add deployment smoke tests to verify proxy-to-backend communication.

5. Monitor Nginx 5xx errors and backend availability.

6. Use config reviews and CI validation for reverse proxy changes.

### Short answer

Cause: Nginx proxies to port ```8080```, but the Node.js app listens on ```3000```.
Fix: Change ```proxy_pass``` to ```http://web:3000;```
Prevention: Use consistent config management, health checks, testing, and monitoring.