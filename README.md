Fakturownia API
===============

Latwo można zingegrować wlasną aplikację lub serwis z systemem Fakturownia


Ponizej opisujemy API dzięki któremu można z innych systemow zarzadzac fakturami, klientami, produktami


Wywolania
---------

Wywolania zawsze musza dotyczyć adresu Twojej aplikacji, np. https://twojaDomena.fakturownia.pl/
W wywolaniu trzeba zawsze podac login i haslo - takie samo jak jest uzywane do 'zwyklego' dostepu do systemu.

```shell
curl -u 'twoj-login:twoje-haslo'  http://twojaDomena.fakturownia.pl/invoices.json?period=this_month
```





