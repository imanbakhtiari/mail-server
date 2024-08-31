## Mail-Server installation using docker

### just install docker and docker-compose
### then just create a docker-compose.yml file and paste this configuration in it

```bash
version: '3'
services:
  posteio:
    image: analogic/poste.io
    ports:
      - "25:25"
      - "80:80"
      - "443:443"
      - "110:110"
      - "143:143"
      - "465:465"
      - "587:587"
      - "993:993"
      - "995:995"
    environment:
      - TZ=Europe/Prague
    volumes:
      - /opt/mail/data:/data
    restart: always
```
### And then run docker compose up

```bash
docker-compose up -d 
```

## 1- PTR record

#### The first thing you need to know is what domain you are going to use for your mail server. I strongly recommend mail.your-domain.com as it looks like a proper name for an email server, not something like ip-87-45.isp.com which will most likely end up in the spam folder. You will probably not be able to change the PTR record (known as reverse DNS) yourself, so contact your server provider to change the PTR record of your server's IP address to mail.your-domain.com.

## 2- A and CNAME records

```
mail.your-domain.com A → 1.2.3.4 (your ip)
```

## 3- MX for your domain
```
your-domain.com MX mail.your-domain.com
```
### example:
```
@ IN MX "mail.basa.ir."
```
## 4- SPF record:
```
your-domain.com. IN TXT "v=spf1 mx ~all"
```

## 5- DKIM record 
#### (this is example only, copy one from your administration!):

```
_s20160910378._domainkey.your-domain.com IN TXT "k=rsa;p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA0FvkMuwN46vvtQCC1JZz7XzRE+l+Lf8/5XUKwWJXOcE7dJoZBbOE0Gz85phZ2q+y4l8D7t/hXDz9q+6/KVQDgJ9muaxSM/uS+KG0ds0QLEiV0GYCVu+ZZQSNPBPjOwlDvo3LraW00lMpd5dUj+xpr07ShfIoULhi7/7t76n5GZMse9yBa4hIhxSG/wCAB4D6IWYBURz9Pc75IDPDTlImr3TP/82YrsULY70CHaPHA1+j1VPA5lE+tnmeqxJW6P537xSutDppv8BZg4nlF3ojg2k6LB/cq15C4QRPAMs77pRA4GVnys1LEJ3JDvV3/csOCZ49oC4m44/TnWXk057OAwIDAQAB"
```

#### Public part of DKIM. You must set the selector (subdomain part) and TXT record exactly the same or it will not work.


### 6- DMARC record:
```
_dmarc.our-domain.com. IN TXT "v=DMARC1; p=none; rua=mailto:dmarc-reports@our-domain.com"
```

## example:
```
_dmarc.domain.tld IN TXT "v=DMARC1; p=none; rua=mailto:admin@domail.tld"
```
#### DMARC is not required, but it is a very helpful method of identifying delivery issues. An explanation of DMARC options can be found at https://poste.io/dmarc?gmail.com


