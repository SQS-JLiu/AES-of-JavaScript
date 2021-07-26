# AES-of-JavaScript
两种JavaScript的AES加密方式（可与Java相互加解密）

由于JavaScript属于弱类型脚本语言，因此当其与强类型的后台语言进行数据交互时会产生各种问题，特别是加解密的操作。本人由于工作中遇到用js与Java进行相互加解密的问题，在网上查了很多资料及代码段，均无法解决。后总结多篇文档内容终于找到解决办法，现记录与此：

 第一种：加解密时需要秘钥（key）和秘钥偏移量（iv）的情况：
  
//该方法可与Java进行相互加解密 ，需要秘钥（key）及秘钥偏移量（iv）的aes加解密
##
    <script>
        <script src="aes_1.js"></script>　
        var key = CryptoJS.enc.Utf8.parse("十六位十六进制数作为秘钥");  
        var iv  = CryptoJS.enc.Utf8.parse('十六位十六进制数作为秘钥偏移量');  
        function Encrypt(word){
            srcs = CryptoJS.enc.Utf8.parse(word);
            var encrypted = CryptoJS.AES.encrypt(srcs, key, { iv: iv,mode:CryptoJS.mode.CBC,padding: CryptoJS.pad.Pkcs7});
            return encrypted.ciphertext.toString().toUpperCase();
        }

        function Decrypt(word){  
            var encryptedHexStr = CryptoJS.enc.Hex.parse(word);
            var srcs = CryptoJS.enc.Base64.stringify(encryptedHexStr);
            var decrypt = CryptoJS.AES.decrypt(srcs, key, { iv: iv,mode:CryptoJS.mode.CBC,padding: CryptoJS.pad.Pkcs7});
            var decryptedStr = decrypt.toString(CryptoJS.enc.Utf8); 
            return decryptedStr.toString();
        }

        var mm = Encrypt('nihao')
        console.log(mm);
        var jm = Decrypt(mm);
        console.log(jm)
    </script>
//如果想要深度了解每步作用，可以参考：http://zhidao.baidu.com/question/647688575019014285.html?qbl=relate_question_0&word=javascript%20aes

第二种：加解密时仅需要秘钥，在线验证地址：http://encode.chahuo.com/
## 
    <script type="text/javascript">
        <script src="aes_2.js"></script>　　//引入的js文件在该链接中：https://github.com/hellobajie/AES-of-JavaScript
        var pwd="秘钥";

        function Encrypt(word){
            return CryptoJS.AES.encrypt(word,pwd).toString();
        }

        function Decrypt(word){
            return CryptoJS.AES.decrypt(word,pwd).toString(CryptoJS.enc.Utf8);
        }

        var mm = Encrypt('nihao');
        console.log(mm)
        var jm = Decrypt(mm);
        console.log(jm)
       
    </script>
