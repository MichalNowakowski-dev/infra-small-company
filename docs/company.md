# Firma (scenariusz laboratoryjny)

> Firma jest fikcyjna. Projekt symuluje jej infrastrukturę IT na potrzeby nauki i portfolio.

## Profil

|            |                                                                                |
| ---------- | ------------------------------------------------------------------------------ |
| Nazwa      | **[NAZWA FIRMY]** (domena wewnętrzna: `[nazwa].local`)                         |
| Branża     | biuro rachunkowe                                                               |
| Pracownicy | 25 osób, w tym 6 pracujących zdalnie 2-3 dni w tygodniu                        |
| Biuro      | jedno, jedna sieć biurowa                                                      |
| Dane       | dokumenty klientów (umowy, faktury, deklaracje), dane osobowe podlegające RODO |

## Potrzeby biznesowe

1. **Wspólne przechowywanie i udostępnianie dokumentów** klientów oraz wewnętrznych: Nextcloud (`portal.[nazwa].local`).
2. **Dostęp zdalny** dla pracowników pracujących z domu, tylko przez VPN.
3. **Dane klientów nie mogą zginąć ani wyciec.**

## Wymagania (z nich wynikają decyzje techniczne)

| Wymaganie                               | Wartość                                                                     | Jak ją realizujemy                                  |
| --------------------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------- |
| **RPO** (maks. utrata danych)           | 24 godziny                                                                  | codzienny backup (etap: `bak01`)                    |
| **RTO** (maks. czas odtworzenia usługi) | 2 godziny                                                                   | udokumentowany i zmierzony test odtworzenia         |
| Dostępność w godzinach pracy (8-18)     | monitoring i alerty na usługi krytyczne                                     | etap: `mon01`                                       |
| Dostęp do danych                        | tylko uwierzytelnieni użytkownicy, ruch szyfrowany (HTTPS)                  | HAProxy z SSL na `fw01`, Nextcloud za reverse proxy |
| Separacja sieci                         | biuro nie ma bezpośredniego dostępu do zarządzania serwerami                | segmenty OFFICE / SERVERS / MGMT, reguły pfSense    |
| Dostęp administracyjny                  | tylko klucze SSH, bez logowania hasłem i bez roota                          | rola `common` (hardening SSH)                       |
| Sekrety                                 | nigdy jawnie w repozytorium                                                 | Ansible Vault                                       |
| RODO                                    | dane osobowe tylko w zaufanej sieci, kopie zapasowe w ograniczonym dostępie | segmentacja, uprawnienia na `bak01`                 |

## Usługi

| Usługa                    | Host     | Krytyczność                      |
| ------------------------- | -------- | -------------------------------- |
| Nextcloud (pliki firmowe) | `app01`  | wysoka                           |
| Reverse proxy / firewall  | `fw01`   | wysoka                           |
| Backup                    | `bak01`  | wysoka                           |
| Monitoring                | `mon01`  | średnia                          |
| Kontroler automatyzacji   | `ctrl01` | niska (narzędzie administratora) |

## Kryteria sukcesu projektu

- Nextcloud dostępny przez HTTPS pod nazwą `portal.[nazwa].local`.
- Pełne odtworzenie usługi z backupu w czasie krótszym niż RTO (udokumentowane pomiarem).
- Alert w ciągu minut od awarii usługi krytycznej.
- Całe środowisko odtwarzalne z repozytorium (`vagrant up` + `ansible-playbook`).
