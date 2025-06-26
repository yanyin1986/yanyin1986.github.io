1. 创建新的 distribution 证书
2. 在本地的 keychain 里面导出证书为 `Certificates.p12`
3. 输入下面的命令，提取证书，注意如果 openssl 3

```shell
## openssl version < 3
openssl pkcs12 \
  -in Certificates.p12 \
  -clcerts \
  -nokeys \
  -out MyCertificate.crt

## openssl version > 3
openssl pkcs12 \
  -legacy \
  -in Certificates.p12 \
  -clcerts \
  -nokeys \
  -out MyCertificate.crt
```

4. 获取指纹信息
```shell
## MD5 
openssl x509 -noout -fingerprint -md5 -inform pem -in MyCertificate.crt

## SHA1
openssl x509 -noout -fingerprint -sha1 -inform pem -in MyCertificate.crt
```

5. 获取公钥
```shell
openssl x509 -pubkey -noout -inform pem -in MyCertificate.crt > PublicKey.pem
```