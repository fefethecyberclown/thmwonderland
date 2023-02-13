# TRYHACKME WONDERLAND
My write up about THM CTF Wonderland (Indo)

Machine: https://tryhackme.com/room/wonderland#

IP Address Machine: 10.10.120.136

### Information Gathering
Pertama kita melakukan nmap -A 10.10.120.136 untuk untuk melakukan port scanning. -A merupakan command untuk menunjukkan OS, version, script scanning, dan traceroute.

![image](https://user-images.githubusercontent.com/88881191/218485265-310c0f6c-929c-4f1d-8242-5fedb35aa8d2.png)

Dari hasil nmap, kita mendapatkan sistem operasi yang digunakan yaitu Service “Linux; CPE:” dengan deskripsi “cpe:/o:linux:linux_kernel”. Dalam kasus ini terdapat 2 port yang terbuka yaitu port 22 yang menggunakan servis OpenSSH 7. 6P1 dan port 80 atau tempat website target menggunakan service http. Kita mencoba membuka port 80 yang merupakan service http. Kita mengetikkan 10.10.120.136:80 dan menemukan page “Follow the White Rabbit.”

![image](https://user-images.githubusercontent.com/88881191/218485689-c467a4bc-ad71-4714-89a4-8716efa8d73d.png)

Mungkin di dalam image ini ada file tersembunyi, saya mencoba mencari file tersembuyi dengan menggunakan steghide

### Service Enumeration
Steghide

![image](https://user-images.githubusercontent.com/88881191/218485705-382f9e62-9cc3-4507-8d1d-ca056d282444.png)
![image](https://user-images.githubusercontent.com/88881191/218485746-1b5ef995-3554-46bd-a3c0-a3dfa197a3a5.png)
![image](https://user-images.githubusercontent.com/88881191/218485770-6084bff0-deed-49c8-b7b1-cbbc6c72f9b9.png)
![image](https://user-images.githubusercontent.com/88881191/218485780-13bf5396-7206-4723-adc1-0cd056e804ef.png)
![image](https://user-images.githubusercontent.com/88881191/218485793-6b7acf0a-ad0f-4b9d-8057-ee5a370e5605.png)
![image](https://user-images.githubusercontent.com/88881191/218485807-d394b0c8-c154-4d2a-bd89-da6f5e399b74.png)
![image](https://user-images.githubusercontent.com/88881191/218487636-de7fc54b-5c46-4f5f-98c5-6452273bccfe.png)
![image](https://user-images.githubusercontent.com/88881191/218487659-c66a446c-6e17-44fc-bcee-11d61eff278f.png)
![image](https://user-images.githubusercontent.com/88881191/218487688-0115c3a0-1d7b-4044-9731-067cc7ff6975.png)
![image](https://user-images.githubusercontent.com/88881191/218487676-f468d454-3bf3-426c-9320-cd7008207214.png)
![image](https://user-images.githubusercontent.com/88881191/218487695-fb9c71ef-65bb-4881-815d-738bcdbef1ab.png)
