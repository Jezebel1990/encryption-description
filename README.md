### Encryption Key (16,24, or 32 bytes) ###
$encryptionKey = [Text.Encoding]::UTF8.GetBytes("MySecretEncryptionKey")

# Text to encrypt
$textToEncrypt = "Segredo."

#Convert the text to byes
$bytesToEncrypt = [Text.Encoding]::UTF8.GetBytes($textToEncrypt)

# Create AES encryption object
$aes = [System.Security.Cryptography.Aes]::Create()
$aes.Mode = [System.Security.Cryptography.CipherMode]::CBC
$aes.Padding = [System.Security.Cryptography.PaddingMode]::PKCS7

$aes = [System.Security.Cryptography.Aes]::Create()

$aes.Mode = [System.Security.Cryptography.CipherMode]::CBC
$aes.Padding = [System.Security.Cryptography.PaddingMode]::PKCS7

# Generate a random IV (Initialization Vector)
$aes.GenerateIV()

# Create an encryption stram
$encryptor = $aes.CreateEncryptor()

# Encrypt the bytes
$encryptedBytes = $encryptor.TransformFinalBlock($bytesToEncrypt, 0, $bytesToEncrypt.Length)

# Convert the encrypted bytes to Base64 for storage
$encryptedText = [Convert]::ToBase64String($aes.Iv + $encryptedBytes)

# Display the encrypted text
Write-Host "Encrypted Text: $encryptedText"



#### Convert the Base64 encrypted text back to bytes ####
$encryptedBytesWithIV = [Convert]::FromBase64String($encryptedText)
$iv = $encryptedBytesWithIV[0..15]
$encryptedBytesOnly = $encryptedBytesWithIv[16..($encryptedBytesWithIV.Length - 1)]

# Set the IV and decrypt
$aes.IV = $iv
$decryptor = $aes.CreateDecryptor()
$decryptedBytes = $decryptor.TransformFinalBlock($encryptedBytesOnly, 0, $encryptedBytesOnly.Length)

# Convert the decrypted bytes back to text
$decryptedText = [Text.Encoding]::UTF8.GetString($decryptedBytes)

# Display the decrypted text
Write-Host "Decrypted Text: $decryptedText"
