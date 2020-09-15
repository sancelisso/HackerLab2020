# HackerLab2020 - Challenge 2

## Challenge

>Pour la suite, il s'agit de se connecter par ssh sur cette IP:
user: chal0x01
IP:51.210.43.224
Port: 1022

A cette étape, nous avons comme données : une clé publique, les informations de connexion à un serveur ssh,un message chiffré, mais pas mot de passe. L'objectif ici est donc de trouver le mot de passe pour pouvoir se connecter.
## Solution

Pour y arriver, nous avons généré une clé privée à partir de la clé publique RSA avec l'outil [RSACtfTool](https://github.com/Ganapati/RsaCtfTool)

![Génération de la clé privéé](Images/priv.png)

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAvb5qikCKoFmVBHvmlcVmBC6kVj3I4do/45SKpCYM5feEcqyY
17hDl1A1XAQC+IQOkdKbfM8ASmnB7mvuASJGHhqKupoMmr8sNW/4mhS66hIT1OK2
hECU6ob86YaABgECMYEhysK6O5I/tKFzvdp0I2aBBxcFpHI6Cc4+Pnopq7uxjn3h
X6k8z9fifqeGSl1n9Hmy4nhEPFgQJJFmXQTaoyDRJbO62pklH+ytfiONF4gNaB7c
NFCwgnoNTscBEveKZ6594veFiDoSRxUbLc9VCMi8f9UBGo+XHP/0wp/HS7I/zIoR
ZmAfN41Hs7xw4kdLyqNfkVNUkYnbMQa4L680VwIDAQABAoIBAQCuiz64hRzA2wdh
A5UfS2JIEJvuFZH4aWuCL7/FtpcA6Ut2rd0/++p5hDhBuBL/h4Nbk1CjZEyRJ25V
pAViF6RGxwW9GD7mD1xcn+8h1lYHQq/pktBxInOatqhrLeYRH9ZjsbVjLvRgHm3p
/Oday10wv3eMDRqMpuNK5+WMVZaFuWLe6K5EGEjt9N9MPEOssy9Ukw0+6STw23d7
G+z1U1HWWTd63wmq2sFKffUzgwO1I0I3jFqMgc8PxdqgKXaqlUT/0me0w3o+heaP
pSdeVFaN350U7wxPwdFydvt7Ay0w2ce1VMP7SdK+2GeXawAeE58Olk0u2s+ovat9
JiKZnQYxAoGBANoVOvpdOuITqMS3t9fbrNBicMDquimqbtTtGS4mzY3HqmHXwRPR
QA7cgf+PqoR6cWLbjeLN9OmuzxtjOSSikkZIzwaKyPNaaP0sZMi363TUE4WVcA5a
Bh9DB0iw5D2Y4zMGLnRNcF0KHQMLccuryA3nRV8QiDfRcZ5OS3W/79tZAoGBAN67
0ym97kevVvRcp/X4i0iRaue1bgssNHMmxoSQfgogkU1c7UL2j3Gl6gMNTf3djo1S
frboITz97pz8Bn8bs8mHJzbwdJbQ5KQK8LN3aEcTwnr2C7fzC/zNujEG44q8BgQ/
aB0VEr1lwvW4/5woyWMEdAmK/SqJM13n9dj4iocvAoGAZSPXeGlod5TafnEdx/RP
T014MWAq+kvantFfDjJcvlZNRJdFKiWvabUGbTl5wXrvvwNH6FnQLdguoOsBg2YK
JKJ7sPMIgyP48QF3tEbn4gTBEQDfOSizEiiJ5kJzd8XCCNXjEXkglWWtUs4p9mUy
oJATciU+p3RxFxQElxVu7fECgYEAqhph23gtOVwEmZ0FvUly/hnovksGXswExoiv
heBgEdXrglzc6o4o4M+oV2pazO3fB7kgAwpnEBCSohurznlWpits3H1pXSDt7QZZ
64XfAqFpnsS4EGUzG3/2mai8MGEGa0oNdmB4rmLAMS93L0jK/K8S+5LFZC3iJ+6W
CH1QNosCgYAjKCYtMoE1PCxUO+MahPi5vo879t7JdgVfK8lnD9ZRAZYv00/vVhYj
FWvS565IAdHJ9riCnA5Eak74E5GfIfefXq380vjh0Cq3MLxquJoG69+wU4DavErO
qz5JfxfpmSyURihSLVsRm06Z/bA78932QW0H25anratFbgURllmtTw==
-----END RSA PRIVATE KEY-----
```

A présent, nous allons déchiffrer le message à l'aide du module RSA de python3

```python
from Crypto.PublicKey import RSA

private_key_file = open('priv.key','r').read()
#private-key est le nom de notre fichier contenant la clé privée
private_key = RSA.importKey(private_key_file) #importation de la clé

m = b"""\x98\xe7\xbd\x1d\xdc\xaf\x03\xc1\xe4\x9a!f\xd6\xb0\xbe-\xe05\x93\x80\x8aN\x1b%!\xe2\x88\x92\xaa\x8aod\xb7V\xfa\xe1\xd2v\x05\xb9\x80t\x83\xa4^\xe6W\\\x91\x1a\xf$

#m est le message chiffré

flag = private_key.decrypt(m) # déchiffrement du message
print(flag.decode('utf-8')) # affichage en format  utf-8
```
A l'éxécution, nous obtenons finalement le message contenant le flag et le mot de passe de connexion au serveur SSH: **Good. Soumet ce flag pour avoir validé cette étape: CTF_ImaB3Cryt0Analyste. Voici le mot de passe pour te connecter au système SSH: chal0x01_qualif**


```FLAG: CTF_ImaB3Cryt0Analyste ```
