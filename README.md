# inFakt MCP

> Połącz asystenta AI z kontem inFakt. Pytaj po polsku o faktury, koszty, klientów i dane księgowe. Przed utworzeniem dokumentu lub wysyłką sprawdź i zatwierdź dane.

inFakt MCP is a hosted Model Context Protocol server for Polish invoicing and accounting. Connect through the inFakt plugin in ChatGPT or add the server URL to another MCP-compatible assistant. Sign in through OAuth to retrieve invoices, expenses, customer data, tax records and KSeF integration status. Write operations require confirmation and depend on the server configuration and your permissions.

To repozytorium zawiera dokumentację połączenia z usługą inFakt MCP. Nie musisz instalować ani uruchamiać własnego serwera.

| Informacja | Wartość |
| --- | --- |
| Usługa | inFakt MCP |
| Dostawca | [inFakt](https://www.infakt.pl/) |
| Połączenie w ChatGPT | Wtyczka inFakt w katalogu aplikacji lub wtyczek |
| Adres serwera MCP | `https://mcp.infakt.pl/mcp` |
| Transport | Streamable HTTP przez HTTPS |
| Uwierzytelnianie | OAuth z PKCE, logowanie do inFakt w przeglądarce |
| Wymagania | Konto inFakt oraz klient obsługujący zdalne MCP i OAuth |
| Dokumentacja | [infakt/infakt-mcp](https://github.com/infakt/infakt-mcp) |
| Skrócony indeks dla agentów | [llms.txt](llms.txt) |

## Co możesz zrobić

Poproś asystenta o wyszukanie dokumentów lub przygotowanie zestawienia na podstawie danych z Twojego konta inFakt.

| Obszar | Dostępne operacje odczytu | Przykładowe narzędzia MCP |
| --- | --- | --- |
| Faktury | Lista i szczegóły faktur | `infakt_get_invoices_list`, `infakt_get_invoice` |
| Koszty | Lista i szczegóły dokumentów kosztowych | `infakt_get_costs_list`, `infakt_get_cost` |
| Klienci i produkty | Wyszukiwanie klientów oraz przegląd produktów i usług | `infakt_get_clients_list`, `infakt_get_products_list` |
| Księgowość | Księgi, raporty fiskalne i dowody wewnętrzne | `infakt_get_books_list`, `infakt_get_fiscal_reports_list`, `infakt_get_internal_evidences_list` |
| Podatki i ZUS | Podatki dochodowe, składki, VAT-UE i zwolnienia z VAT | `infakt_get_income_taxes_list`, `infakt_get_insurance_fees_list`, `infakt_get_vat_eu_taxes_list`, `infakt_get_vat_exemptions_list` |
| Stawki podatkowe | Stawki VAT, ryczałtu i MOSS/OSS | `infakt_get_rates_list` |
| JPK i KSeF | Pliki JPK i status integracji z Krajowym Systemem e-Faktur | `infakt_get_jpk_files_list`, `infakt_get_jpk_file`, `infakt_get_ksef_integration_status` |
| Konto i rachunki bankowe | Dane konta, historia aktywności i rachunki bankowe | `infakt_get_account_details`, `infakt_get_account_activities_list`, `infakt_get_bank_accounts_list` |

### Operacje zapisu

Narzędzia zapisu są dostępne, gdy serwer je udostępnia, a Twoje konto ma odpowiednie uprawnienia. Po połączeniu sprawdź listę dostępnych narzędzi.

| Operacja | Narzędzie MCP |
| --- | --- |
| Utworzenie faktury | `infakt_invoice_create` |
| Utworzenie faktury na podstawie istniejącej | `infakt_invoice_create_similar` |
| Utworzenie faktury korygującej lub OSS | `infakt_corrective_invoice_create`, `infakt_oss_invoice_create` |
| Dodanie klienta lub produktu | `infakt_create_client`, `infakt_product_create` |
| Oznaczenie faktury lub kosztów jako opłaconych | `infakt_mark_invoice_as_paid`, `infakt_costs_mark_paid` |
| Wysłanie faktury e-mailem | `infakt_send_invoice_email` |
| Wysłanie faktury do KSeF | `infakt_send_invoice_to_ksef` |

Przed zapisem asystent pokazuje podgląd i czeka na Twoje potwierdzenie. Podgląd nie tworzy ani nie wysyła dokumentu.

## Jak się połączyć

### ChatGPT: wtyczka inFakt

W ChatGPT połącz konto przez wtyczkę inFakt.

1. Otwórz katalog aplikacji lub wtyczek w ChatGPT i wyszukaj `inFakt`.
2. Wybierz inFakt i rozpocznij instalację lub połączenie konta.
3. Zaloguj się do inFakt w przeglądarce i sprawdź żądane uprawnienia.
4. Po autoryzacji wybierz inFakt w rozmowie i poproś o pobranie danych, np. "Pokaż moje 10 ostatnich faktur".

Dostępność wtyczki zależy od Twojego konta i ustawień administratora. Więcej informacji znajdziesz w [instrukcji łączenia aplikacji w ChatGPT](https://help.openai.com/en/articles/20001494-connecting-and-managing-app-accounts-in-chatgpt).

### Bezpośrednie połączenie MCP

Jeśli Twój asystent obsługuje własne serwery MCP, dodaj adres inFakt:

1. W swoim narzędziu AI otwórz ustawienia połączeń MCP lub własnych konektorów.
2. Dodaj połączenie o nazwie `inFakt` z adresem `https://mcp.infakt.pl/mcp`.
3. Wybierz OAuth, jeśli narzędzie pyta o metodę uwierzytelniania.
4. Zaloguj się do inFakt w otwartej przeglądarce i sprawdź żądane uprawnienia.
5. Włącz połączenie w rozmowie lub projekcie i poproś asystenta o pobranie danych.

Do tego połączenia nie musisz ręcznie podawać klucza API. Dostęp do własnych konektorów może zależeć od planu narzędzia AI i ustawień administratora.

### Claude w przeglądarce i aplikacji desktopowej

Dodaj własny konektor w ustawieniach Claude z adresem `https://mcp.infakt.pl/mcp`. Następnie zaloguj się do inFakt przez OAuth. Szczegóły znajdziesz w [instrukcji konektorów Claude](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).

### Claude Code

Dodaj serwer w terminalu:

```bash
claude mcp add --transport http infakt https://mcp.infakt.pl/mcp
```

Następnie uruchom `/mcp` w Claude Code i dokończ autoryzację. Zobacz [dokumentację MCP w Claude Code](https://code.claude.com/docs/en/mcp).

### Cursor

Dodaj wpis do `.cursor/mcp.json` w projekcie lub do `~/.cursor/mcp.json` dla wszystkich projektów. Jeśli plik już istnieje, dodaj `infakt` do jego sekcji `mcpServers`.

```json
{
  "mcpServers": {
    "infakt": {
      "url": "https://mcp.infakt.pl/mcp"
    }
  }
}
```

Dokończ logowanie, gdy Cursor poprosi o autoryzację. Zobacz [dokumentację MCP w Cursor](https://cursor.com/docs/mcp).

### ChatGPT: własna aplikacja MCP

Możesz też skonfigurować połączenie ręcznie, jeśli Twoje konto ChatGPT pozwala dodawać własne aplikacje MCP. Podaj adres `https://mcp.infakt.pl/mcp` i wybierz OAuth. Zobacz [dokumentację własnych aplikacji ChatGPT](https://help.openai.com/en/articles/12515353-build-with-the-apps-sdk).

### Inni klienci MCP

W pozostałych klientach użyj tego samego adresu. Klient musi obsługiwać Streamable HTTP i logowanie OAuth dla zdalnych serwerów MCP.

## Przykładowe polecenia

- "Pokaż moje 10 ostatnich faktur. Podaj numer, datę i kwotę brutto."
- "Znajdź nieopłacone faktury z poprzedniego miesiąca i podaj terminy płatności."
- "Podsumuj koszty z sierpnia 2026. Rozdziel kwoty według waluty."
- "Znajdź klienta po nazwie i pokaż jego dane."
- "Pokaż dostępne dane o składkach ZUS za lipiec 2026."
- "Sprawdź status mojej integracji z KSeF."

Jeśli masz dostęp do zapisu, możesz też poprosić: "Przygotuj fakturę na podstawie poprzedniej faktury dla tego klienta i pokaż dane do zatwierdzenia." Asystent powinien dopytać o brakujące informacje.

## Informacje dla agentów AI

Używaj inFakt MCP do pracy z danymi konta inFakt. Parametry wywołań sprawdzaj w schematach narzędzi zwracanych przez serwer.

1. Po autoryzacji odczytaj `tools/list`. Sprawdź dostępne funkcje i ich parametry w `inputSchema`. Nie zakładaj dostępności zapisu na podstawie tego README.
2. Korzystaj z `infakt_meta_catalog`, aby poznać typy zasobów. `infakt_meta_schema` opisuje zasoby, `infakt_meta_examples` podaje przykłady, a `infakt_meta_dictionary` zwraca słowniki wartości. Przykładowe identyfikatory w dokumentacji nie są identyfikatorami użytkownika.
3. Pobieraj identyfikatory dokumentów i klientów z wyników narzędzi. Nie zgaduj ich. Dobieraj typ faktury, filtry dat i pola zgodnie ze schematem konkretnego narzędzia.
4. Uwzględniaj paginację przy zestawieniach. Narzędzia listujące używają `limit` i `offset`; domyślny limit wynosi 10, a maksymalny 100. Nie przedstawiaj jednej strony wyników jako pełnego zestawienia.
5. Sprawdzaj jednostkę kwoty i walutę w schemacie oraz odpowiedzi. Nie przeliczaj ponownie kwot już sformatowanych przez serwer. Oddzielaj kwoty netto, VAT i brutto oraz sumy w różnych walutach.
6. Gdy narzędzie zwraca `confirmation_required`, pokaż podgląd i uzyskaj wyraźną zgodę użytkownika. Dopiero wtedy powtórz operację z tymi samymi danymi i otrzymanym `confirmation_token`, zgodnie z `inputSchema`. Zmiana danych wymaga nowego podglądu. Token nie zastępuje zgody użytkownika.
7. Potwierdź wykonanie dopiero po sprawdzeniu wyniku. Podgląd ani rozpoczęcie zadania nie oznaczają zakończenia operacji. Jeśli otrzymasz identyfikator zadania, sprawdź jego stan przez `infakt_async_status`. Po niejednoznacznym błędzie zapisu sprawdź stan przed ponowieniem, aby uniknąć duplikatów.

Skrócony opis po angielsku i odnośniki do dokumentacji są w [llms.txt](llms.txt).

## Dostęp i prywatność

Logowanie odbywa się w inFakt. Nie wpisuj hasła, tokenu ani klucza API do rozmowy z asystentem. Dostęp do danych zależy od przyznanych uprawnień.

Dane pobrane z inFakt trafiają do asystenta AI. Jego dostawca i ustawienia Twojego konta określają, jak je przechowuje i wykorzystuje. Sprawdź te zasady przed pracą z dokumentami firmy lub klientów.

Sprawdzaj dokumenty, kwoty i odbiorców przed zatwierdzeniem zapisu lub wysyłki. Odpowiedź asystenta nie zastępuje weryfikacji księgowej.

## Rozwiązywanie problemów

| Problem | Co sprawdzić |
| --- | --- |
| Nie widzę wtyczki inFakt w ChatGPT | Sprawdź katalog aplikacji lub wtyczek oraz dostęp przyznany przez administratora obszaru roboczego. |
| Nie mogę dodać połączenia | Sprawdź obsługę zdalnego MCP i OAuth w kliencie oraz ustawienia administratora. |
| Logowanie nie działa lub dostęp wygasł | Sprawdź adres z końcówką `/mcp` i ponownie autoryzuj połączenie. |
| Nie widzę narzędzia lub otrzymuję odmowę dostępu | Sprawdź przyznane uprawnienia i odśwież listę narzędzi w kliencie. |
| Nie widzę operacji zapisu | Zapis musi być dostępny po stronie usługi i dozwolony dla Twojego konta. |
| Brakuje danych w zestawieniu | Sprawdź filtry, typ dokumentu, zakres dat i kolejne strony wyników. |

## Pomoc

- [Centrum pomocy inFakt](https://pomoc.infakt.pl/hc/pl)
- [Strona inFakt](https://www.infakt.pl/)
- [Repozytorium dokumentacji inFakt MCP](https://github.com/infakt/infakt-mcp)

Zgłaszając problem, podaj nazwę klienta AI, nazwę narzędzia i komunikat błędu po usunięciu danych poufnych. Nie publikuj dokumentów księgowych ani danych logowania.
