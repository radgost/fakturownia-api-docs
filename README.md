# Fakturownia API


Opis jak zintegrować własną aplikację lub serwis z systemem <http://fakturownia.pl/>



Dzięki API można z innych systemów wystawiać faktury/rachunki/paragony oraz zarządzać tymi dokumentami, a także klientami i produktami

## Spis treści
+ [API Token](#token)
+ [Dodatkowe parametry dostępne przy pobieraniu listy rekordów](#list_params)
+ [Faktury - przykłady wywołania](#examples)
	+ [Pobranie listy faktur z aktualnego miesiąca](#f1)
	+ [Faktury danego klienta](#f2)
	+ [Pobranie faktury po ID](#f3)
	+ [Pobranie PDF-a](#f4)
	+ [Wysłanie faktury E-MAILEM do klienta](#f5)
	+ [Dodanie nowej faktury](#f6)
	+ [Dodanie nowej faktury (po ID klienta, produktu, sprzedawcy)](#f7)
	+ [Dodanie nowej faktury korygującej](#f8)
	+ [Aktualizacja faktury](#f9)
	+ [Aktualizacja pozycji na fakturze](#f9b)
	+ [Usunięcie pozycji na fakturze](#f9c)
	+ [Dodanie pozycji na fakturze](#f9d)
	+ [Zmiana statusu faktury](#f10)
	+ [Pobranie listy definicji faktur cyklicznych](#f11)
	+ [Dodanie definicji faktury cyklicznej](#f12)
	+ [Aktualizacja definicji faktury cyklicznej](#f13)
	+ [Usunięcie faktury](#f14)
	+ [Połączenie istniejącej faktury i paragonu](#f15)
+ [Link do podglądu faktury i pobieranie do PDF](#view_url)
+ [Przykłady użycia  - zakup szkolenia](#use_case1)
+ [Faktury - specyfikacja](#invoices)
+ [Klienci](#clients)
	+ [Lista klientów](#k1)
	+ [Wyszukiwanie klientów po nazwie, mailu lub nazwie skróconej](#k1b)
	+ [Pobranie wybranego klienta po ID](#k2)
	+ [Pobranie wybranego klienta po zewnętrznym ID](#k2b)
	+ [Dodanie klienta](#k3)
	+ [Aktualizacja klienta](#k4)
+ [Produkty](#products)
	+ [Lista produktów](#p1)
	+ [Lista produktów ze stanem magazynowym podanego magazynu](#p2)
	+ [Pobranie wybranego produktu po ID](#p3)
	+ [Pobranie wybranego produktu po ID ze stanem magazynowym podanego magazynu](#p4)
	+ [Dodanie produktu](#p5)
	+ [Aktualizacja produktu](#p6)
+ [Dokumenty magazynowe](#warehouse_documents)
	+ [Wszystkie dokumenty magazynowe](#wd1)
	+ [Pobranie wybranego dokumentu po ID](#wd2)
	+ [Dodanie dokumentu magazynowego MM](#wd3a)
	+ [Dodanie dokumentu magazynowego PZ](#wd3)
	+ [Dodanie dokumentu magazynowego WZ](#wd4)
	+ [Dodanie dokumentu magazynowego PZ dla istniejącego klienta, działu i produktu](#wd5)
	+ [Aktualizacja dokumentu](#wd6)
	+ [Usunięcie dokumentu](#wd7)
+ [Kategorie](#categories)
	+ [Lista kategorii](#cat1)
	+ [Pobranie wybranej kategorii po ID](#cat2)
	+ [Dodanie nowej kategorii](#cat3)
	+ [Aktualizacja kategorii](#cat4)
	+ [Usunięcie kategorii o podanym ID](#cat5)
+ [Magazyny](#warehouses)
	+ [Lista magazynów](#wh1)
	+ [Pobranie wybranego magazynu po ID](#wh2)
	+ [Dodanie nowego magazynu](#wh3)
	+ [Aktualizacja magazynu](#cat4)
	+ [Usunięcie magazynu o podanym ID](#cat5)
+ [Działy](#departments)
	+ [Lista działów](#dep1)
	+ [Pobranie wybranego działu po ID](#dep2)
	+ [Dodanie nowego działu](#dep3)
	+ [Aktualizacja_działu](#dep4)
  	+ [Usunięcie działu o podanym ID](#dep5)
+ [Logowanie i pobranie Tokena przez API](#get_token_by_api)
+ [Konta systemowe](#accounts)
+ [Przykłady w PHP i Ruby](#codes)


<a name="token"/>

## API token

`API_TOKEN` token trzeba pobrać z ustawień aplikacji ("Ustawienia -> Ustawienia konta -> Integracja -> Kod autoryzacyjny API")

<a name="list_params"/>

## Dodatkowe parametry dostępne przy pobieraniu listy rekordów
Do wywołań można przekazywać dodatkowe parametry - te same które są używane w aplikacji, np. `page=`, `period=` itp.

Parametr `page=` umożliwia iterowanie po paginowanych rekordach.
Domyślnie przyjmuje wartość `1` i wyświetla pierwsze N rekordów, gdzie N to limit ilości zwracanych rekordów.
Aby uzyskać kolejne N rekordów, należy wywołać akcję z parametrem `page=2`, itd.

Parametr `period=` umożliwia wybranie rekordów z zadanego okresu.
Może przyjąć następujące wartości:
- last_12_months
- this_month
- last_30_days
- last_month
- this_year
- last_year
- all
- more (tutaj trzeba jeszcze dostarczyć dodatkowe parametry date_from (np. "2018-12-16") i date_to (np. "2018-12-21"))

<a name="examples"/>

## Przykłady wywołania

<a name="f1"/>
Pobranie listy faktur z aktualnego miesiąca

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?period=this_month&api_token=API_TOKEN&page=1
```

<a name="f2"/>
Faktury danego klienta

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?client_id=ID_KLIENTA&api_token=API_TOKEN
```

<a name="f3"/>
Pobranie faktury po ID


```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.json?api_token=API_TOKEN
```

<a name="f4"/>
Pobranie PDF-a


```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.pdf?api_token=API_TOKEN
```

<a name="f5"/>
Wysłanie faktury e-mailem do klienta (na e-mail klienta podany przy tworzeniu faktury, pole "buyer_email")


```shell
curl -X POST https://twojaDomena.fakturownia.pl/invoices/100/send_by_email.json?api_token=API_TOKEN
```

inne opcje PDF:
* print_option=original - Oryginał
* print_option=copy - Kopia
* print_option=original_and_copy - Oryginał i kopia
* print_option=duplicate Duplikat


<a name="f6"/>
Dodanie nowej faktury

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
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
            "buyer_email": "buyer@testemail.pl",
            "buyer_tax_no": "5252445767",
            "positions":[
                {"name":"Produkt A1", "tax":23, "total_price_gross":10.23, "quantity":1},
                {"name":"Produkt A2", "tax":0, "total_price_gross":50, "quantity":3}
            ]
        }
    }'
```

<a name="f7"/>
Dodanie nowej faktury - minimalna wersja (tylko pola wymagane), gdy mamy Id produktu (product_id), nabywcy (client_id) i sprzedawcy (department_id) wtedy nie musimy podawać pełnych danych. Opcjonalnie można podać również id odbiorcy (recipient_id).
Zostanie wystawiona Faktura VAT z aktualnym dniem i z 5 dniowym terminem płatności.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "payment_to_kind": 5,
            "client_id": 1,
            "positions":[
                {"product_id": 1, "quantity":2}
            ]
        }}'
```

<a name="f8"/>
Dodanie nowej faktury korygującej

```shell
curl http://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "kind": "correction",
            "from_invoice_id": "2432393,
            "client_id": 1,
            "positions":[
                {"name": "Product A1",
                "quantity":-1,
                "total_price_gross":"-10",
                "tax":"23",
                "kind":"correction",
                "correction_before_attributes": {
                    "name":"Product A1",
                    "quantity":"2",
                    "total_price_gross":"20",
                    "tax":"23",
                    "kind":"correction_before"
                },
                "correction_after_attributes": {
                    "name":"Product A1",
                    "quantity":"1",
                    "total_price_gross":"10",
                    "tax":"23",
                    "kind":"correction_after"
                }
            }]
        }}'
```

<a name="f9"/>
Aktualizacja faktury

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "buyer_name": "Nowa nazwa klienta Sp. z o.o."
        }
    }'
```

<a name="f9b"/>
Aktualizacja pozycji na fakturze - aby edytować pozycję na fakturze, należy podać id pozycji.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"id":32649087, "name":"test"}]
        }
    }'
```

<a name="f9c"/>
Usunięcie pozycji na fakturze - aby usunąć pozycję na fakturze, należy podać id pozycji wraz z parametrem _destroy=1.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"id":32649087, "_destroy": 1}]
        }
    }'
```

<a name="f9d"/>
Dodanie pozycji na fakturze. Pozycja zostanie dopisana jako ostatnia.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"name":"Produkt A1", "tax":23, "total_price_gross":10.23, "quantity":1}]
        }
    }'
```

<a name="f10"/>
Zmiana statusu faktury

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/invoices/111/change_status.json?api_token=API_TOKEN&status=STATUS" -X POST
```

<a name="f11"/>
Pobranie listy definicji faktur cyklicznych

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/recurrings.json?api_token=API_TOKEN
```

<a name="f12"/>
Dodanie definicji faktury cyklicznej

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/recurrings.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "recurring": {
            "name": "Nazwa cyklicznosci",
            "invoice_id": 1,
            "start_date": "2016-01-01",
            "every": "1m",
            "issue_working_day_only": false,
            "send_email": true,
            "buyer_email": "mail1@mail.pl, mail2@mail.pl",
            "end_date": "null"
        }}'
```

<a name="f13"/>
Aktualizacja definicji faktury cyklicznej (zmiana daty wystawienia następnej faktury)

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/recurrings/111.json \
    -X PUT \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "recurring": {
            "next_invoice_date": "2016-02-01"
        }
    }'
```

<a name="f14"/>
Usunięcie faktury

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/invoices/INVOICE_ID.json?api_token=API_TOKEN"
```

<a name="f15"/>
Połączenie istniejącej faktury i paragonu

```shell
curl https://YOUR_DOMAIN.fakturownia.test/invoices/ID_FAKTURY.json \
    -X PUT \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "from_invoice_id": ID_PARAGONU,
            "invoice_id": ID_PARAGONU,
            "exclude_from_stock_level": true
        }
    }'
```

<a name="view_url"/>

## Link do podglądu faktury i pobieranie do PDF

Po pobraniu danych faktury np. przez:

```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.json?api_token=API_TOKEN
```

API zwraca nam m.in. pole `token` na podstawie którego możemy otrzymać linki do podglądu faktury oraz do pobrania PDF-a z wygenrowaną fakturą.
Linki takie umożliwiają odwołanie się do wybranej faktury  bez konieczności logowania - czyli możemy np. te linki przesłać klientowi, który otrzyma dostęp do faktury i PDF-a.

Lini te są postaci:

podgląd: `https://twojaDomena.fakturownia.pl/invoice/{{token}}`
pdf: `https://twojaDomena.fakturownia.pl/invoice/{{token}}.pdf`

Np dla tokenu równego: `HBO3Npx2OzSW79RQL7XV2` publiczny PDF będzie pod adresem `https://twojaDomena.fakturownia.pl/invoice/HBO3Npx2OzSW79RQL7XV2.pdf`

<a name="use_case1"/>

## Przykłady użycia w PHP - zakup szkolenia

`TODO`

Przykład flow Portalu, który generuje dla klienta fakturę Proformę, wysyła ją klientowi i po opłaceniu wysyła do klienta bilet na szkolenie

* Klient wypełnia dane w Portalu
* Portal wywołuje API z fakturownia.pl i tworzy fakturę
* Portal wysyła Klientowi fakturę Proforma w PDF wraz z linkiem do płatności
* Klient opłaca fakturę Proforma (np. na PayPal lub PayU.pl)
* Fakturownia.pl otrzymuje informację, że płatność została wykonana, tworzy Fakturę VAT i wysyła ją Klientowi oraz wywołuje API Portalu
* Po otrzymaniu informacji o płatności (przez API) Portal wysyła Klientowi bilet na Szkolenie


<a name="invoices"/>

## Faktury


* `GET /invoices/1.json` pobranie faktury
* `POST /invoices.json` dodanie nowej faktury
* `PUT /invoices/1.json` aktualizacja faktury
* `DELETE /invoices/1.json` skasowanie faktury


Przykład - dodanie nowej faktury (minimalna wersja, gdy mamy Id produktu, nabywcy i sprzedawcy wtedy nie musimy podawać pełnych danych). Zostanie wystawiona Faktura VAT z aktualnym dniem i z 5 dniowym terminem płatności. Pole department_id określa firmę (lub dział) który wystawia fakturę (można go uzyskać klikając na firmę w menu Ustawienia > Dane firmy)

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "payment_to_kind": 5,
            "client_id": 1,
            "positions":[
                {"product_id": 1, "quantity":2}
            ]
        }}'
```

Pola faktury

```shell
"number" : "13/2012", - numer faktury (jeśli nie będzie podany wygeneruje się automatycznie)
"kind" : "vat", - rodzaj faktury (vat, proforma, bill, receipt, advance, correction, vat_mp, invoice_other, vat_margin, kp, kw, final, estimate)
"income" : "1", - faktura przychodowa (1) lub kosztowa (0)
"issue_date" : "2013-01-16", - data wystawienia
"place" : "Warszawa", - miejsce wystawienia
"sell_date" : "2013-01-16", - data sprzedaży (może być data lub miesiąc postaci 2012-12)
"category_id" : "", - id kategorii
"department_id" : "1", - id działu firmy (w menu Ustawienia > Dane firmy należy kliknąć na firmę/dział i ID działu pojawi się w URL); Jeśli nie będzie tego pola oraz nie będzie pola 'seller_name' wtedy będą wstawione domyślne dane Twojej firmy
"seller_name" : "Radgost Sp. z o.o.", - sprzedawca
"seller_tax_no" : "525-244-57-67", - numer identyfikacji podatkowej sprzedawcy (domyślnie NIP)
"seller_tax_no_kind" : "", - rodzaj numeru identyfikacyjnego sprzedawcy; pole puste (domyślnie) jest interpretowane jako "NIP"; w innym wypadku traktowane jako dowolny wpis własny (np. PESEL, REGON)
"seller_bank_account" : "24 1140 1977 0000 5921 7200 1001", - konto bankowe sprzedawcy
"seller_bank" : "BRE Bank",
"seller_post_code" : "02-548",
"seller_city" : "Warszawa",
"seller_street" : "ul. Olesińska 21",
"seller_country" : "", - kraj sprzedawcy (ISO 3166)
"seller_email" : "platnosci@radgost123.com",
"seller_www" : "",
"seller_fax" : "",
"seller_phone" : "",
"client_id" : "-1" - id kupującego (jeśi -1 to klient zostanie utworzony w systemie)
"buyer_name" : "Nazwa klienta" - nabywca
"buyer_tax_no" : "525-244-57-67", - numer identyfikacji podatkowej nabywcy (domyślnie NIP)
"buyer_tax_no_kind" : "", - rodzaj numeru identyfikacyjnego nabywcy; pole puste (domyślnie) jest interpretowane jako "NIP"; w innym wypadku traktowane jako wpis własny (np. PESEL, REGON)
"disable_tax_no_validation" : "",
"buyer_post_code" : "30-314", - kod pocztowy nabywcy
"buyer_city" : "Warszawa", - miasto nabywcy
"buyer_street" : "Nowa 44", - ulica nabywcy
"buyer_country" : "PL", - kraj nabywcy (ISO 3166)
"buyer_note" : "", - dodatkowy opis nabywcy
"buyer_email" : "", - email nabywcy
"recipient_id" : "", - id odbiorcy (id klienta z systemu)
"recipient_name" : "", - nazwa odbiorcy
"recipient_street" : "", - ulica odbiorcy
"recipient_post_code" : "", - kod pocztowy odbiorcy
"recipient_city" : "", - miasto odbiorcy
"recipient_country" : "", - kraj odbiorcy (ISO 3166)
"recipient_email" : "", - e-mail odbiorcy
"recipient_phone" : "", - numer telefonu odbiorcy
"recipient_note" : "", - dodatkowy opis odbiorcy
"additional_info" : "0" - czy wyświetlać dodatkowe pole na pozycjach faktury
"additional_info_desc" : "PKWiU" - nazwa dodatkowej kolumny na pozycjach faktury
"show_discount" : "0" - czy rabat
"payment_type" : "transfer",
"payment_to_kind" : termin płatności. gdy jest tu "other_date", wtedy można określić konkretną datę w polu "payment_to", jeśli jest tu liczba np. 5 to wtedy mamy 5 dniowy okres płatności
"payment_to" : "2013-01-16",
"status" : "issued",
"paid" : "0,00",
"oid" : "zamowienie10021", - numer zamówienia (np z zewnętrznego systemu zamówień)
"oid_unique" : jeśli to pole będzie ustawione na 'yes' wtedy nie system nie pozwoli stworzyc 2 faktur o takim samym OID (może to być przydatne w synchronizacji ze sklepem internetowym)
"warehouse_id" : "1090",
"seller_person" : "Imie Nazwisko",
"buyer_first_name" : "Imie",
"buyer_last_name" : "Nazwisko",
"paid_date" : "",
"currency" : "PLN",
"lang" : "pl",
"exchange_currency" : "", - przeliczona waluta (przeliczanie sumy i podatku na inną walutę), np. "PLN"
"exchange_kind" : "", - źródło kursu do przeliczenia waluty ("ecb", "nbp", "cbr", "nbu", "nbg", "own")
"exchange_currency_rate" : "", - własny kurs przeliczenia waluty (używany, gdy parametr exchange_kind ustawiony jest na "own")
"internal_note" : "",
"invoice_template_id" : "1",
"description" : "- opis faktury",
"description_footer" : "", - opis umieszczony w stopce faktury
"description_long" : "", - opis umieszczony na odwrocie faktury
"invoice_id" : "" - pole z id powiązanego dokumentu, np. id zamówienia przy zaliczce albo id wzorcowej faktury przy fakturze cyklicznej,
"from_invoice_id" : "" - id faktury na podstawie której faktura została wygenerowana (przydatne np. w przypadku generacji Faktura VAT z Faktury Proforma),
"delivery_date" : "" - data wpłynięcia dokumentu (tylko przy wydatkach),
"buyer_company" : "1" - czy klient jest firmą
"additional_invoice_field" : "" - wartość dodatkowego pola na fakturze, Ustawienia > Ustawienia Konta > Konfiguracja > Faktury i dokumenty > Dodatkowe pole na fakturze
"internal_note" : "" - treść notatki prywatnej na fakturze, niewidoczna na wydruku.
"positions":
   		"product_id" : "1",
   		"name" : "Fakturownia Start",
   		"additional_info" : "", - dodatkowa informacja na pozycji faktury (np. PKWiU)
   		"discount_percent" : "", - zniżka procentowa (uwaga: aby rabat był wyliczany trzeba ustawić pole: 'show_discount' na '1' oraz przed wywołaniem należy sprawdzić czy w Ustawieniach Konta pole: "Jak obliczać rabat" ustawione jest na "procentowo")
   		"discount" : "", - zniżka kwotowa (uwaga: aby rabat był wyliczany trzeba ustawić pole: 'show_discount' na 1 oraz przed wywołaniem należy sprawdzić czy w Ustawieniach Konta pole: "Jak obliczać rabat" ustawione jest na "kwotowo")
   		"quantity" : "1",
   		"quantity_unit" : "szt",
   		"price_net" : "59,00", - jeśli nie jest podana to zostanie wyliczona
   		"tax" : "23",
   		"price_gross" : "72,57", - jeśli nie jest podana to zostanie wyliczona
   		"total_price_net" : "59,00", - jeśli nie jest podana to zostanie wyliczona
   		"total_price_gross" : "72,57",
   		"code" : "" - kod produktu
"calculating_strategy" =>
{
  "position": "default" lub "keep_gross" - metoda wyliczania kwot na pozycjach faktury
  "sum": "sum" lub "keep_gross" lub "keep_net" - metoda sumowania kwot z pozycji
  "invoice_form_price_kind": "net" lub "gross" - cena jednostkowa na formatce faktury
}
```

Wartości pól

Pole: `kind`
```shell
	"vat" - faktura VAT
	"proforma" - faktura Proforma
	"bill" - rachunek
	"receipt" - paragon
	"advance" - faktura zaliczkowa
	"final" - faktura końcowa
	"correction" - faktura korekta
	"vat_mp" - faktura MP
	"invoice_other" - inna faktura
	"vat_margin" - faktura marża
	"kp" - kasa przyjmie
	"kw" - kasa wyda
	"estimate" - zamówienie
	"vat_mp" - faktura MP
	"vat_rr" - faktura RR
	"correction_note" - nota korygująca
	"accounting_note" - nota księgowa
	"client_order" - własny dokument nieksięgowy
	"dw" - dowód wewnętrzny
	"wnt" - Wewnątrzwspólnotowe Nabycie Towarów
	"wdt" - Wewnątrzwspólnotowa Dostawa Towarów
	"import_service" - import usług
	"import_service_eu" - import usług z UE
	"import_products" - import towarów - procedura uproszczona
	"export_products" - eksport towarów
```

Pole: `lang`
```shell
	"pl" - faktura w języku polskim
	"en" - język angielski
	"en-GB" - język angielski UK
	"de" - język niemiecki
	"fr" - język francuski
	"cz" - język czeski
	"ru" - język rosyjski
	"es" - język hiszpański
	"it" - język włoski
	"nl" - język niderlandzki
	"hr" - język chorwacki
	"ar" - język arabski
	"sk" - język słowacki
	"sl" - język słoweński
	"he" - język grecki
	"et" - język estoński
	"cn" - język chiński
	"hu" - język węgierski
	"tr" - język turecki
	"fa" - język perski

	można tworzyć faktury dwujęzyczne łącząc symbole dwóch języków przy pomocy ukośnika, np:
	"pl/en" - język polski i angielski
```


Pole: `income`
```shell
	"1" - fakura przychodwa
	"0" - faktura kosztowa
```

Pole: `payment_type`
```shell
	"transfer" - przelew
	"card" - karta płatnicza
	"cash" -  gotówka
	"barter" - barter
	"cheque" - czek
	"bill_of_exchange" - weksel
	"cash_on_delivery" - opłata za pobraniem
	"compensation" - kompensata
	"letter_of_credit" - akredytywa
	"payu" - PayU
	"paypal" - PayPal
	"off" - "nie wyświetlaj"
	"dowolny_inny_wpis_tekstowy"
```

Pole: `status`
```shell
	"issued" - wystawiona
	"sent" - wysłana
	"paid" - opłacona
	"partial" - częściowo opłacona
	"rejected" - odrzucona
```

Pole: `discount_kind` - rodzaj rabatu
```shell
	"percent_unit" - liczony od ceny jednostkowej
	"percent_total" - liczony od ceny całkowitej
	"amount" - kwotowy
```


<a name="clients"/>

## Klienci

<a name="k1"/>
Lista klientów

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&page=1"
```

<a name="k1b"/>
Wyszukiwanie klientów po nazwie, mailu lub nazwie skróconej

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&name=CLIENT_NAME"
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&email=EMAIL_ADDRESS"
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&shortcut=SHORT_NAME"
```

<a name="k2"/>
Pobranie wybranego klienta po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients/100.json?api_token=API_TOKEN"
```

<a name="k2b"/>
Pobranie wybranego klienta po zewnętrznym ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?external_id=100&api_token=API_TOKEN"
```

<a name="k3"/>
Dodanie klienta

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/clients.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "client": {
            "name": "Klient1",
            "tax_no": "5252445767",
            "bank" : "bank1",
            "bank_account" : "bank_account1",
            "city" : "city1",
            "country" : "",
            "email" : "email@gmail.com",
            "person" : "person1",
            "post_code" : "post-code1",
            "phone" : "phone1",
            "street" : "street1"
        }}'
```

<a name="k4"/>
Aktualizacja klienta

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/clients/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "client": {
            "name": "Klient2",
            "tax_no": "52524457672",
            "bank" : "bank2",
            "bank_account" : "bank_account2",
            "city" : "city2",
            "country" : "PL",
            "email" : "email@gmail.com",
            "person" : "person2",
            "post_code" : "post-code2",
            "phone" : "phone2",
            "street" : "street2"
        }}'
```


<a name="products"/>

## Produkty

<a name="p1"/>
Lista produktów

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products.json?api_token=API_TOKEN&page=1"
```

<a name="p2"/>
Lista produktów ze stanem magazynowym podanego magazynu

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products.json?api_token=API_TOKEN&warehouse_id=WAREHOUSE_ID&page=1"
```

<a name="p3"/>
Pobranie wybranego produktu po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products/100.json?api_token=API_TOKEN"
```

<a name="p4"/>
Pobranie wybranego produktu po ID ze stanem magazynowym podanego magazynu

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products/100.json?api_token=API_TOKEN&warehouse_id=WAREHOUSE_ID"
```

<a name="p5"/>
Dodanie produktu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/products.json \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "product": {
            "name": "PoroductAA",
            "code": "A001",
            "price_net": "100",
            "tax": "23"
        }}'
```

<a name="p6"/>
Aktualizacja produktu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/products/333.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "product": {
            "name": "PoroductAA2",
            "code": "A0012",
            "price_gross": "102",
	    "tax": "23"
        }}'
```
**Uwaga:** Cenna netto jest wyliczana na podstawie wartości ceny brutto oraz podatku, nie można jej edytować wprost przez API.



<a name="warehouse_documents"/>

## Dokumenty magazynowe

<a name="wd1"/>
Wszystkie dokumenty magazynowe

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json?api_token=API_TOKEN"
```
można przekazywać takie same parametry jakie są przekazywane w aplikacji (na stronie listy faktur)

<a name="wd2"/>
Pobranie wybranego dokumentu po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/555.json?api_token=API_TOKEN"
```

<a name="wd3a"/>
Dodanie dokumentu magazynowego MM

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "warehouse_document": {
                    "kind":"mm",
                    "number": null,
                    "warehouse_id": "1",
                    "issue_date": "2017-10-23",
                    "department_name": "Department1 SA",
                    "client_name": "Client1 SA",
                    "warehouse_actions":[
                        {"product_name":"Produkt A1", "purchase_tax":23, "purchase_price_net":10.23, "quantity":1, "warehouse2_id":13}
                    ]
                }}'
```

<a name="wd3"/>
Dodanie dokumentu magazynowego PZ

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"pz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_name": "Department1 SA",
					"client_name": "Client1 SA",
					"warehouse_actions":[
						{"product_name":"Produkt A1", "purchase_tax":23, "purchase_price_net":10.23, "quantity":1},
						{"product_name":"Produkt A2", "purchase_tax":0, "purchase_price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd4"/>
Dodanie dokumentu magazynowego WZ

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"wz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_name": "Department1 SA",
					"client_name": "Client1 SA",
					"warehouse_actions":[
						{"product_id":"333", "tax":23, "price_net":10.23, "quantity":1},
						{"product_id":"333", "tax":0, "price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd5"/>
Dodanie dokumentu magazynowego PZ dla istniejącego klienta, działu i produktu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"pz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_id": "222",
					"client_id": "111",
					"warehouse_actions":[
						{"product_id":"333", "purchase_tax":23, "price_net":10.23, "quantity":1},
						{"product_id":"333", "purchase_tax":0, "price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd6"/>
Aktualizacja dokumentu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/555.json
			 	-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"warehouse_document": {
						"client_name": "New client name SA"
				    }}'
```

<a name="wd7"/>
Usunięcie dokumentu

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/100.json?api_token=API_TOKEN"
```


<a name="categories"/>

## Kategorie

<a name="cat1"/>
Lista wszystkich kategorii

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/categories.json?api_token=API_TOKEN"
```

<a name="cat2"/>
Pobranie pojedycznej kategorii po ID

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/categories/100.json?api_token=API_TOKEN"
```

<a name="cat3"/>
Dodanie nowej kategorii

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/categories.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"category": {
					"name":"my_category",
					"description": null
				}}'
```

<a name="cat4"/>
Aktualizacja kategorii

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/categories/100.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"category": {
					"name":"my_category",
					"description": "new_description"
				}}'
```


<a name="cat5"/>
Usunięcie kategorii o podanym ID

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/categories/100.json?api_token=API_TOKEN"
```

<a name="warehouses"/>

## Magazyny

<a name="wh1"/>
Lista wszystkich magazynów

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/warehouses.json?api_token=API_TOKEN"
```

<a name="wh2"/>
Pobranie pojedycznego magazynu po ID

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/warehouses/100.json?api_token=API_TOKEN"
```

<a name="wh3"/>
Dodanie nowego magazynu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouses.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse": {
					"name":"my_warehouse",
					"kind": null,
					"description": null
				}}'
```

<a name="wh4"/>
Aktualizacja magazynu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouses/100.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse": {
					"name":"my_category",
					"kind": null,
					"description": "new_description"
				}}'
```


<a name="wh5"/>
Usunięcie magazynu o podanym ID

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/warehouses/100.json?api_token=API_TOKEN"
```


<a name="departments"/>

## Działy

<a name="dep1"/>
Lista wszystkich działów

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/departments.json?api_token=API_TOKEN"
```

<a name="dep2"/>
Pobranie pojedycznego działu po ID

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/departments/100.json?api_token=API_TOKEN"
```

<a name="dep3"/>
Dodanie nowego działu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/departments.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"department": {
					"name":"my_warehouse",
					"shortcut": "short_name",
					"tax_no": "-"
				}}'
```

<a name="dep4"/>
Aktualizacja działu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/departments/100.json
        -X PUT
        -H 'Accept: application/json'
        -H 'Content-Type: application/json'
        -d '{
        "api_token": "API_TOKEN",
        "department": {
          "name":"new_name",
          "shortcut": "new_short_name",
          "tax_no": "xxx-xxx-xx-xx"
        }}'
```

<a name="dep5"/>
Usunięcie działu o podanym ID

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/departments/100.json?api_token=API_TOKEN"
```


<a name="get_token_by_api"/>

## Logowanie i pobranie tokena przez API

```shell
curl https://app.fakturownia.pl/login.json \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
            "login": "login_or_email",
            "password": "password"
    }'
```

To zapytanie zwraca token i informacje o URL konta w Fakturowni (pola `prefix` i `url`):

```shell
{
	"login":"marcin",
	"email":"email@test.pl",
	"prefix":"YYYYYYY",
	"url":"https://YYYYYYY.fakturownia.pl",
	"first_name":"Jan",
	"last_name":"Kowalski",
	"api_token":"XXXXXXXXXXXXXX"
}
```

<a name="accounts"/>

## Konta Systemowe

Jest to opcja dla Partnerów, którzy chcą zakładać konta Fakturowni z poziomu swojej aplikacji. Np. mogą to być
dostawcy sklepów internetowych, systemów rezerwacji itp lub innych systemów którzy chcą udostępnić swoim użytkownikom funkcjonalność wystawiania faktur.

Klient w portalu Partnera jednym przyciskiem może założyć konto i od razu zacząć wystawiać faktury (nie musi samodzielnie zakładać konta w Fakturownia.pl)

Pola: user.login, user.from_partner, user, company nie są wymagane

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/account.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
            "api_token": "API_TOKEN",
            "account": {
                "prefix": "prefix1",
		"lang": "pl"
            },
            "user": {
                "login": "login1",
                "email": "email1@email.pl",
                "password": "password1",
                "from_partner": "PARTNER_CODE"
            },
            "company": {
                "name": "Company1",
                "tax_no": "5252445700",
                "post_code": "00-112",
                "city": "Warsaw",
                "street": "Street 1/10",
                "person": "Jan Nowak",
                "bank": "Bank1",
                "bank_account": "111222333444555666111"
            }
        }'

```

Po utworzeniu konta zwracane są:

```shell

{
	"prefix":"prefix126", - prefix utworzonego konta (moze byc innny niz podany, gdy podany już istniał)
	"api_token":"62YPJfIekoo111111", - kod dostepu do utworzonego konta
	"url":"https://prefix126.fakturownia.pl", - url utworzonego konta
	"login":"login1", - login użytkownika  (moze byc innny niz podany, gdy podany już istniał)
	"email":"email1@email.pl"
}
```

Inne pola dostępne przy tworzeniu nowego konta (pomocne przy integracji)

```shell
	"account": {
		"prefix": "prefix-konta",
		"lang": "pl",
		"integration_fast_login": true - umożliwia automatyczne logowanie Twoich użytkowników w Fakturowni
		"integration_logout_url": "http://twojastrona.pl/" - umożliwia powrót Twoich użytkowników na Twoją stronę po ich wylogowaniu się z Fakturowni
	}
```

Pobranie informacji o koncie:

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/account.json?api_token=API_TOKEN"
```

<a name="codes"/>

## Przykłady w PHP i Ruby

<https://github.com/radgost/fakturownia-api/blob/master/example1.php/>

<https://github.com/radgost/fakturownia-api/blob/master/example1.rb/>

Ruby Gem do integracji z Fakturownia.pl: <https://github.com/kkempin/fakturownia/>
