# Self-Signed Certificates for Proxmox
> Ubuntu 22.04

## Certificate Authority

Create Working Folders
```
mkdir selfsigned-certs && cd selfsigned-certs
```

Create RSA
```
openssl genrsa -aes256 -out ca-key.pem 4096
```

Create public CA Cert  
```
openssl req -new -x509 -sha256 -days 3650 -key ca-key.pem -out ca.pem
```

Show CA Cert Output
```
openssl x509 -in ca.pem -text
```

## Generate Certificate

Create RSA key
```
openssl genrsa -out cert-key.pem 4096
```

Create Certificate Signing Request (CSR)
```
openssl req -new -sha256 -subj "/CN=home.local" -key cert-key.pem -out cert.csr
```

Create extfile for a certificate subject alternative name
```
echo "subjectAltName=DNS:pve.local,IP:192.168.1.88" >> extfile.cnf
```
Create certificate
```
openssl x509 -req -sha256 -days 3650 -in cert.csr -CA ca.pem -CAkey ca-key.pem -out cert.pem -extfile extfile.cnf -CAcreateserial
```

## Add Certificate to Proxmox

Combine cert.pem & ca.pem
```
cat cert.pem > certca.pem
cat ca.pem >> certca.pem
```

Add cert-key.pm & certca.pem to Proxmox
> pve -> Certificates -> Upload Custom Certificate \
```cat cert-key.pem```
> Private Key (Optional): \
```cat certca.pem```
> Certificate Chain:

## Add Self-Signed Certificate to Ubuntu

Create folder for Self-Signed Certificates
```
sudo mkdir /usr/local/share/ca-certificates/extra
```

Convert .pem to .crt
```
openssl x509 -in ca.pem -inform PEM -out ca.crt
```

Copy ca.crt to folder
```
sudo cp ca.crt /usr/local/share/ca-certificates/extra/ca.crt
```

Update CA store
```
sudo update-ca-certificates
```
