2. Plan adresacji

Segment Intnet w VirtualBoxie Podsieć Brama (fw01)
WAN NAT 10.0.2.0/24 (DHCP VirtualBoxa) 10.0.2.2
Serwery intnet-servers 10.10.10.0/24 10.10.10.1
Biuro intnet-office 10.10.20.0/24 10.10.20.1
Zarządzanie intnet-mgmt 10.10.99.0/24 10.10.99.1

Adresy hostów: ctrl01 = 10.10.99.10. Serwery dostaną 10.10.10.x (idm01 .11, app01 .12, mon01 .13, bak01 .14).

3. fw01 (pfSense) ręcznie w VirtualBoxie

Pobierz pfSense CE ISO (AMD64) z netgate.com, rozpakuj .gz. Ustawienia VM:

Typ: BSD / FreeBSD (64-bit), 1 GB RAM, 1-2 CPU, dysk 10 GB.
Cztery karty sieciowe, wszystkie typu Intel PRO/1000 MT Desktop:
Adapter 1: NAT (WAN)
Adapter 2: Internal Network, intnet-servers
Adapter 3: Internal Network, intnet-office
Adapter 4: Internal Network, intnet-mgmt
W adapterach 2-4 ustaw Promiscuous Mode na “Allow All”.

W instalatorze zostaw domyślne opcje. Po restarcie w konsoli:

1. Assign Interfaces, bez VLAN-ów: WAN = em0, LAN = em1, OPT1 = em2, OPT2 = em3.
2. Set interface IP address: LAN 10.10.10.1/24, OPT1 10.10.20.1/24, OPT2 10.10.99.1/24. DHCP serwera na razie wyłącz, adresy statyczne.

Panel WWW będzie pod https://10.10.99.1 (z ctrl01 lub z klienta w mgmt). Pamiętaj, że domyślnie pfSense zezwala na panel tylko z LAN, więc na OPT2 trzeba dodać regułę pass dla ctrl01 (TCP 443, 22), a docelowo zmienisz też domyślne hasło admina.
