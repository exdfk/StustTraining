# Advanced Encryption Standard
```csharp
using System.Security.Cryptography;
using System.Text;
using System;
namespace exteduapi.Api.Provider
{
    public class AESCryptoProvider
    {        
        /// 需要32個長度     
        private string innerkey
        {
            get
            {
                return "50645367566B59703373367639792F42";
            }
        }
     
        /// 需要16個位元     
        private string inneriv
        {
            get
            {
                return "3273357638792F42";
            }
        }

     
        /// 加密

        public string Encrypt(string source)
        {

            byte[] sourceBytes = Encoding.UTF8.GetBytes(source);
            var aes = new RijndaelManaged();
            aes.Key = Encoding.UTF8.GetBytes(innerkey);
            aes.IV = Encoding.UTF8.GetBytes(inneriv);
            aes.Mode = CipherMode.CBC;
            aes.Padding = PaddingMode.PKCS7;
            ICryptoTransform transform = aes.CreateEncryptor();
            return Convert.ToBase64String(transform.TransformFinalBlock(sourceBytes, 0, sourceBytes.Length));
        }

        /// <summary>
        /// 解密
        /// </summary>
        /// <param name="encryptData"></param>
        /// <returns></returns>
        public string Decrypt(string encryptData)
        {
            var encryptBytes = Convert.FromBase64String(encryptData);
            var aes = new RijndaelManaged();
            aes.Key = Encoding.UTF8.GetBytes(innerkey);
            aes.IV = Encoding.UTF8.GetBytes(inneriv);
            aes.Mode = CipherMode.CBC;
            aes.Padding = PaddingMode.PKCS7;
            ICryptoTransform transform = aes.CreateDecryptor();
            return Encoding.UTF8.GetString(transform.TransformFinalBlock(encryptBytes, 0, encryptBytes.Length));
        }
    }
}
```

### <a href="https://www.allkeysgenerator.com/Random/Security-Encryption-Key-Generator.aspx" target="_blank">Encryption Key Generator </a>