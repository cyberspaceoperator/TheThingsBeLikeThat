# 443 - HTTPS

## Generating a client certificate

First you must already have an existing CA key to craft a client certificate. If you could not gain access to a CA cert or craft one via the web server, than the next steps will be useless.

```
openssl genrsa -out client.key 4096
openssl req -new -key client.key -out client.req
openssl x509 -req -​ in​ client.req -CA lacasadepapelhtb.crt -CAkey ca.key
-set_serial 101 -extensions client -days 365 -outform PEM -out client.cer
openssl pkcs12 -export -inkey client.key -in client.cer -out client.p12
rm client.key client.req client.cer
```

{% hint style="info" %}
&#x20;Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

