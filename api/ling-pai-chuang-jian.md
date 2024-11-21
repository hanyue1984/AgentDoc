---
icon: key
---

# 令牌创建

令牌使用AES-128-CBC算法进行加密以下是几个主流语言的方法.如果需要其他的可以通过技术群取得联系

{% tabs %}
{% tab title="JavaScript" %}
{% code lineNumbers="true" fullWidth="true" %}
```javascript
const CryptoJS = require('crypto-js');

class Agents {
    /**
     * 使用 AES-128-CBC 加密数据
     *
     * @param {string} data 要加密的数据
     * @param {string} key 密钥
     * @param {string} iv 初始化向量
     * @return {string} 加密后的数据
     */
    encrypt(data, key, iv) {
        // 将字符串转换为字节数组
        const dataBytes = CryptoJS.enc.Utf8.parse(data);
        const keyBytes = CryptoJS.enc.Utf8.parse(key);
        const ivBytes = CryptoJS.enc.Utf8.parse(iv);

        // 填充数据
        const paddedData = this.pkcs7Padding(dataBytes);

        // 使用 AES-128-CBC 模式加密数据
        const encrypted = CryptoJS.AES.encrypt(paddedData, keyBytes, {
            iv: ivBytes,
            mode: CryptoJS.mode.CBC,
            padding: CryptoJS.pad.NoPadding // 使用自定义填充
        });

        // 将加密后的数据转换为 Base64 编码的字符串
        return encrypted.ciphertext.toString(CryptoJS.enc.Base64);
    }

    /**
     * PKCS7 填充
     *
     * @param {CryptoJS.lib.WordArray} data 要填充的数据
     * @return {CryptoJS.lib.WordArray} 填充后的数据
     */
    pkcs7Padding(data) {
        const blockSize = 16; // AES 块大小
        const paddingSize = blockSize - (data.sigBytes % blockSize);
        const padding = CryptoJS.lib.WordArray.create([paddingSize], paddingSize);
        return data.concat(padding);
    }
}

// 示例用法
const agents = new Agents();
const data = "Hello, World!";
const key = '2fdc826ed2d966e2f3fca09516446f75'; // 32字节
const iv = 'xdm2fl24e0m3yo9c'; // 16字节

const encryptedData = agents.encrypt(data, key, iv);
console.log("Encrypted Data: " + encryptedData);

```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code lineNumbers="true" %}
```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import base64

class Agents:
    def __init__(self):
        pass

    def encrypt(self, data, key, iv):
        """
        使用 AES-128-CBC 加密数据

        :param data: 要加密的数据
        :param key: 密钥
        :param iv: 初始化向量
        :return: 加密后的数据
        """
        # 创建 AES 密钥
        cipher = AES.new(key, AES.MODE_CBC, iv)

        # 填充数据
        padded_data = pad(data, AES.block_size)

        # 执行加密操作
        ciphertext = cipher.encrypt(padded_data)

        # 将加密后的数据转换为 Base64 编码的字符串
        return base64.b64encode(ciphertext).decode('utf-8')

# 示例用法
if __name__ == "__main__":
    agents = Agents()
    data = b"Hello, World!"
    key = b'2fdc826ed2d966e2f3fca09516446f75'  # 32字节
    iv = b'xdm2fl24e0m3yo9c'  # 16字节

    encrypted_data = agents.encrypt(data, key, iv)
    print("Encrypted Data:", encrypted_data)

```
{% endcode %}
{% endtab %}

{% tab title="PHP" %}
{% code lineNumbers="true" %}
```php
<?php
class Agents {
    /**
     * 使用 AES-128-CBC 加密数据
     *
     * @param string $data 要加密的数据
     * @param string $key 密钥
     * @param string $iv 初始化向量
     * @return string 加密后的数据
     */
    public function encrypt($data, $key, $iv) {
        // 填充数据
        $paddedData = $this->pkcs7Padding($data);

        // 使用 AES-128-CBC 模式加密数据
        $encryptedData = openssl_encrypt($paddedData, 'AES-128-CBC', $key, OPENSSL_RAW_DATA, $iv);

        // 将加密后的数据转换为 Base64 编码的字符串
        return base64_encode($encryptedData);
    }

    /**
     * PKCS7 填充
     *
     * @param string $data 要填充的数据
     * @return string 填充后的数据
     */
    private function pkcs7Padding($data) {
        $blockSize = 16; // AES 块大小
        $paddingSize = $blockSize - (strlen($data) % $blockSize);
        return $data . str_repeat(chr($paddingSize), $paddingSize);
    }
}

// 示例用法
$agents = new Agents();
$data = "Hello, World!";
$key = '2fdc826ed2d966e2f3fca09516446f75'; // 32字节
$iv = 'xdm2fl24e0m3yo9c'; // 16字节

$encryptedData = $agents->encrypt($data, $key, $iv);
echo "Encrypted Data: " . $encryptedData . "\n";
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

public class Agents {

    /**
     * 使用 AES-128-CBC 加密数据
     *
     * @param data 要加密的数据
     * @param key  密钥
     * @param iv   初始化向量
     * @return 加密后的数据
     * @throws Exception 如果加密过程中发生错误
     */
    public byte[] encrypt(byte[] data, byte[] key, byte[] iv) throws Exception {
        // 创建 AES 密钥
        SecretKeySpec secretKey = new SecretKeySpec(key, "AES");

        // 创建 CBC 模式的初始化向量
        IvParameterSpec ivSpec = new IvParameterSpec(iv);

        // 创建 Cipher 实例
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");

        // 初始化 Cipher 为加密模式
        cipher.init(Cipher.ENCRYPT_MODE, secretKey, ivSpec);

        // 填充数据
        byte[] paddedData = pkcs7Padding(data);

        // 执行加密操作
        byte[] ciphertext = cipher.doFinal(paddedData);

        // 将加密后的数据转换为 Base64 编码的字符串
        return Base64.getEncoder().encode(ciphertext);
    }

    /**
     * PKCS7 填充
     *
     * @param data 要填充的数据
     * @return 填充后的数据
     */
    private byte[] pkcs7Padding(byte[] data) {
        int blockSize = 16; // AES 块大小
        int paddingSize = blockSize - (data.length % blockSize);
        byte[] paddedData = new byte[data.length + paddingSize];
        System.arraycopy(data, 0, paddedData, 0, data.length);
        for (int i = data.length; i < paddedData.length; i++) {
            paddedData[i] = (byte) paddingSize;
        }
        return paddedData;
    }

    public static void main(String[] args) {
        try {
            Agents agents = new Agents();
            byte[] data = "Hello, World!".getBytes(StandardCharsets.UTF_8);
            byte[] key = "2fdc826ed2d966e2f3fca09516446f75".getBytes(StandardCharsets.UTF_8); // 32字节
            byte[] iv = "xdm2fl24e0m3yo9c".getBytes(StandardCharsets.UTF_8); // 16字节

            byte[] encryptedData = agents.encrypt(data, key, iv);
            System.out.println("Encrypted Data: " + new String(encryptedData, StandardCharsets.UTF_8));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code lineNumbers="true" %}
```go
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
```
{% endcode %}
{% endtab %}
{% endtabs %}

加密分为几个步骤

1.结构体创建

2.转化成json字符串

3.通过key跟iv进行加密&#x20;

4.转化base64得到最终得令牌

以下是各语言得结构体



{% tabs %}
{% tab title="Go" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}
