<img alt="Logo" src="http://coderslab.pl/svg/logo-coderslab.svg" width="400">

# Revealing Module Pattern - Kalkulator

## Na czym polega zadanie?

### Zadanie polega napisaniu obsługi kalkulatora. Kalkulaltor powinien obsługiwać dodawanie, odejmowanie, mnożenie, dzielenie oraz części dziesiętne. Powinien również posiadać historię wcześniejszych wyrażeń.

### 1. Przygotowanie do pracy

W pliku `index.html` znajduje się stworzony kalkulator a w pliku `style.css` znajdują się do niego style. Nic nie zmieniaj w tych plikach - edytuj tylko i wyłącznie plik `app.js`. 

W tym zadaniu będziemy korzystać z Wzorca Odkrytego Modułu. W pliku `app.js` znajduje się już szablon. Nasz kod uruchomi się dopiero przy zdarzeniu `DOMContentLoaded` więc swobodnie możemy operować drzewem DOM. Dodatkowo mamy w kodzie zmienną `Calculator` do której jest przypisane samowywołujące się wyrażenie funkcyjne (__IFEE__), które zwraca obiekt.

Dzięki zastosowaniu Wzorca Odkrytego Modułu, wszystkie funkcje obsługujące kalulator będą ukryte (prywatne) a na zewnątrz zwracamy tylko jedną metodę, która dzięki domknięciom, ma dostęp do wszystkich prywatnych zmiennych i funkcji.

W kodzie znajdują się również 4 zmienne:
- `buttons` - jest to tablica ze wszystkimi przyciskami w naszym kalkulatorze,
- `historyView` - jest to element drzewa DOM odpowiadający za wyświetlanie historii operacji,
- `equationView` - jest to element drzewa DOM odpowiadający za wyświetlanie obecnego wyrażenia,
- `operations` - jest to tablica ze wszystkimi operacjami matematycznymi, jakie może wykonać użytkownik.

Twoim zadaniem będzie napisanie odpowiedniego kodu do istniejących funkcji, tak aby mieć sprawnie działający kalulator.

 
### 2. Obsługa zdarzeń

Do funkcji `attachEvents()` dopisz taki kod, który nałoży wszystkim przyciskom __oprócz ostatniego__ nasłuchiwanie zdarzeń. Nasłuchiwać będziemy kliknięcia. Na kliknięcie, niech wywoła się funkcja, która zablokuje standardowe zachowanie przycisku - `event.preventDefault()` oraz wypisz w konsoli tekst `Naciśnięto przycisk!`.

Do ostatniego przycisku również dodaj nasłuchiwanie zdarzenia - kliknięcia. Wykonaj te same czynności, wypisz jednak w kosnoli tekst `Sprawdzam wyrażenie!`.

Wywołaj funkcję `attachEvents()` wewnątrz funkcji `init()` - pamiętaj, że funkcję `init()` zwracamy dalej - dzięki temu staje się ona metodą obiektu zapisanego w zmiennej `Calculator`. Przed końcem pliku znajduje się więc wywołanie tej metody.

Jeśli wszystkie kroki zostały wykonane, kliknięcie na przycisk powinno wyświetlić w konsoli odpowiedni komunikat.

### 3. Wyświetlanie liczb

Zajmiemy się teraz funkcją `addCharacter()` - niech przyjmuje ona jako parametr ciąg znaków. Zadaniem tej funkcji jest dodanie parametru jako ostatni element w tekście występującym wewnątrz `equationView`.

Co to znaczy?
Na starcie wartość tekstu wewnątrz `equationView` to pusty ciąg znaków. Po naciśnięciu przycisku, na przykład `9`, dodajemy tą `9` jako ostatni znak wewnątrz `equationView`. Jeśli teraz naciśniemy `8`, to `8` zostanie dodane jako ostatni znak.
Wartość wewnątrz `equationView` to `98`.

Wykorzystaj właściowść `innerHTML` w taki sposób, aby funkcja `addCharacter(data)` dodawała właśnie poprzez `innerHTML` znak przekazywany jako parametr `data` jako ostatni znak wewnątrz `equationView`.

Na samym końcu należy wywołać wyżej wymienioną funkcję wewnątrz nasłuchiwania zdarzeń na wszystkie przyciski oprócz ostatniego wewnątrz `attachEvents()`. Funkcji `addCharacter(data)` jako parametr przekazujemy tekst trzymany wewnątrz naciśniętego przycisku:

```
// ...
event.preventDefault();
addCharacter(event.target.innerHTML);
// ...
```

### 4. Obliczanie wartości wyrażenia

Tym razem zajmiemy się funkcją `calculate()`. Funkcja ta powinna wykonać następujące czynności:
- pobrać tekst znajdujący się wewnątrz `equationView` i zapisać go do zmiennej
- wywołać funkcję `eval()`, do której przekazujemy wyrażenie zapisane w zmiennej z poprzedniego punktu
- wynik wywołania `eval()` należy zapisać do zmiennej
- zmienną z wynikiem należy wyświetlić wewnątrz `equationView`

Funkcja `eval()` to funkcja, która wykonuje kod / wyrażenie przekazane jako zmienną. Daje nam ona możliwość wywołania kodu, który może zostać dołączony do programu w trakcie jego działania.

Korzystamy tutaj z `eval()` aby zrobić prosty kalkulator ale należy uważać na tą funkcję - __jest to potencjalna dziura, jeśli chodzi o bezpieczeństwo aplikcji - korzystaj z `eval()` z bardzo dużą rozwagą (najlepiej jednak jest unikać tej funkcji).__

Wywołaj funkcję `calculate()` wewnątrz nasłuchiwania zdarzeń nałożonego na ostatni przycisk (button ze znakiem równości).

Jeśli wszystkie kroki zostały odpowiednio wykonane, aplikacja powinna pozwolić na wpisanie jakiegokolwiek wyrażenia a naciśnięcie przycisku ze znakiem równości obliczy wyrażenie i wypisze je w odpowiednim miejscu.

Teoretycznie kalkulator jest już skończony jednak czy widzisz jakieś problemy z działaniem kalkulatora?


### 5. Zasada działania kalulatora

Nasz kalkulator powinien umożliwiać wprowadzenie dowolnego wyrażania przy pomocy przycisków. Jednak powinien mieć on odpowiednie ograniczenia - możemy wstawić parę cyfr pod rząd, ale __nie możemy dać pod rząd dwóch znaków!__. W związku z musimy przygotować sobie sprawdzanie takie warunku.

Mamy funkcję `checkIfCharIsOperation(text)`. Niech ta funkcja przyjmuje parametr oznaczający ciąg znaków. Funkcja ma na celu sprawdzić, czy dany znak jest znakiem, który znajduje się w tablicy `operations`. Funkcja zwraca `true` jeśli tak jest lub `false`, gdy parametr nie jest znakiem (jest wtedy liczbą).


### 6. Sprawdzanie wyrażenia

Kolejnym krokiem jest sprawdzenie, czy przycisk naciśnięty przez użytkownika może zostać dodany do wyrażenia. Jest to zadaniem funkcji `checkEquation()`, która jako parametr przyjmuje tekst. Funkcja zwraca `false` w dwóch przypadkach:
- jeśli ostatnim znakiem wewnątrz naszego wyrażenia jest znak z tablicy `operations` (wtedy mielibyśmy dwa znaki pod rząd)
- jeśli nasze wyrażenie nie ma jakichkolwiek znaków i chcemy dodać znak z tablicy `operations` (nie możemy zaczynać wyrażenia od znaku).

Aby łatwiej skonstruować funkcję `checkEquation()`, skorzystaj z funkcji `checkIfCharIsOperation()`.

Wywołaj funkcję `checkEquation()` wewnątrz nasłuchiwania zdarzeń na wszystkie przyciski oprócz ostatniego. Przekaż funkcji jako parametr tekst, który znajduje się w akurat naciśniętym przycisku (wykorzystaj `event.target`). Zapisz to, co zwraca funkcja do zmiennej (np.: `canIaddChar`).


### 7. Modyfikacja dodawania znaku

Wykonując poprzedni podpunkt, mamy pewność, że wartość ukryta pod naciśniętym przyciskiem może zostać dodana do wyrażenia ale jeszcze z tego nie korzystamy.

Zmodyfikuj `addCharacter()` w takim sposób, aby funkcja przyjmowała dwa paremetry - pierwszy niech zostanie taki sam, drugi niech będzie flagą - wartością logiczną.

Funkcja `addCharacter()` powinna dodawać tekst do wyrażenia tylko wtedy, gdy flaga jest prawdą (`true`).

Przykładowe wywołanie funkcji:
```
//...
event.preventDefault();
var canIaddChar = checkEquation(event.target.innerHTML);
addCharacter(event.target, canIaddChar);
//...
```

### 8. Modyfikacja obliczania wyrażenia

Funkcja `eval()` działa dobrze, jeśli nasze wyrażenie jest poprawne matematycznie. Jednak, gdy ostatnim znakiem w naszym wyrażeniu jest znak znajdujący się w tablicy `operations` otrzymamy błąd.

Dopisz taki kod do funkcji `calculate()`, który będzie usuwał ostatni znak wyrażenia, jeśli znajduje się on w tablicy `operations`. Dopiero po tej operacji wywołaj funkcję `eval()`;


### 9. Historia operacji

Nasz kalkulator jest gotowy ale dodamy do niego ostatnią funkcjonalność - historię operacji.

Funkcja `updateHistory()` powinna jako parametr przyjmować ciąg znaków. Zadaniem funkcji `updateHistory()` jest dodanie do `historyView` jako element `p` operacji, która została właśnie wykonana. Wywołaj funkcję wewnątrz `calculate()`.

Przykład wywołania:
```
var currentEquation = equationView.innerHTML;  
//...
var answer = eval(currentEquation);
updateHistory(currentEquation + "=" + answer);
equationView.innerHTML = answer;
//...


### 10. KONIEC! :)

