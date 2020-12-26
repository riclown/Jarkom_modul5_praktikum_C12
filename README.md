# Jarkom_modul5_praktikum_C12

## Kelompok C12
* 05111840000146 - [I Kadek Ricky Suirta](https://github.com/riclown)
* 05111840000162 - [Fransiskus Xaverius Kevin Koesnadi](https://github.com/fxkevink)

## Daftar Isi
* [Pembuatan Topologi](#topologi)
* [Soal 1](#soal-1)
* [Soal 2](#soal-2)
* [Soal 3](#soal-3)
* [Soal 4](#soal-4)
* [Soal 5](#soal-5)
* [Soal 6](#soal-6)
* [Soal 7](#soal-7)


## Topologi

**(A)** Membuat topologi jaringan sesuai dengan rancangan yang diberikan pada gambar di bawah ini:

![Img](https://github.com/riclown/Jarkom_modul5_praktikum_C12/blob/main/img/topol.jpg)

Keterangan:

* **SURABAYA** diberikan IP Tuntap

* **MALANG** merupakan **DNS Server** diberikan **IP DMZ**

* **MOJOKERTO** merupakan **DHCP Server** diberikan **IP DMZ**

* **MADIUN** dan **PROBOLINGGO** merupakan **WEB Server**

* **Setiap Server** diberikan memory sebesar **128M**

* **Client** dan **Router** diberikan memori sebesar **96M**

* Jumlah host pada subnet **SIDOARJO 200 Host**

* Jumlah host pada subnet **GRESIK 210 Host**

**(B)** Melakukan subnetting dengan VLSM:

![Img](https://github.com/riclown/Jarkom_modul5_praktikum_C12/blob/main/img/subnetting.jpg)

Pembuatan Tree:

![Img](https://github.com/riclown/Jarkom_modul5_praktikum_C12/blob/main/img/tree.png)

 ________ ___________ _________ 
| Subnet |    Host   | Length  |
|--------|-----------|---------|
| A1     | SERVER    | 29      |
| A2     |  201      | 24      |
| A3     | 2         | 30      |
| A4     | 2         | 30      |
| A5     | 211       | 24      |
| A6     |  3        | 30      | 
|Total   | 419       | 23      |
--------------------------------
 ________ ___________ _________ ______________ ________________ ______________ 
| Subnet | Jumlah IP | Submask |      NID     |     Netmask    | Broadcast ID |
|--------|-----------|---------|--------------|----------------|--------------|
| A1     | SERVER    | 29      |10.151.77.104 |255.255.255.248 |10.151.77.111 |
| A2     |  200      | 24      |192.168.1.0   |255.255.255.0   |192.168.1.255 |
| A3     | 2         | 30      |192.168.0.0   |255.255.255.252 |192.168.0.3   |
| A4     | 2         | 30      |192.168.0.4   |255.255.255.252 |192.168.0.7   |
| A5     | 210       | 24      |192.168.2.0   |255.255.255.0   |192.168.2.255 |
| A6     | SERVER    | 29      |192.168.0.8   |255.255.255.248 |192.168.0.15  |
-------------------------------------------------------------------------------

**(C)** Melakukan Routing

Sintaks untuk **topo.sh**

```
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.76.53 eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

# Server switch 2
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Klien switch 1
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &

# Klien switch 3
xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
```
![Img](https://github.com/riclown/Jarkom_modul5_praktikum_C12/blob/main/img/topo.jpg)

Lalu  jalankan `bash topo.sh` pada *putty* dan masukkan *username* dan *password* default. Pada router **SURABAYA**, **BATU**, **KEDIRI**  lakukan setting `sysctl` dengan mengetikkan perintah `nano /etc/sysctl.conf`, dan tambahkan `net.ipv4.conf.all.accept_source_route = 1`.

Lalu setting IP pada setiap *interfaces* uml dengan mengetikkan `nano /etc/network/interfaces` sebagai berikut:

**SURABAYA**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.76.54
netmask 255.255.255.252
gateway 10.151.76.53

auto eth1 #switch 5
iface eth1 inet static
address   192.168.0.1
netmask 255.255.255.252

auto eth2 #switch 6
iface eth2 inet static
address 192.168.0.5
netmask 255.255.255.252

```

**BATU**

```
auto lo
iface lo inet loopback

auto eth0 #switch 5
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252 
gateway 192.168.0.1

auto eth1 #switch 3
iface eth1 inet static
address 192.168.1.1
netmask 255.255.255.0

auto eth2 #switch 2
iface eth2 inet static
address 10.151.77.105
netmask 255.255.255.248

```

**KEDIRI**

```
auto lo
iface lo inet loopback

auto eth0 #switch 6
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1 #switch 4
iface eth1 inet static
address 192.168.2.1
netmask 255.255.255.0

auto eth2 #switch 1
iface eth2 inet static
address 192.168.0.9
netmask 255.255.255.248
```

**MALANG**

```
auto lo
iface lo inet loopback

auto eth0 #switch 2
iface eth0 inet static
address 10.151.77.106
netmask 255.255.255.248
gateway 10.151.77.105
```

**MOJOKERTO**

```
auto lo
iface lo inet loopback

auto eth0 #switch 2
iface eth0 inet static
address 10.151.77.107
netmask 255.255.255.248
gateway 10.151.77.105
```

**PROBOLINGGO**

```
auto lo
iface lo inet loopback

auto eth0 #switch 1
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9
```

**MADIUN**

```
auto lo
iface lo inet loopback

auto eth0 #switch 1
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9
```

**SIDOARJO**

```
auto lo
iface lo inet loopback

auto eth0 #switch 3
iface eth0 inet static
address 192.168.1.2
netmask 255.255.255.0
gateway 192.168.1.1
```

**GRESIK**

```
auto lo
iface lo inet loopback

auto eth0 #switch 4
iface eth0 inet static
address 192.168.2.2
netmask 255.255.255.0
gateway 192.168.2.1
```

Lakukan proses routing dari **SURABAYA** ke subnet yang tidak berhubungan secara langsung dengan membuat script yang bernama `rute.sh` di **SURABAYA**, dan masukkan sintaks berikut:

```
route add -net 10.151.77.104 netmask 255.255.255.248 gw 192.168.0.2 #A1
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.2 #A2
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.6 #A5
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.6 #A6
```

Jalannkan script `rute.sh` di **SURABAYA** dan restart network dengan mengetikkan `service networking restart` di setiap UML. Untuk menguji koneksi antara subnet, terlebih dahulu diuji coba dengan mengetikkan `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` pada router **SURABAYA**. IP Tables dapat dimasukkan ke dalam *script*, dalam hal ini, nama script berupa `table.sh`.


**(D)** Memberikan IP pada subnet **SIDOARJO** dan **GRESIK** secara dinamis menggunakan DHCP SERVER



## Soal 1

## Soal 2

## Soal 3

## Soal 4

## Soal 5

## Soal 6

## Soal 7


## Kendala
