
---

# hikCentral登录(3.1前)
## 流程简述

### HTTP登录
1. `getSecurityCrypto`  [[HCP协议]]获取 `m_sessionId` 、`m_cryptoKey` 

2.  拿`m_cryptoKey` 对密码进行加密，`m_sessionId` 用于填充 `url`

3.  使用加密后的密码发起登录请求，获得`m_challenge`、`m_iterations`

4.  对 `m_pwd` 使用 `m_challenge`和`m_iterations`进行加密得到`m_aseKey`

### HTTPS登录

1.  在`url`中填入 `m_sessionId` (空的)

2.  登录报文中的密码直接填明文

3.  直接发起登录请求

## HTTP登录详细步骤
### `getSecurityCrypto`

 获取 `m_sessionId` 、`m_cryptoKey`         `"/ISAPI/Bumblebee/Platform/V0/Security/Crypto"`

### `clientLogin`

1. 在`url`中填入 `m_sessionId`

2. `base64`解码 `m_cryptoKey` 得到 `baseCode` 

3. 将解码后的密钥`baseCode`转换成内存中的格式 `m_rsa`

4. 用 `m_rsa` 对密码 `m_pwd` 进行加密得到加密后的 `rsaPwd`

5. 对加密后的密码 `rsaPwd` 再进行 `base64` 编码得到 `basePwd`

6. 登录接口的参数中密码字段填 `basePwd`

登录接口的返回内容会更新 `m_sessionId`、`m_challenge`、`m_iterations`

<img src="./image/hikcentral登录流程/Pasted image 20251030152424.png" width="70%" height="auto">

## HTTPS登录详细步骤
### `httpsClientLogin`

1. 在`url`中填入 `m_sessionId` (空的)

2. 登录接口的参数中密码字段填 `m_pwd`

3. 登录接口的返回内容会更新 `m_sessionId`、`m_challenge`、`m_iterations`
<img src="./image/hikcentral登录流程/Pasted image 20251030155531.png" width="70%" height="auto">


## 登录后得到`m_aseKey`

1. 将 `m_pwd` 和 `m_challenge` 拼接起来得到 `input`

2. 对 `input` 进行 `m_iterations` 次 `SHA256` 迭代计算，将二进制哈希值的每个字节转化为16进制形式

3. 迭代计算的结果放在 `m_aesKey` 中

`m_aesKey` 用于解密后面获取到加密的解码器密码、计算得到 `m_centralToken`、
做各种`AES`相关的加解密操作（仅在HTTP登录时，获取到的密码是密文，需要解密），包括:

- 各种取流`url`中的密码解密

- 回放时获取到的解码器密码的解密

<img src="./image/hikcentral登录流程/Pasted image 20251030155910.png" width="70%" height="auto">

## 登录后得到`AUTH`

### `getAuth`

`genToken` 得到 `m_centralToken` ，用到 `m_aesKey`

接口返回得到 `m_tvmsAuth`，后续 `url` 中的 `AUTH` 字段需要填


---

# hikCentral登录(3.1)

## 什么时候走新的登录流程

`HCP`平台从3.1开始，使用新的登录流程。因此键盘根据平台版本决定走新/旧登录流程。

但是获取平台版本的接口`getVersion`在`HCP`3.1 也做了修改：
~~登录前查询接口，返回成功，并==能获取到版本信息==。~~ --->   登录前查询接口，返回成功，但是==获取不到版本信息==。

因此，判断走新旧登录流程的逻辑如下：
```C
登录前调用getVersion;
if(获取到版本信息)      走新的登录流程
else                   走老的登录流程
```

## 流程简述

1. `getSecurityCryptoV1`获取：挑战串`Challenge` 、盐值`Salt` 、迭代轮次1`Iterations1`、迭代轮次2`Iterations2`

2.   `getCalcResults`计算：
	`base64`(`Sha256`(原始密码+Salt) 迭代轮次1) = 成果物1
	`base64`(`Sha256`(成果物1 +挑战串) 迭代轮次2) = 成果物2

3.  登录请求中的密码字段填中间成果物2、`url`中的`m_sessionId` 填 `Challenge` 
	发起登录请求，得到`m_sessionId`、`m_challenge`、`m_iterations`

4.  对 `成果物1` 使用 `m_challenge`和`m_iterations`进行加密得到`m_aseKey`


## HTTP/HTTPS登录详细步骤

### `getSecurityCryptoV1`

	获取:挑战串`Challenge` 、盐值`Salt` 、迭代轮次1`Iterations1`、迭代轮次2`Iterations2`    `"/ISAPI/Bumblebee/Platform/V1/Security/Crypto"`

### `getCalcResults`
	base64(Sha256(原始密码 + Salt) 迭代轮次1) = 成果物1
	base64(Sha256(成果物1 + 挑战串) 迭代轮次2) = 成果物2

<img src="./image/hikcentral登录流程/Pasted image 20251031150256.png" width="55%" height="auto">

### `clientLogin` / `httpsClientLogin` 

1. 在`url` 的 `m_sessionId` 中填入 `getSecurityCryptoV1` 获取到的 `Challenge` 

2. 登录接口的参数中密码字段填`getCalcResults` 算出的 成果物2

3. 登录接口的返回内容会更新 `m_sessionId`、`m_challenge`、`m_iterations`

<img src="./image/hikcentral登录流程/Pasted image 20251031150918.png" width="55%" height="auto">


## 登录后得到`m_aseKey`

1. 将  `getCalcResults`得到的`成果物1` 和 `m_challenge` 拼接起来得到 `input`

2. 对 `input` 进行 `m_iterations` 次 `SHA256` 迭代计算，将二进制哈希值的每个字节转化为16进制形式

3. 迭代计算的结果放在 `m_aesKey` 中

`m_aesKey` 用于解密后面获取到加密的解码器密码、计算得到 `m_centralToken`、
做各种`AES`相关的加解密操作（仅在HTTP登录时，获取到的密码是密文，需要解密），包括:

- 各种取流`url`中的密码解密

- 回放时获取到的解码器密码的解密

<img src="./image/hikcentral登录流程/Pasted image 20251031151431.png" width="70%" height="auto">

