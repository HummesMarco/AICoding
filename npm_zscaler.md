# Fixing npm Certificate Error Behind Zscaler

1. **Convert the Zscaler CRT to PEM**
   - **With OpenSSL (Git-bash/WSL):**
     ```bash
     openssl x509 -inform DER \
                  -in "/c/source/.../cert copy.crt" \
                  -out "/c/certs/zscaler.pem" \
                  -outform PEM
     ```
   - **With Windows' certutil (Admin PowerShell/CMD):**
     ```powershell
     certutil -f -encode "C:\source\...\cert copy.crt" "C:\certs\zscaler.pem"
     ```

2. **Tell npm to trust that PEM**
   ```powershell
   npm config set cafile "C:\certs\zscaler.pem"
   npm config set strict-ssl true
   ```

3. **(Optional) Use `NODE_EXTRA_CA_CERTS`**
   ```powershell
   setx NODE_EXTRA_CA_CERTS "C:\certs\zscaler.pem"
   # Restart your shell, then:
   npm install
   ```

4. **Verify settings and install**
   ```powershell
   npm config get cafile
   npm config get strict-ssl
   node -e "console.log(process.env.NODE_EXTRA_CA_CERTS)"
   npm install --verbose
   ```

After following these steps, npm will trust your Zscaler certificate and no longer fail with `UNABLE_TO_GET_ISSUER_CERT_LOCALLY`.
