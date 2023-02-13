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
Steghide merupakan tool untuk mencari hidden data pada suatu file, Pada kasus ini kita mendownload foto White Rabbit kemudian kita menggunakan command “steghide extract -sf white_rabbit_1.jpg” dan mengosongkan passphrase, kemudian ditemukan hidden file yaitu hint.txt yang langsung terdownload di kalilinux kita.

![image](https://user-images.githubusercontent.com/88881191/218485705-382f9e62-9cc3-4507-8d1d-ca056d282444.png)

![image](https://user-images.githubusercontent.com/88881191/218485746-1b5ef995-3554-46bd-a3c0-a3dfa197a3a5.png)

Kita mencoba command “cat hint.txt” dan ditemukan clue “follow the r a b b i t” namun clue ini masih belum terlalu memberikan bayangan untuk masuk ke next step dari penetration ini, sehingga kita menggunakan dirbuster dan berharap kita mendapatkan informasi tambahan.

![image](https://user-images.githubusercontent.com/88881191/218485770-6084bff0-deed-49c8-b7b1-cbbc6c72f9b9.png)

Dirbuster
Kita menggunakan dirbuster untuk mencari hidden file pada page Follow the White Rabbit. Pertama kita memasukkan http://10.10.120.136:80 (web target kita), check Go Faster, check List based brute force, file list nya kita menggunakan payload medium.txt, uncheck Be Recursive, dan masukkan file extension yang kita curigai yaitu php,txt,xlsx,html,pdf. Kemudian kita start.

![image](https://user-images.githubusercontent.com/88881191/218485780-13bf5396-7206-4723-adc1-0cd056e804ef.png)

Kita mendapatkan hasil directory /r/a/b/b/i/t yang muncul paling atas. Kita akan mencoba membuka directory ini pada browser untuk mendapatkan clue tambahan.

![image](https://user-images.githubusercontent.com/88881191/218485780-13bf5396-7206-4723-adc1-0cd056e804ef.png)

Dari clue steghide (follow the r a b b i t) dan hasil dirbuster kita mendapatkan clue untuk membuka directory di browser. Kita mencoba membuka directory /r/ terdapat tulisan “Keep Going”, kita terus memasukkan directory sampai menemukan web dari dir /r/a/b/b/i/t yang berisi header “Open the door and enter wonderland.”

![image](https://user-images.githubusercontent.com/88881191/218485793-6b7acf0a-ad0f-4b9d-8057-ee5a370e5605.png)

![image](https://user-images.githubusercontent.com/88881191/218485807-d394b0c8-c154-4d2a-bd89-da6f5e399b74.png)

Dari kalimat “Open the door and enter wonderland.” Kita mendapatkan sebuah clue yaitu pada kata “open”, disini kita disuruh untuk membuka sesuatu, sehingga kita mencoba untuk melakukan inspect element pada web ini, dan menemukan sebuah informasi menarik. “alice:HowDothTheLittleCrocodileImproveHisShiningTail”
Kita mendapatkan username “alice” dan password “HowDothTheLittleCrocodileImproveHisShiningTail”.

![image](https://user-images.githubusercontent.com/88881191/218487636-de7fc54b-5c46-4f5f-98c5-6452273bccfe.png)

Karena tadi hanya ada 2 port yang terbuka dan port 80 sudah kita buka, maka kita berasumsi bahwa ini adalah credentials ssh untuk port 22, kita akan mencoba login ke ssh port 22.

### Exploitation
Kita menggunakan command ssh alice@10.10.120.136 dan memasukkan password “HowDothTheLittleCrocodileImproveHisShiningTail”, kemudian kita sukses masuk sebagai alice. Kemudian kita menggunakan command ls (list) untuk melihat file yang ada didalamnya. Terdapat 2 file yaitu root.txt dan walrus_and_the_carpenter.py.

![image](https://user-images.githubusercontent.com/88881191/218487659-c66a446c-6e17-44fc-bcee-11d61eff278f.png)

Kita melakukan command ls -la (la = agar bisa menampilkan folder/file yang depannya “.”) dan menemukan beberapa directory dan file. Namun, tidak ada tanda-tanda yang menunjuk ke flag user. Padahal seharusnya ketika kita ls user, kita akan langsung menemukan flag user. Disini terdapat file “root.txt” dan “walrus_and_the_carpenter.py”. Ini merupakan sesuatu yang mencurigakan, karena file “root.txt” biasanya berada didalam akun root. Kita mencoba cat root.txt, kita mendapatkan feedback bahwa Permission denied. Lalu dimanakah flag user?

![image](https://user-images.githubusercontent.com/88881191/218487688-0115c3a0-1d7b-4044-9731-067cc7ff6975.png)

Kita membaca kembali hint di website tryhackme tempat mensubmit flag user, hint menuliskan bahwa “Everything is upside down.”

![image](https://user-images.githubusercontent.com/88881191/218487676-f468d454-3bf3-426c-9320-cd7008207214.png)

Biasanya flag root berada pada directory root dengan nama file root.txt (/root/root.txt). Yang menarik pada machine ini adalah kita menemukan root.txt di home directory ssh. Dari question hint dan keunikan peletakan root.txt, kita menyimpulkan bahwa peletakan posisi flag ditaruh terbalik. Dengan mengetikkan command “cd /root/” (masuk ke directory root), kemudian menggunakan command “cat user.txt” (untuk membaca file user.txt, flag user biasanya disimpan dengan nama “user.txt”). Kita menemukan flag user.

![image](https://user-images.githubusercontent.com/88881191/218487695-fb9c71ef-65bb-4881-815d-738bcdbef1ab.png)

Flag User: thm{“Curiouser and couriouser!”}
