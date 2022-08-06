

Generate CA Key and Certificate
```sh
openssl req -newkey rsa:2048 -days 100 -nodes -x509 -keyout ca-key.pem -out ca-cert.pem
```

Generate Receiver Key and CSR
```sh
openssl req -newkey rsa:2048 -nodes -keyout receiver-key.pem -out receiver-req.pem
```

Sign Receiver Certificate
```sh
openssl x509 -req -in receiver-req.pem -days 60 -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out receiver-cert.pem -extfile receiver.conf
```

Now, configure receiver cert and key in Collector config
```
...
        tls:
          cert_file: /etc/receiver-cert.pem
          key_file: /etc/receiver-key.pem
...
```

Then use the CA cert for exporter using the IP used in receiver.conf

```
OTEL_EXPORTER_OTLP_ENDPOINT=https://127.0.0.1:4317
OTEL_EXPORTER_OTLP_CERTIFICATE=/etc/ca-cert.pem
```
