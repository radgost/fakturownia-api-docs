Fakturownia API
===============

Latwo można zingegrować wlasną aplikację lub serwis z systemem Fakturownia


Ponizej opisujemy API dzięki któremu można z innych systemow zarzadzac fakturami, klientami, produktami


API token
---------

`API_TOKEN` token trzeba pobrać z ustawień aplikacji ("Ustawienia -> Ustawienia konta -> Integracja -> Kod autoryzacyjny API")

Przykłady wywołania
----------------

Pobranie listy faktur z aktualnego miesiąca

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?period=this_month&api_token=API_TOKEN
```

Faktury danego klienta

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?client_id=ID_KLIENTA&api_token=API_TOKEN
```

Pobranie faktury po ID


```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.json&api_token=API_TOKEN
```

Pobranie PDF-a


```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.pdf?api_token=API_TOKEN
```

inne opcje PDF:
* print_option=original - Oryginał
* print_option=copy - Kopia
* print_option=original_and_copy - Oryginał i kopia
* print_option=duplicate Duplikat


Dodanie nowej faktury

```shell
curl http://YOUR_DOMAIN.fakturownia.pl/invoices.json 
  -H 'Accept: application/json'  
	-H 'Content-Type: application/json'  
	-d '{
	  	"api_token": "API_TOKEN",
	  	"invoice": {
			"kind":"vat", 
			"number": null, 
			"sell_date": "2013-01-16", 
			"issue_date": "2013-01-16", 
			"payment_to": "2013-01-23",
			"seller_name": "Wystawca Sp. z o.o.", 
			"seller_tax_no": "5252445767", 
			"buyer_name": "Klient1 Sp. z o.o.",
			"buyer_tax_no": "5252445767",
			"positions":[
				{"name":"Produkt A1", "tax":23, "total_price_gross":10.23, "quantity":1},
				{"name":"Produkt A2", "tax":0, "total_price_gross":50, "quantity":3}
			]		
		}
	}'
```

Aktualizacja faktury

```shell
TODO
```

Przykłady użycia: Zakup Szkolenia
----------------

Przykład flow Portalu, który generuje dla klienta fakturę Proformę, wysyła ją klientowi i po opłaceniu wysyła do klienta bilet na szkolenie

* Klient wypełnia dane w Portalu
* Portal wywołuje API z fakturownia.pl i tworzy fakturę
* Prtal pobiera wysyła Klientowi fakturę Proforma w PDF wraz z linkiem do płatności
* Klient opłaca fakturę Proforma (np. na PayPal lub PayU.pl)
* Fakturownia.pl otrzymuje informację, że płatność została wykonana, tworzy Fakturę VAT i wysyła ją Klientowi oraz wywołuje API Portalu
* Po otrzymaniu informacji o płatności (przez API) Portal wysyła Klientowi bilet na Szkolenie


Faktury
-------


* `GET /invoices/1.json` pobranie faktury
* `POST /invoices.json` dodanie nowej faktury
* `PUT /invoices/1.json` aktualizacja faktury
* `DELETE /invoices/1.json` skasowanie faktury
 
Pola faktury

```shell
"number" : "13/2012", 
"kind" : "vat", 
"income" : "1", 
"additional_info_desc" : "PKWiU", 
"from_invoice_id" : "746977", 
"tax2_visible" : "false", 
"client_id" : "-1", 
"pattern" : "yy/mm/dd/nr-d", 
"issue_date" : "2013-01-16", 
"place" : "", 
"sell_date" : "2013-01-16", 
"category_id" : "", 
"department_id" : "1", 
"seller_name" : "Radgost Sp. z o.o.", 
"seller_tax_no" : "525-244-57-67", 
"seller_bank_account" : "24 1140 1977 0000 5921 7200 1001", 
"seller_bank" : "BRE Bank", 
"seller_post_code" : "02-548", 
"seller_city" : "Warszawa", 
"seller_street" : "ul. Olesińska 21", 
"seller_country" : "", 
"seller_email" : "platnosci@radgost123.com", 
"seller_www" : "", 
"seller_fax" : "", 
"seller_phone" : "", 
"buyer_name" : "Nazwa klienta", 
"buyer_tax_no" : "525-244-57-67", 
"disable_tax_no_validation" : "", 
"buyer_post_code" : "30-314", 
"buyer_city" : "Warszawa", 
"buyer_street" : "Nowa 44", 
"buyer_country" : "", 
"buyer_note" : "", 
"buyer_email" : "", 
"additional_info" : "0", 
"show_discount" : "0", 
"payment_type" : "transfer", 
"payment_to_kind" : "other_date", 
"payment_to" : "2013-01-16", 
"status" : "issued", 
"paid" : "0,00", 
"oid" : "zamoweieni123", 
"discount_kind" : "percent_unit", 
"warehouse_id" : "1090", 
"seller_person" : "Imie Nazwisko", 
"buyer_first_name" : "Imie", 
"buyer_last_name" : "Nazwisko", 
"description" : "", 
"paid_date" : "", 
"currency" : "PLN", 
"lang" : "pl", 
"exchange_currency" : "", 
"internal_note" : "", 
"invoice_template_id" : "1", 
"description_footer" : "", 
"description_long" : "", 
"positions":
   		"product_id" : "1", 
   		"name" : "Fakturownia Start", 
   		"additional_info" : "", 
   		"discount_percent" : "", 
   		"discount" : "", 
   		"quantity" : "1", 
   		"quantity_unit" : "szt", 
   		"price_net" : "59,00", 
   		"tax" : "23", 
   		"price_gross" : "72,57", 
   		"total_price_net" : "59,00", 
   		"total_price_gross" : "72,57"
```








