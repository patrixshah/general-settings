# Solution: Create an SSL Certificate with Subject Alternative Names (SANs)

you need to create a self-signed SSL certificate that includes the SAN field. Here’s how you can do this using OpenSSL on a Windows system:

### Step-by-Step Guide to Create a Self-Signed SSL Certificate with SANs

- Open Command Prompt as Administrator

  Make sure you have OpenSSL installed and open the Command Prompt with administrative privileges. For windows user better to use "Git Bash Here".

- Navigate to Your Project Directory

  Change to the directory where you want to save your SSL certificate files:

  ```bash
  cd path\to\your\project
  ```

- Create an OpenSSL Configuration File
  Create a new text file named openssl-san.cnf in your project directory. This file will be used to specify the SANs for your certificate. Add the following content to the file:

  ```ini
  [req]
  default_bits       = 2048
  default_md         = sha256
  prompt             = no
  default_keyfile    = localhost.key
  distinguished_name = dn
  req_extensions     = req_ext

  [dn]
  C  = US
  ST = State
  L  = City
  O  = Organization
  OU = Organizational Unit
  CN = localhost

  [req_ext]
  subjectAltName = @alt_names

  [alt_names]
  DNS.1 = localhost
  DNS.2 = 127.0.0.1
  DNS.3 = yourlocaldomain.test
  ```

  Make sure to replace the details under [dn] with your actual details (they can be dummy details for local development). Under [alt_names], list all the domains (SANs) that should be covered by the certificate (e.g., localhost, 127.0.0.1, or any custom local domain).

- Generate a Private Key
  Run the following command to generate a private key:

  ```bash
  openssl genrsa -out localhost.key 2048
  ```

- Generate a Certificate Signing Request (CSR) with the SAN Configuration
  Use the OpenSSL configuration file created earlier to generate a CSR:

  ```bash
  openssl req -new -key localhost.key -out localhost.csr -config openssl-san.cnf
  ```

- Generate a Self-Signed Certificate with SANs
  Finally, generate a self-signed SSL certificate that includes SANs:

  ```bash
  openssl x509 -req -in localhost.csr -signkey localhost.key -out localhost.crt -days 365 -extensions req_ext -extfile openssl-san.cnf
  ```

  This command uses the configuration file to add the SANs to the self-signed certificate.

- Configure Your Local Server to Use the New Certificate
  If you are using Apache, update your virtual host configuration to use the new certificate:

  ```apache
  <VirtualHost *:443>
      DocumentRoot "C:/path/to/your/project/public"
      ServerName localhost

      SSLEngine on
      SSLCertificateFile "C:/path/to/your/project/localhost.crt"
      SSLCertificateKeyFile "C:/path/to/your/project/localhost.key"

      <Directory "C:/path/to/your/project/public">
          AllowOverride All
          Require all granted
      </Directory>
  </VirtualHost>
  ```

  If you are using Nginx, update your server block:

  ```nginx
  server {
      listen 443 ssl;
      server_name localhost;

      ssl_certificate "C:/path/to/your/project/localhost.crt";
      ssl_certificate_key "C:/path/to/your/project/localhost.key";

      root "C:/path/to/your/project/public";
      index index.php index.html;

      location / {
          try_files $uri $uri/ /index.php?$query_string;
      }

      location ~ \.php$ {
          include snippets/fastcgi-php.conf;
          fastcgi_pass unix:/var/run/php/php-fpm.sock; # Adjust as needed
      }
  }
  ```

  Restart your web server to apply the changes:

  Apache: httpd -k restart or through your XAMPP/WAMP control panel.
  Nginx: nginx -s reload or restart the service.

- Add the Certificate to the Trusted Root Certification Authorities (Optional but Recommended)
  To avoid warnings and errors, add the certificate to your system’s Trusted Root Certification Authorities:

  1. **Open the localhost.crt File:** Double-click the **localhost.crt** file to open it.
  2. **Install the Certificate:** Click "Install Certificate" > "Local Machine" > "Next".
  3. **Choose the Certificate Store:** Select "Place all certificates in the following store" > "Browse" > choose "Trusted Root Certification Authorities" > "Next".
  4. **Complete the Wizard:** Click "Finish" to install the certificate.

- Access Your Site with HTTPS
  Open Chrome and navigate to https://localhost or your specific local domain. The net::ERR_CERT_COMMON_NAME_INVALID error should no longer appear if the SANs are correctly set.

# Conclusion

By creating a self-signed SSL certificate with the **Subject Alternative Names (SANs)** properly configured, you can avoid the **`net::ERR_CERT_COMMON_NAME_INVALID`** error in Chrome. Remember that this solution is suitable for local development purposes. For production environments, always use a certificate from a trusted Certificate Authority (CA).
