Build with server option
```
meteor build ../mychannel-build --server http://192.168.0.100:3000
```
Generate key
```
keytool -genkey -alias mychannel -keyalg RSA -keysize 2048 -validity 10000
```
Sign with the key
```
jarsigner -digestalg SHA1 release-unsigned.apk mychannel
```

Optional zip align
```
path_to/zipalign 4 release-unsigned.apk mychannel-release-signed.apk
```
