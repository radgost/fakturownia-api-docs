Fakturownia API
===============

Latwo można zingegrować wlasną aplikację lub serwis z systemem Fakturownia


Ponizej opisujemy API dzięki któremu można z innych systemow zarzadzac fakturami, klientami, produktami


Wywolania
---------

Wywolania zawsze musza dotyczyć adresu Twojej aplikacji, np. https://twojaDomena.fakturownia.pl/
W wywolaniu trzeba zawsze podac login i haslo - takie samo jak jest uzywane do 'zwyklego' dostepu do systemu.

Pobranie listy faktur
---------------------

Z aktualnego miesiąca

```shell
curl -u 'login:twoje-haslo'  https://twojaDomena.fakturownia.pl/invoices.json?period=this_month
```

Faktury danego klienta

```shell
curl -u 'login:twoje-haslo'  https://twojaDomena.fakturownia.pl/invoices.json?client_id=ID_KLIENTA
```

Pobranie faktury po ID
----------------------

```shell
curl -u 'login:twoje-haslo'  https://twojaDomena.fakturownia.pl/invoices/100.json
```

PDF


```shell
curl -u 'login:twoje-haslo'  https://twojaDomena.fakturownia.pl/invoices/100.pdf?print_option=original
```

inne opcje PDF:
* print_option=original - Oryginał
* print_option=copy - Kopia
* print_option=original_and_copy - Oryginał i kopia
* print_option=duplicate Duplikat







