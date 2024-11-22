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
const { JSON } = require('json-bigint');

class Data {
    constructor(account, uid, time, username, wallet) {
        this.account = account;
        this.uid = uid;
        this.time = time;
        this.username = username;
        this.wallet = wallet;
    }
}

class Agents {
    // Encrypt 使用 AES-128-CBC 加密数据
    encrypt(data, key, iv) {
        const block = crypto.createCipheriv('aes-128-cbc', Buffer.from(key), Buffer.from(iv));
        
        // 填充数据
        const paddedData = this.pkcs7Padding(data);
        
        // 加密数据
        let encrypted = block.update(paddedData, 'binary', 'base64');
        encrypted += block.final('base64');
        
        return encrypted;
    }

    // pkcs7Padding 使用 PKCS#7 填充数据
    pkcs7Padding(data) {
        const blockSize = 16;
        const padding = blockSize - (data.length % blockSize);
        const padtext = Buffer.alloc(padding, padding);
        return Buffer.concat([Buffer.from(data), padtext]);
    }
}

function main() {
    const data = new Data(
        "1723205701015@jjserver.com",
        "110",
        "1732281558618904700",
        "测试1",
        100
    );
    
    const Key = "2fdc826ed2d966e2f3fca09516446f75";
    const IV = "xdm2fl24e0m3yo9c";
    
    const jsonData = JSON.stringify(data);
    const agent = new Agents();
    const encryptedData = agent.encrypt(jsonData, Key, IV);
    
    console.log("Encrypted Data:", encryptedData);
}

main();

```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code lineNumbers="true" %}
```python
import json
import base64
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend
import time

class Data:
    def __init__(self, account, uid, time, username, wallet):
        self.account = account
        self.uid = uid
        self.time = time
        self.username = username
        self.wallet = wallet

class Agents:
    @staticmethod
    def encrypt(data, key, iv):
        backend = default_backend()
        cipher = Cipher(algorithms.AES(key), modes.CBC(iv), backend=backend)
        encryptor = cipher.encryptor()

        # 填充数据
        padded_data = Agents.pkcs7_padding(data)

        # 加密数据
        ciphertext = encryptor.update(padded_data) + encryptor.finalize()

        return base64.b64encode(ciphertext).decode('utf-8')

    @staticmethod
    def pkcs7_padding(data):
        block_size = algorithms.AES.block_size // 8
        padding = block_size - (len(data) % block_size)
        padtext = bytes([padding] * padding)
        return data + padtext

def main():
    current_time = int(time.time() * 1e9)
    print(f"time: {current_time}")

    data = Data(
        account="1723205701015@jjserver.com",
        uid="110",
        time=str(current_time),
        username="测试1",
        wallet=100
    )

    key = b"2fdc826ed2d966e2f3fca09516446f75"
    iv = b"xdm2fl24e0m3yo9c"

    json_data = json.dumps(data.__dict__).encode('utf-8')
    agent = Agents()
    encrypted_data = agent.encrypt(json_data, key, iv)
    print(f"Encrypted Data: {encrypted_data}")

if __name__ == "__main__":
    main()

```
{% endcode %}
{% endtab %}

{% tab title="PHP" %}
{% code lineNumbers="true" %}
```php
<?php

class Data {
    public $account;
    public $uid;
    public $time;
    public $username;
    public $wallet;

    public function __construct($account, $uid, $time, $username, $wallet) {
        $this->account = $account;
        $this->uid = $uid;
        $this->time = $time;
        $this->username = $username;
        $this->wallet = $wallet;
    }
}

class Agents {
    // Encrypt 使用 AES-128-CBC 加密数据
    public function encrypt($data, $key, $iv) {
        // 填充数据
        $paddedData = $this->pkcs7Padding($data);

        // 加密数据
        $ciphertext = openssl_encrypt($paddedData, 'AES-128-CBC', $key, OPENSSL_RAW_DATA, $iv);

        return base64_encode($ciphertext);
    }

    // pkcs7Padding 使用 PKCS#7 填充数据
    private function pkcs7Padding($data) {
        $blockSize = 16;
        $padding = $blockSize - (strlen($data) % $blockSize);
        $padtext = str_repeat(chr($padding), $padding);
        return $data . $padtext;
    }
}

function main() {
    $time = microtime(true) * 1000000; // 获取当前时间（纳秒）
    echo "time: " . $time . "\n";

    $data = new Data(
        "1723205701015@jjserver.com",
        "110",
        (string)$time,
        "测试1",
        100
    );

    $key = "2fdc826ed2d966e2f3fca09516446f75";
    $iv = "xdm2fl24e0m3yo9c";

    $jsonData = json_encode((array)$data);
    $agent = new Agents();
    $encryptedData = $agent->encrypt($jsonData, $key, $iv);
    echo "Encrypted Data: " . $encryptedData . "\n";
}

main();
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

public class Main {

    static class Data {
        String account;
        String uid;
        String time;
        String username;
        double wallet;

        public Data(String account, String uid, String time, String username, double wallet) {
            this.account = account;
            this.uid = uid;
            this.time = time;
            this.username = username;
            this.wallet = wallet;
        }

        public Map<String, Object> toMap() {
            Map<String, Object> map = new HashMap<>();
            map.put("account", account);
            map.put("uid", uid);
            map.put("time", time);
            map.put("username", username);
            map.put("wallet", wallet);
            return map;
        }
    }

    static class Agents {
        // Encrypt 使用 AES-128-CBC 加密数据
        public String encrypt(byte[] data, byte[] key, byte[] iv) throws Exception {
            SecretKeySpec secretKey = new SecretKeySpec(key, "AES");
            IvParameterSpec ivSpec = new IvParameterSpec(iv);

            Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
            cipher.init(Cipher.ENCRYPT_MODE, secretKey, ivSpec);

            byte[] encryptedData = cipher.doFinal(data);
            return Base64.getEncoder().encodeToString(encryptedData);
        }

        // pkcs7Padding 使用 PKCS#7 填充数据
        public byte[] pkcs7Padding(byte[] data) {
            int blockSize = 16;
            int padding = blockSize - (data.length % blockSize);
            byte[] paddedData = new byte[data.length + padding];
            System.arraycopy(data, 0, paddedData, 0, data.length);
            for (int i = data.length; i < paddedData.length; i++) {
                paddedData[i] = (byte) padding;
            }
            return paddedData;
        }
    }

    public static void main(String[] args) throws Exception {
        long time = System.nanoTime();
        System.out.println("time: " + time);

        Data data = new Data(
                "1723205701015@jjserver.com",
                "110",
                String.valueOf(time),
                "测试1",
                100
        );

        String keyStr = "2fdc826ed2d966e2f3fca09516446f75";
        String ivStr = "xdm2fl24e0m3yo9c";

        byte[] key = keyStr.getBytes(StandardCharsets.UTF_8);
        byte[] iv = ivStr.getBytes(StandardCharsets.UTF_8);

        Map<String, Object> dataMap = data.toMap();
        String jsonData = new com.google.gson.Gson().toJson(dataMap);

        Agents agent = new Agents();
        byte[] paddedData = agent.pkcs7Padding(jsonData.getBytes(StandardCharsets.UTF_8));
        String encryptedData = agent.encrypt(paddedData, key, iv);
        System.out.println("Encrypted Data: " + encryptedData);
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
func main() {
	data := Data{
		Account:  "1723205701015@jjserver.com",
		UID:      "110",
		Time:     "1732281558618904700",
		Username: "测试1",
		Wallet:   100,
	}
	Key := "2fdc826ed2d966e2f3fca09516446f75"
	IV := "xdm2fl24e0m3yo9c"
	jsonData, _ := json.Marshal(data)
	var agent Agents
	encryptedData, _ := agent.Encrypt(jsonData, []byte(Key), []byte(IV))
	fmt.Printf("Encrypted Data: %s\n", encryptedData)
}

```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
以上数据输出内容为

LHgsTGbF+GdhOS4lQSn3p72mumuptCzGzOTN0sqTD9R7efiVwvgIP8jzN4TZICkg8b+PMNrtJNvkIgI+wqioKfKMsBS8WaHjpHmjzZ7hsW2APxGIyvg05xdacmvOJbSBJSFq+gFb9VwTa23r8a/fC+S325k/ujDqiho1bCoeKko=
{% endhint %}

把以上数据用`Bearer 拼接上在协议头中加入到`Authorization中

