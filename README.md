# Mikrotik scripts

### Automatic CF domain ip change

I dont want to pay my ISP extra for static ip address, so i wrote this script to change my CF domain accordingly.
It checks for IP changes on main pppoe out every 3 minutes, and updates CF record if something has changed.

First of all, you need to create your home domain (lets say `home.example.com`) via cloudflare console or API.

```bash
curl -X POST "https://api.cloudflare.com/client/v4/zones/MY_CF_ZONE_ID/dns_records" \
     -H "X-Auth-Email: MY_USER@MAIL.COM" \
     -H "X-Auth-Key: MY_API_KEY" \
     -H "Content-Type: application/json" \
     --data '{"type":"A","name":"home.example.com","content":"10.10.10.10","ttl":{},"priority":10,"proxied":false}'
```

From json response extract `result.id` value (for example, pipe output of curl to ` curl ... | jq -r ".result.id"`).

Script variables you need to change::

- domain : domain you want to update

- theinterface : your interface that is pointing to internet

- cfmail : cloudflare mail

- cfapikey : cloudflare api key

- cfzoneid : cloudflare zone id

- cfdomainid : cloudflare domain id shown above.

This script works by updating A record with PUT request using `fetch` microtik command.

