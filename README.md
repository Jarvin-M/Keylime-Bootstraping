# Keylime-Bootstraping
Project for Advanced Topics in Security and Privacy based on Keylime - keylime.dev

### What has worked so far
```git clone https://github.com/keylime/keylime.git
python3 -m venv atsp
source atsp/bin/activate
cd keylime
sudo ./installer.sh -m -s
```
Making tpm2 ready
`sudo apt-get install libsapi0 libsapi-utils libsapi-dev tpm2-tools tpm2-abrmd` 


Initialize Start the TPM emulator - https://github.com/keylime/tpm4720-keylime
``
/usr/local/bin/init_tpm_server
/usr/local/bin/tpm_serverd
``
Check if tpm is running 
`dmesg | grep -i tpm
sudo find /sys -name tpm0
export TPM2TOOLS_TCTI="mssim:port=2321"
`

```
sudo keylime_verifier
sudo keylime_registrar
sudo keylime_agent
```

```sudo /usr/local/bin/tpm_serverd ```
Change the values in the registrar_client.py and tenant.py in `/usr/local/lib/python3.6/dist-packages/keylime-1.2-py3.6.egg/keylime/`
to the respective names in your `/var/lib/keylime/cv_ca/`

Create plaintext file filetosend.txt
`sudo keylime_tenant -c update -t 127.0.0.1 -v 127.0.0.1 -u D432FBB3-D2F1-4A97-9EF7-75BD81C00000 -f filetosend.txt`

## Secure Payload

Encrypted data is provisioned to an node- `keylime_agent`and can be used to bootstrap a system. Files that need to be sent to a cloud agent `keylime_agent` are bundled together and sent to the agent together with a user key needed to decrypt the files. 
### Single file encryption
`sudo keylime_tenant -c update -t 127.0.0.1 -v 127.0.0.1 -u D432FBB3-D2F1-4A97-9EF7-75BD81C00000 -f filetosend.txt` 

### Certificate mode
 `keylime_tenant -c update -t 127.0.0.1 -v 127.0.0.1 -u D432FBB3-D2F1-4A97-9EF7-75BD81C00000 --cert /var/lib/keylime/ca --include /home/jarvin/Documents/ATSP/project/inclddir/ `
 
 inclddir contains autorun.sh and the files that need to be sent.
 The --cert appends revocation certificate at in the directory `/var/lib/keylime/ca` for the agent with the specified UUID -u.This certificate is then handled by the `keylime_verifier`. --include adds optional data files that is included in a zipped folder together with the certificated and securely sent to the target agent `keylime_agent`.
 
 The `keylime_verifier` handles certificate revocation whereby if a new agent is added that doesn't match the `keylime_tenant`'s specification then they can not decrypt the files
 
The provisioning successfully happpens and decrypted files can be seen in the directory `/var/lib/keylime/secure/unzipped` containing the certificates and the files. 

 
