# JWT system

JWT between frontend and backend

Jtoken作为前后端认证的令牌

服务端保留私钥，注册时client发送post请求，server使用私钥加密形成token返回给client；client发送其他请求时需要带上token，server通过检验token内容来确认身份

结构：`xxxxx.yyyyy.zzzzz` => Header.Payload.Signature

## 前后端的交互

`.env`文件表示的端口以及url的base，以及database的url

`cors`代表的跨域请求，根url设置为`localhost`

# base64 和 UriEncode

Uri是URL的超集，如果某些内容不符合浏览器解析URL的格式，可以进行URIEncode让其可以被浏览器解析，URLEncode类似

JSON序列化的字符串也可以URIEncode

base64编码方法的特点就是末尾的等于号`=`，URIEncode为`%3D`

## base64算法

1. 将原始数据每三个字节作为一组，每个字节是8个bit，所以一共是 24 个 bit
2. 将 24 个 bit 分为四组，每组 6 个 bit
3. 在每组前面加补 00，将其补全成四组8个bit
4. 到此步，原生数据的3个字节已经变成4个字节了，增大了将近30%
5. 一组只有一个字节，补两个等号，`M -> TQ==`，对于加密串`EncryptStr`，别名`Estr`；`Estr[1]`的构造视为填充0，`Estr[2]`和`Estr[3]`视为填充padding
6. 一组只有两个字节，补一个等号，`MA -> TUE=`，对于加密串`EncryptStr`，别名`Estr`；`Estr[2]`的构造视为填充0，`Estr[3]`视为填充padding


## 嗅觉

json字符串的base64编码总是以`ey...`开头

## 3DES加密算法

3DES加密是DES加密的加强版

一个24字节的密钥分为三个8字节的密钥，分别进行一次加密，解密，加密；因为一二次密钥不同所以不会抵消；CBC模式会使用一个8字节的密钥2进行异或加密

如果三个密钥相同算法退化为DES算法

base64编码结尾的`=`进行URIEncode会变成`%3D`，如果结尾有这项那么联想到`URIDecode`和`base64decode`
