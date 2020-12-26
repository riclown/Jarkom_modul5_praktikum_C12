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

![Img](https://github.com/riclown/Jarkom_modul5_praktikum_C12/blob/main/img/tree.jpg)

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



## Soal 1

## Soal 2

## Soal 3

## Soal 4

## Soal 5

## Soal 6

## Soal 7


## Kendala
