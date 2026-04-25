---
source_url: file:///Users/yin.yan/Documents/yanyin1986.github.io/开发笔记/iOS App备案，获取证书公钥.md
ingested: 2026-04-25
sha256: f0e5b9afa396ef416773d54d718302155a171d7a249df410a2cdc22424d71c1f
---


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