****# LastPass 2022 Incident Analysis

## Źródła
1. Raport z Incydentu | https://blog.lastpass.com/posts/security-incident-update-recommended-actions
2. MITRE D3FEND MATRIX | https://d3fend.mitre.org/
3. MITRE ATT&CK MATRIX | https://attack.mitre.org/
4. CYBER KILL CHAIN | https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html

## 1. Nazwa incydentu
Brak konkretnej nazwy incydentu, natomiast sam LastPass używa nazw:
Incydent 1 i Incydent 2 

## 2. Organizacja i rok
LastPass - dostawca rozwiązania do zarządzania hasłami, będący w czasie incydentu częścią firmy GoTo.
2 incydenty w 2022 roku:
1. Pierwsza aktywność pomiędzy 8 a 12 Sierpnia
2. Drugi jako seria działań obejmujących rekonesans, enumerację oraz eksfiltrację danych w środowisku chmurowej pamięci masowej, prowadzonych od 12 sierpnia 2022 r. do 26 października 2022 r. używając informacji skradzionych w trakcie Incydentu 1.

## 3. Profil atakującego

Ze względu na brak jednoznacznych dowodów:

- brak kontaktu ze strony sprawcy/sprawców,
- brak żądań,
- brak wiarygodnych śladów sprzedaży skradzionych danych lub prób ich sprzedaży,

nie można wiarygodnie określić profilu ani motywacji sprawcy.

## 3.1 Własna ocena

Na podstawie poniższych informacji:

- seria działań trwająca kilka miesięcy,
- wykorzystanie informacji zdobytych podczas pierwszego incydentu do przeprowadzenia kolejnego etapu,
- prowadzenie rekonesansu, enumeracji i eksfiltracji danych,
- wieloetapowy charakter operacji,

możliwe scenariusze obejmują zaawansowaną działalność cyberprzestępczą, szpiegostwo korporacyjne lub działania grupy APT.

Dostępne informacje nie pozwalają jednak jednoznacznie przypisać sprawcy do żadnej z rozważanych kategorii ani wykluczyć pozostałych.

## 4. Wektor wejścia

Incydent 1:
Brak danych. Raport nie ujawnia początkowego wektora wejścia. 
Potwierdzono jedynie, że atakujący uzyskał dostęp do środowiska deweloperskiego z wykorzystaniem konta dewelopera.

Incydent 2:
Wektorem wejścia było wykorzystanie podatności w oprogramowaniu multimedialnym firmy trzeciej zainstalowanym na prywatnym komputerze jednego z czterech inżynierów DevOps, co umożliwiło zdalne wykonanie kodu (RCE).

## 5. Cel działania

Incydent 1:
Dostęp do dokumentacji technicznej i repozytoriów kodu źródłowego LastPass'a oraz ich eksfiltracja. 
W części repozytoriów kodu źródłowego znajdowały się:
- dane uwierzytelniające zapisane jawnym tekstem (plaintext)
- certyfikaty cyfrowe, używane w środowiskach deweloperskich
- zaszyfrowane dane uwierzytelniające wykorzystywane w środowisku produkcyjnym, między innymi do wykonania kopii zapasowych

Do wykorzystania zaszyfrowanych danych wymagany był oddzielny klucz deszyfrujący, który podczas pierwszego incydentu nie był dostępny ani dla zaatakowanego inżyniera, ani dla atakującego.

Incydent 2:
Dostęp do i eksfiltracja kopii zapasowych środowiska produkcyjnego, innych zasobów przechowywanych w chmurze oraz powiązanych z nimi kopii zapasowych krytycznych baz danych poprzez przechwycenie Hasła Głównego (Master Password) oraz wyeksportowanie zawartości firmowego sejfu LastPass'a i folderów współdzielonych, zawierających bezpieczne notatki (secure notes) z kluczami dostępu AWS oraz kluczami deszyfrującymi. Klucze te były niezbędne do uzyskania dostępu do kopii zapasowych środowiska produkcyjnego LastPass'a przechowywanych w usłudze AWS S3.

## 6. Naruszone elementy CIA
   
Incydent 1 i 2:
   - Poufność
     Tak. Nieuprawniony dostęp oraz eksfiltracja dokumentacji technicznej, repozytoriów kodu źródłowego, kopii zapasowych i innych poufnych danych.
   - Integralność
      Nie. Raport nie wskazuje, aby dane lub systemy zostały zmodyfikowane bądź uszkodzone.
   - Dostępność
      Nie. Raport nie wskazuje na zakłócenie działania usług ani utratę dostępności systemów.

## 7. Cyber Kill Chain
   - Reconnaissance
   - Weaponization
   - Delivery
   - Exploitation
   - Installation
   - Command & Control
   - Actions on Objectives

## 8. Co poszło nie tak po stronie obrońców?

## 9. Gdzie można było przerwać incydent?

## 10. Trzy rekomendacje bezpieczeństwa
****
