# TRYHACKME WONDERLAND
My write up about THM CTF Wonderland (Indo)

Machine: https://tryhackme.com/room/wonderland#

IP Address Machine: 10.10.120.136

### Information Gathering
Pertama kita melakukan nmap -A 10.10.120.136 untuk untuk melakukan port scanning. -A merupakan command untuk menunjukkan OS, version, script scanning, dan traceroute.

![image](https://user-images.githubusercontent.com/88881191/218485265-310c0f6c-929c-4f1d-8242-5fedb35aa8d2.png)

Dari hasil nmap, kita mendapatkan sistem operasi yang digunakan yaitu Service “Linux; CPE:” dengan deskripsi “cpe:/o:linux:linux_kernel”. Dalam kasus ini terdapat 2 port yang terbuka yaitu port 22 yang menggunakan servis OpenSSH 7. 6P1 dan port 80 atau tempat website target menggunakan service http. Kita mencoba membuka port 80 yang merupakan service http. Kita mengetikkan 10.10.120.136:80 dan menemukan page “Follow the White Rabbit.”


Mungkin di dalam image ini ada file tersembunyi, saya mencoba mencari file tersembuyi dengan menggunakan steghide

### Service Enumeration
Steghide

