---
icon: key
---

# 令牌创建

令牌使用AES-128-CBC算法进行加密以下是几个主流语言的方法.如果需要其他的可以通过技术群取得联系

{% tabs %}
{% tab title="Node.js" %}
{% code lineNumbers="true" fullWidth="true" %}
```javascript
const crypto = require('crypto');
const { Buffer } = require('buffer');

function pkcs7Padding(data, blockSize) {
    const padding = blockSize - (data.length % blockSize);
    const padtext = Buffer.alloc(padding, padding);
    return Buffer.concat([data, padtext]);
}

function pkcs7Unpadding(data) {
    const padding = data[data.length - 1];
    if (padding > data.length || padding > 16) {
        throw new Error('Invalid padding');
    }
    for (let i = 0; i < padding; i++) {
        if (data[data.length - padding + i] !== padding) {
            throw new Error('Invalid padding');
        }
    }
    return data.slice(0, data.length - padding);
}

class Agents {
    // Encrypt 使用 AES-128-CBC 加密数据
    encrypt(data, key, iv) {
        const block = crypto.createCipheriv('aes-128-cbc', key, iv);
        let ciphertext = block.update(data, 'utf8', 'base64');
        ciphertext += block.final('base64');
        return ciphertext;
    }

    // Decrypt 使用 AES-128-CBC 解密数据
    decrypt(data, key, iv) {
        const decodedData = Buffer.from(data, 'base64');
        const decipher = crypto.createDecipheriv('aes-128-cbc', key, iv);
        let plaintext = decipher.update(decodedData);
        plaintext = Buffer.concat([plaintext, decipher.final()]);
        return pkcs7Unpadding(plaintext).toString('utf8');
    }
}

const data = {
    account: "1723205701015@jjserver.com",
    uid: "110",
    time: Date.now().toString(),
    username: "测试1",
    wallet: 100
};

const key = Buffer.from('2fdc826ed2d966e2f3fca09516446f75', 'utf8');
const iv = Buffer.from('xdm2fl24e0m3yo9c', 'utf8');

const agent = new Agents();
const jsonData = JSON.stringify(data);

const encryptedData = agent.encrypt(jsonData, key, iv);
console.log(`Encrypted Data: ${encryptedData}`);

const decryptedData = agent.decrypt(encryptedData, key, iv);
console.log(`Decrypted Data: ${decryptedData}`);

```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code lineNumbers="true" %}
```python
import base64
import json
import time
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad

class Agents:
    def __init__(self):
        self.block_size = AES.block_size

    # Encrypt 使用 AES-128-CBC 加密数据
    def encrypt(self, data, key, iv):
        cipher = AES.new(key, AES.MODE_CBC, iv)
        padded_data = pad(data, self.block_size)
        ciphertext = cipher.encrypt(padded_data)
        return base64.b64encode(ciphertext).decode('utf-8')

    # Decrypt 使用 AES-128-CBC 解密数据
    def decrypt(self, data, key, iv):
        decoded_data = base64.b64decode(data)
        cipher = AES.new(key, AES.MODE_CBC, iv)
        plaintext = cipher.decrypt(decoded_data)
        return unpad(plaintext, self.block_size).decode('utf-8')

if __name__ == "__main__":
    data = {
        "account": "1723205701015@jjserver.com",
        "uid": "110",
        "time": str(int(time.time() * 1000)),
        "username": "测试1",
        "wallet": 100.0
    }

    key = b"2fdc826ed2d966e2f3fca09516446f75"
    iv = b"xdm2fl24e0m3yo9c"

    agent = Agents()
    json_data = json.dumps(data).encode('utf-8')

    encrypted_data = agent.encrypt(json_data, key, iv)
    print(f"Encrypted Data: {encrypted_data}")

    decrypted_data = agent.decrypt(encrypted_data, key, iv)
    print(f"Decrypted Data: {decrypted_data}")

```
{% endcode %}
{% endtab %}

{% tab title="PHP" %}
{% code lineNumbers="true" %}
```php
<?php
class Agents {
    private $blockSize = 16;

    // Encrypt 使用 AES-128-CBC 加密数据
    public function encrypt($data, $key, $iv) {
        $paddedData = $this->pkcs7Padding($data, $this->blockSize);
        $ciphertext = openssl_encrypt($paddedData, 'AES-128-CBC', $key, OPENSSL_RAW_DATA, $iv);
        return base64_encode($ciphertext);
    }

    // Decrypt 使用 AES-128-CBC 解密数据
    public function decrypt($data, $key, $iv) {
        $decodedData = base64_decode($data);
        $plaintext = openssl_decrypt($decodedData, 'AES-128-CBC', $key, OPENSSL_RAW_DATA, $iv);
        return $this->pkcs7Unpadding($plaintext, $this->blockSize);
    }

    // pkcs7Padding 使用 PKCS#7 填充数据
    private function pkcs7Padding($data, $blockSize) {
        $padding = $blockSize - (strlen($data) % $blockSize);
        $padtext = str_repeat(chr($padding), $padding);
        return $data . $padtext;
    }

    // pkcs7Unpadding 去除 PKCS#7 填充
    private function pkcs7Unpadding($data, $blockSize) {
        $length = strlen($data);
        if ($length == 0) {
            throw new Exception("Input data is empty");
        }

        $padding = ord($data[$length - 1]);
        if ($padding > $length || $padding > $blockSize) {
            throw new Exception("Invalid padding");
        }

        for ($i = 0; $i < $padding; $i++) {
            if (ord($data[$length - $padding + $i]) != $padding) {
                throw new Exception("Invalid padding");
            }
        }

        return substr($data, 0, $length - $padding);
    }
}

$data = [
    'account' => '1723205701015@jjserver.com',
    'uid' => '110',
    'time' => time(),
    'username' => '测试1',
    'wallet' => 100
];

$key = '2fdc826ed2d966e2f3fca09516446f75';
$iv = 'xdm2fl24e0m3yo9c';

$agent = new Agents();
$jsonData = json_encode($data);

$encryptedData = $agent->encrypt($jsonData, $key, $iv);
echo "Encrypted Data: " . $encryptedData . "\n";

$decryptedData = $agent->decrypt($encryptedData, $key, $iv);
echo "Decrypted Data: " . $decryptedData . "\n";
?>

```
{% endcode %}


{% endtab %}

{% tab title="Java" %}
{% code lineNumbers="true" %}
```java
import javax.crypto.Cipher;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.util.Base64;
import java.util.HashMap;
import java.util.Map;

public class Agents {
    private static final int BLOCK_SIZE = 16;

    // Encrypt 使用 AES-128-CBC 加密数据
    public String encrypt(String data, String key, String iv) throws Exception {
        byte[] paddedData = pkcs7Padding(data.getBytes(StandardCharsets.UTF_8), BLOCK_SIZE);
        SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(StandardCharsets.UTF_8), "AES");
        IvParameterSpec ivSpec = new IvParameterSpec(iv.getBytes(StandardCharsets.UTF_8));
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        cipher.init(Cipher.ENCRYPT_MODE, secretKey, ivSpec);
        byte[] encryptedData = cipher.doFinal(paddedData);
        return Base64.getEncoder().encodeToString(encryptedData);
    }

    // Decrypt 使用 AES-128-CBC 解密数据
    public String decrypt(String data, String key, String iv) throws Exception {
        byte[] decodedData = Base64.getDecoder().decode(data);
        SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(StandardCharsets.UTF_8), "AES");
        IvParameterSpec ivSpec = new IvParameterSpec(iv.getBytes(StandardCharsets.UTF_8));
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        cipher.init(Cipher.DECRYPT_MODE, secretKey, ivSpec);
        byte[] decryptedData = cipher.doFinal(decodedData);
        return new String(pkcs7Unpadding(decryptedData, BLOCK_SIZE), StandardCharsets.UTF_8);
    }

    // pkcs7Padding 使用 PKCS#7 填充数据
    private byte[] pkcs7Padding(byte[] data, int blockSize) {
        int padding = blockSize - (data.length % blockSize);
        byte[] padtext = new byte[padding];
        for (int i = 0; i < padding; i++) {
            padtext[i] = (byte) padding;
        }
        byte[] paddedData = new byte[data.length + padding];
        System.arraycopy(data, 0, paddedData, 0, data.length);
        System.arraycopy(padtext, 0, paddedData, data.length, padding);
        return paddedData;
    }

    // pkcs7Unpadding 去除 PKCS#7 填充
    private byte[] pkcs7Unpadding(byte[] data, int blockSize) {
        int length = data.length;
        if (length == 0) {
            throw new IllegalArgumentException("Input data is empty");
        }

        int padding = data[length - 1];
        if (padding > length || padding > blockSize) {
            throw new IllegalArgumentException("Invalid padding");
        }

        for (int i = 0; i < padding; i++) {
            if (data[length - padding + i] != padding) {
                throw new IllegalArgumentException("Invalid padding");
            }
        }

        return Arrays.copyOfRange(data, 0, length - padding);
    }

    public static void main(String[] args) throws Exception {
        Map<String, Object> data = new HashMap<>();
        data.put("account", "1723205701015@jjserver.com");
        data.put("uid", "110");
        data.put("time", System.currentTimeMillis());
        data.put("username", "测试1");
        data.put("wallet", 100.0);

        String key = "2fdc826ed2d966e2f3fca09516446f75";
        String iv = "xdm2fl24e0m3yo9c";

        Agents agent = new Agents();
        String jsonData = new com.google.gson.Gson().toJson(data);

        String encryptedData = agent.encrypt(jsonData, key, iv);
        System.out.println("Encrypted Data: " + encryptedData);

        String decryptedData = agent.decrypt(encryptedData, key, iv);
        System.out.println("Decrypted Data: " + decryptedData);
    }
}

```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code lineNumbers="true" %}
```go
package main

import (
	"bytes"
	"crypto/aes"
	"crypto/cipher"
	"encoding/base64"
	"encoding/json"
	"errors"
	"fmt"
	"time"
)

type Data struct {
	Account  string  `json:"account"`
	UID      string  `json:"uid"`
	Time     string  `json:"time"`
	Username string  `json:"username"`
	Wallet   float64 `json:"wallet"`
}
type Agents struct {
}

// Encrypt 使用 AES-128-CBC 加密数据
func (a *Agents) Encrypt(data, key, iv []byte) ([]byte, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return nil, err
	}

	// 使用 CBC 模式加密
	mode := cipher.NewCBCEncrypter(block, iv)

	// 填充数据
	paddedData := pkcs7Padding(data)

	// 创建缓冲区
	ciphertext := make([]byte, len(paddedData))
	mode.CryptBlocks(ciphertext, paddedData)

	return []byte(base64.StdEncoding.EncodeToString(ciphertext)), nil
}

// pkcs7Padding 使用 PKCS#7 填充数据
func pkcs7Padding(data []byte) []byte {
	padding := aes.BlockSize - len(data)%aes.BlockSize
	padtext := bytes.Repeat([]byte{byte(padding)}, padding)
	return append(data, padtext...)
}

// Decrypt 使用 AES-128-CBC 解密数据
func (a *Agents) Decrypt(data []byte, key, iv []byte) ([]byte, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return nil, err
	}
	// 解码数据
	data, err = base64.StdEncoding.DecodeString(string(data))
	if err != nil {
		return nil, err
	}
	// 使用 CBC 模式解密
	mode := cipher.NewCBCDecrypter(block, iv)

	// 创建缓冲区
	plaintext := make([]byte, len(data))
	mode.CryptBlocks(plaintext, data)

	// 去除填充
	unpaddedData, err := pkcs7Unpadding(plaintext)
	if err != nil {
		return nil, err
	}

	return unpaddedData, nil
}

// pkcs7Unpadding 去除 PKCS#7 填充
func pkcs7Unpadding(data []byte) ([]byte, error) {
	length := len(data)
	if length == 0 {
		return nil, errors.New("input data is empty")
	}

	padding := int(data[length-1])
	if padding > length || padding > aes.BlockSize {
		return nil, errors.New("invalid padding")
	}

	for i := 0; i < padding; i++ {
		if data[length-padding+i] != byte(padding) {
			return nil, errors.New("invalid padding")
		}
	}

	return data[:length-padding], nil
}

func main() {
	Time := time.Now().UnixNano()
	fmt.Printf("time:%d\n", Time)
	data := Data{
		Account:  "1723205701015@jjserver.com",
		UID:      "110",
		Time:     fmt.Sprintf("%d", Time),
		Username: "测试1",
		Wallet:   100,
	}
	Key := "2fdc826ed2d966e2f3fca09516446f75"
	IV := "xdm2fl24e0m3yo9c"
	jsonData, _ := json.Marshal(data)
	var agent Agents
	encryptedData, _ := agent.Encrypt(jsonData, []byte(Key), []byte(IV))
	fmt.Printf("Encrypted Data: %s\n", encryptedData)

	decryptedData, _ := agent.Decrypt(encryptedData, []byte(Key), []byte(IV))
	fmt.Printf("Decrypted Data: %s\n", string(decryptedData))
}

```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
以上数据输出内容为

Encrypted Data: LHgsTGbF+GdhOS4lQSn3p72mumuptCzGzOTN0sqTD9R7efiVwvgIP8jzN4TZICkg8b+PMNrtJNvkIgI+wqioKVTo5rZYOcv731ASJUYEVXH6KvnXR2NhFMmWVqxxXEUSmM4DgYpW4DWe2OKsKymYIjmY0s6MwRYF5DOiOigdobA=&#x20;

Decrypted Data: {"account":"1723205701015@jjserver.com","uid":"110","time":"1732443087776312600","username":"测试1","wallet":100}
{% endhint %}

## Data结构(<mark style="color:orange;">AES-128-CBC</mark>)

| Name     | Type   | Description                                      |
| -------- | ------ | ------------------------------------------------ |
| account  | string | 账号名<[见创建账号](../zhun-bei-gong-zuo/quickstart.md)> |
| uid      | string | 自己平台得用户ID                                        |
| time     | string | 秒时间戳                                             |
| username | string | 可以随意也可以保存成用户昵称,方便后期在后台查找用户                       |
| wallet   | float  | 当前平台用户钱包余额,这里如果没有用户会创建一个这个用户钱包,钱包余额以代理商为准        |
| url      | string | 查询钱包余额地址<[详细见](cha-xun-di-zhi.md)>               |

### 加密部分:

* 使用 AES-128-CBC 加密模式。&#x20;
* 数据进行 PKCS#7 填充
* 结果转换为 Base64 编码。

### 解密部分:

* 将 Base64 编码的数据解码为二进制数据
* 使用解密函数进行解密
* 去除PKCS#7 填充

### 数据结构:

* data 对象与 Go 代码中的 Data 结构体对应
* key 和 iv 需要转换为字节数组

### 时间戳:

* 获取当前时间的时间戳

把以上数据用`Bearer 拼接上在协议头中加入到`Authorization中

{% hint style="info" %}
<mark style="color:orange;">注意:以上方法请不要放在客户端或者网页中避免您得Key跟IV丢失,请妥善在自己服务器保存令牌,这个是当前用户的身份,服务器发送的回调也以这个令牌为依据判断是贵平台的哪个用户</mark>
{% endhint %}

