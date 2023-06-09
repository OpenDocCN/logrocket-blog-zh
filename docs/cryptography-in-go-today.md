# 当今世界的密码学——log rocket 博客

> 原文：<https://blog.logrocket.com/cryptography-in-go-today/>

密码学是在第三方对手面前进行安全通信的技术实践和研究。在 web 应用程序中，开发人员使用加密技术来确保用户数据的安全，并确保系统不会被不良分子利用，这些不良分子可能会利用系统中的漏洞谋取私利。

大多数编程语言都有自己的通用加密原语、算法等的实现。在这篇文章中，我们将会看到 Go 编程语言是如何处理加密的，以及现在有哪些可用的加密软件包。

首先，让我们看看标准 Go 库中的 crypto 包。

## Go 的标准加密包

如果你写 Go 已经有相当长的时间了，你会同意标准的 Go 库非常健壮，涵盖了从 HTTP 到编码甚至测试的所有内容。因此，Go 自带加密软件包也就不足为奇了。

加密包本身包含常见的加密常数、基本加密原则的实现等等。然而，它的大部分价值在于它的子包。crypto 包有各种各样的子包，每个子包都专注于一个加密算法、原则或标准。

我们有 aes 包，重点是 [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (高级加密标准)；hmac，专注于 [HMAC](https://en.wikipedia.org/wiki/HMAC) (基于哈希的消息认证码)进行数字签名和验证；和许多其他人。有了这些包，我们可以执行不同的加密相关的任务，如加密、解密、散列等。让我们探索一下如何做到这一点。

### 散列法

杂凑基本上是取得任意大小的输入并产生固定大小的输出的过程。至少，一个好的散列算法绝不会为两个不同的输入产生相同的输出，而总是为一个给定的输入产生相同的输出。

有许多不同的哈希算法，如 SHA-256、SHA-1 和 MD5——所有这些算法在 Go crypto 包中都受支持——以及其他一些算法。下面是一个函数的实现，它使用 SHA-256 散列算法对明文进行散列，并以十六进制格式返回散列。

```
func hashWithSha256(plaintext string) (string, error) {
   h := sha256.New()
   if _, err := io.WriteString(h, plaintext);err != nil{
      return "", err
   }
   r := h.Sum(nil)
   return hex.EncodeToString(r), nil
}

func main(){
  hash, err := hashWithSha256("hashsha256")
  if err != nil{
     log.Fatal(err)
  }
  fmt.Println(hash)  //c4107b10d93310fb71d89fb20eec1f4eb8f04df12e3f599879b03be243093b14
}

```

如您所见，sha256 子包的`New`函数返回了一个实现[哈希接口](https://golang.org/pkg/hash/#Hash)的类型。任何实现此接口的类型也会实现 Writer 接口。因此，我们可以简单地将我们的明文写入其中，使用`Sum`方法获得校验和，并以十六进制格式对结果进行编码。

这段代码也适用于其他散列算法——您只需要从适当的包中创建一个散列。因此，如果我们使用 MD5 算法进行哈希运算，我们会得到:

```
h := md5.New()

```

### 对称密钥密码术

我们也可以只使用 Go 标准库来实现[对称密钥加密](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)。对称密钥密码术简单地包括用相同的密钥加密明文和解密相应的密文。

有了 Go crypto 包，我们可以利用流密码和块密码进行加密和解密。让我们来看看如何在 [CBC(密码块链接)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_block_chaining_(CBC))模式下使用 AES 实现对称密钥加密。

首先，我们编写一个函数，用给定的密钥创建一个新的分组密码。AES 只接受密钥长度为 128、192 或 256 位的密钥。因此，我们将散列给定的密钥，并将散列作为分组密码的密钥传递。这个函数从**密码**子包返回一个[块](https://golang.org/pkg/crypto/cipher/#Block)和一个错误。

```
func newCipherBlock(key string) (cipher.Block, error){
   hashedKey, err := hashWithSha256(key)
   if err != nil{
      return nil, err
   }
   bs, err := hex.DecodeString(hashedKey)
   if err != nil{
      return nil, err
   }
   return aes.NewCipher(bs[:])
}

```

在我们开始编写用于加密和解密的函数之前，我们需要编写两个函数，分别用于填充和解填充明文。填充只是增加明文长度的行为，这样它可以是固定大小(通常是块大小)的倍数。这通常通过在明文中添加字符来实现。

有不同的填充方案，因为 Go 不会自动填充明文，所以我们必须自己填充。用户[胡应环](https://gist.github.com/huyinghuan)的这个 [GitHub 要点](https://gist.github.com/huyinghuan/7bf174017bf54efb91ece04a48589b22)展示了一种使用 PKCS7 填充方案填充明文的简单方法，该方案在 [RFC 2315](https://tools.ietf.org/html/rfc2315) 的第 10.3 节中定义。

```
var (
   // ErrInvalidBlockSize indicates hash blocksize <= 0.
   ErrInvalidBlockSize = errors.New("invalid blocksize")

   // ErrInvalidPKCS7Data indicates bad input to PKCS7 pad or unpad.
   ErrInvalidPKCS7Data = errors.New("invalid PKCS7 data (empty or not padded)")

   // ErrInvalidPKCS7Padding indicates PKCS7 unpad fails to bad input.
   ErrInvalidPKCS7Padding = errors.New("invalid padding on input")
)

func pkcs7Pad(b []byte, blocksize int) ([]byte, error) {
   if blocksize <= 0 {
      return nil, ErrInvalidBlockSize
   }
   if b == nil || len(b) == 0 {
      return nil, ErrInvalidPKCS7Data
   }
   n := blocksize - (len(b) % blocksize)
   pb := make([]byte, len(b)+n)
   copy(pb, b)
   copy(pb[len(b):], bytes.Repeat([]byte{byte(n)}, n))
   return pb, nil
}

func pkcs7Unpad(b []byte, blocksize int) ([]byte, error) {
   if blocksize <= 0 {
      return nil, ErrInvalidBlockSize
   }
   if b == nil || len(b) == 0 {
      return nil, ErrInvalidPKCS7Data
   }

   if len(b)%blocksize != 0 {
      return nil, ErrInvalidPKCS7Padding
   }
   c := b[len(b)-1]
   n := int(c)
   if n == 0 || n > len(b) {
      fmt.Println("here", n)
      return nil, ErrInvalidPKCS7Padding
   }
   for i := 0; i < n; i++ {
      if b[len(b)-n+i] != c {
         fmt.Println("hereeee")
         return nil, ErrInvalidPKCS7Padding
      }
   }
   return b[:len(b)-n], nil
}

```

既然我们已经记下了，我们就可以写加密和解密的函数了。

```
//encrypt encrypts a plaintext
func encrypt(key, plaintext string) (string, error) {
   block, err := newCipherBlock(key)
   if err != nil {
      return "", err
   }

  //pad plaintext
   ptbs, _ := pkcs7Pad([]byte(plaintext), block.BlockSize())

   if len(ptbs)%aes.BlockSize != 0 {
      return "",errors.New("plaintext is not a multiple of the block size")
   }

   ciphertext := make([]byte, len(ptbs))

  //create an Initialisation vector which is the length of the block size for AES
   var iv []byte = make([]byte, aes.BlockSize)
   if _, err := io.ReadFull(rand.Reader, iv); err != nil {
      return "", err
   }

   mode := cipher.NewCBCEncrypter(block, iv)

  //encrypt plaintext
   mode.CryptBlocks(ciphertext, ptbs)

  //concatenate initialisation vector and ciphertext
   return hex.EncodeToString(iv) + ":" + hex.EncodeToString(ciphertext), nil
}

//decrypt decrypts ciphertext
func decrypt(key, ciphertext string) (string, error) {
   block, err := newCipherBlock(key)
   if err != nil {
      return "", err
   }

  //split ciphertext into initialisation vector and actual ciphertext
   ciphertextParts := strings.Split(ciphertext, ":")
   iv, err := hex.DecodeString(ciphertextParts[0])
   if err != nil {
      return "", err
   }
   ciphertextbs, err := hex.DecodeString(ciphertextParts[1])
   if err != nil {
      return "", err
   }

   if len(ciphertextParts[1]) < aes.BlockSize {
      return "", errors.New("ciphertext too short")
   }

   // CBC mode always works in whole blocks.
   if len(ciphertextParts[1])%aes.BlockSize != 0 {
      return "", errors.New("ciphertext is not a multiple of the block size")
   }

   mode := cipher.NewCBCDecrypter(block, iv)

   // Decrypt cipher text
   mode.CryptBlocks(ciphertextbs, ciphertextbs)

  // Unpad ciphertext
   ciphertextbs, err = pkcs7Unpad(ciphertextbs, aes.BlockSize)
   if err != nil{
      return "", err
   }

   return string(ciphertextbs), nil
}

```

我们现在可以这样测试我们的函数:

```
func main() {
  pt := "Highly confidential message!"
  key := "aSecret"

   ct, err := encrypt(key, pt)
   if err != nil{
      log.Fatalln(err)
   }
   fmt.Println(ct)  //00af9595ed8bae4c443465aff651e4f6:a1ceea8703bd6aad969a64e7439d0664320bb2f73d9a31433946b81819cb0085

   ptt, err := decrypt(key, ct)
   if err != nil{
      log.Fatalln(err)
   }
   fmt.Println(ptt)  //Highly confidential message!

}

```

### 公钥密码学

[公钥加密](https://en.wikipedia.org/wiki/Public-key_cryptography)不同于对称密钥加密，不同的密钥用于加密和解密。存在两种不同的密钥:用于解密的私钥和用于加密的公钥。

RSA 是公钥密码系统的一个流行例子，可以在 Go 中使用 [rsa](https://golang.org/pkg/crypto/rsa/) 子包来实现。

要实现 RSA，我们必须首先生成我们的私钥和公钥。为此，我们可以使用`GenerateKey`生成一个私钥，然后从私钥生成公钥。

```
func main(){
//create an RSA key pair of size 2048 bits
  priv, err := rsa.GenerateKey(rand.Reader, 2048)
  if err != nil{
     log.Fatalln(err)
  }

  pub := priv.Public()
}

```

我们现在可以结合使用 RSA 和 OAEP 来加密和解密明文和密文。

```
func main(){
    ...
    options := rsa.OAEPOptions{
     crypto.SHA256,
     []byte("label"),
  }

  message := "Secret message!"

  rsact, err := rsa.EncryptOAEP(sha256.New(), rand.Reader, pub.(*rsa.PublicKey), []byte(message), options.Label)
  if err != nil{
     log.Fatalln(err)
  }

  fmt.Println("RSA ciphertext", hex.EncodeToString(rsact))

  rsapt, err := priv.Decrypt(rand.Reader,rsact, &options)
  if err != nil{
     log.Fatalln(err)
  }

  fmt.Println("RSA plaintext", string(rsapt))

}

```

### 数字签名

数字签名是密码学的另一个应用。数字签名基本上允许我们验证通过网络传输的消息没有被攻击者篡改。

实现数字签名的一种常见方法是使用消息认证码(MAC)，特别是 HMAC。HMACs 利用散列函数，是确保消息真实性的安全方式。我们可以使用 [hmac](https://golang.org/pkg/crypto/hmac/) 子包在 Go 中实现 hmac。

这里有一个如何做到这一点的例子:

```
/*hmacs make use of an underlying hash function so we have to specify one*/
mac := hmac.New(sha256.New, []byte("secret"))
mac.Write([]byte("Message whose authenticity we want to guarantee"))
macBS := mac.Sum(nil) //

falseMac := []byte("someFalseMacAsAnArrayOfBytes")
equal := hmac.Equal(falseMac, macBS)
fmt.Println(equal)  //false - therefore the message to which this hmac is attached has been tampered

```

## Bcrypt

除了 Go 标准加密库，Go 生态系统中还有其他与加密相关的包。其中之一就是 [bcrypt](https://godoc.org/golang.org/x/crypto/bcrypt) 。

bcrypt 包是流行的散列算法 [bcrypt](https://en.wikipedia.org/wiki/Bcrypt) 的 Go 实现。Bcrypt 是散列密码的行业标准算法，大多数语言都有某种形式的 bcrypt 实现。

在这个包中，我们可以使用`GenerateFromPassword`函数从密码中获得 bcrypt 散列，并传递一个代价。

```
hash, err := bcrypt.GenerateFromPassword("password", bcrypt.DefaultCost)

```

然后，我们可以通过执行以下操作来检查给定的密码是否与给定的哈希匹配:

```
err := bcrypt.CompareHashAndPassword([]byte("hashedPassword"), []byte("password"))

```

## 结论

本文到此为止！希望这篇文章能让您了解 Go 生态系统有多健壮，至少在密码学方面。你也可以在这里查看 Go 标准库[的内容，看看 Go 还包含了什么。](https://golang.org/pkg/#stdlib)

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)