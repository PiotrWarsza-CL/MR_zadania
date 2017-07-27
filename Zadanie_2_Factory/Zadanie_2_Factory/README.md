<img alt="Logo" src="http://coderslab.pl/svg/logo-coderslab.svg" width="400">

# Factory Pattern & Revealing Module Pattern - Sklep

## Na czym polega zadanie?

### Zadanie polega napisaniu obsługi sklepu - aplikacji, która pozwala dodawać nam nowe produkty do bazy w sklepie. 

### 1. Przygotowanie do pracy

W pliku `index.html` znajduje się stworzony formularz do dodawania produktów a w pliku `style.css` znajdują się do niego style. Nic nie zmieniaj w tych plikach - edytuj tylko i wyłącznie plik `app.js`. 

W tym zadaniu będziemy korzystać z Wzorca Odkrytego Modułu i Wzorca Fabryki. W pliku `app.js` znajduje się już szablon podzielony na dwie części - czesć związaną z Wzorcem Fabryki i część związaną z Wzorcem Odkrytego Modułu. Nasz kod uruchomi się dopiero przy zdarzeniu `DOMContentLoaded` więc swobodnie możemy operować drzewem DOM. Dodatkowo mamy w kodzie zmienną `Shop` do której jest przypisane samowywołujące się wyrażenie funkcyjne (__IFEE__), które zwraca obiekt.

Twoim zadaniem będzie napisanie odpowiedniego kodu do istniejących funkcji, tak aby mieć sprawnie działający kalulator.

 
### 2. Fabryka - Obsługa

W kodzie znajduje się funkcja `CreateItemFactory`, która będzie naszą fabryką / konstruktorem. Jej zadaniem będzie tworzenie nowych obiektów na podstawie przekazanych danych.

Zanim jednak przejdziemy do obsługi fabryki, należy stworzyć konstruktory obiektów, które będą tworzone. W kodzie znajdują się następujące konstruktory:
- `vegetable()` - warzywo
- `fruit()` - owoc
- `meat()` - mięso
- `fish()` - ryba

Zajmijmy się warzywami. Zadaniem konstruktora `vegetable()` jest stworzenie obiektu. Konstruktor jako parametr również przyjmuje obiekt. Każde produkt w sklepie powinien zawierać nazwę, kraj pochodzenia, informację o przechowywaniu w lodówce oraz typ produktu. Warzyw nie musimy przechowywac lodówce a typ warzywa to po prostu `vegetable` :)

Konstruktor powinien wyglądać tak:
```
function Vegetable(options) {
    this.name = options.name;
    this.country = options.country;
    this.fridge = false;
    this.type = "vegetable";
}
```
W analogiczny sposób dokończ pozostałe konstruktory, wiedząc, że mięsa i ryby muszą być przechowywane w lodówce.

### 3. Fabryka - Działanie

Mając wszystkie typy produktów, możemy dokończyć fabrykę.

Do naszego głównego konstruktora `CreateItemFactory` poprzez prototyp dodajemy metodę `createItem`, która powinna przyjmować dwa parametry - obiekt z danymi i ciąg znaków. Metoda powinna zwracać nowy obiekt w zależności od przekazanego drugiego parametru. Skorzystaj z instrukcji `switch`.

Przykładowo, gdy drugim parametrem dla metody jest `"vegetable"` to zwracamy nowy obiekt przy pomocy odpowiedniego konstruktora i słowa kluczowego `new`, jednocześnie przekazując konstruktowi pierwszy parametr metody:
```
//...
CreateItemFactory.prototype.createItem = function(options, type) {
//...
    return new Vegetable(options);
//...
}
```

### 4. Fabryka - Uruchomienie

Nasza fabryka to też konstruktor. W takim razie należy go wywołać. Stwórz zmienną `ShopDatabase` do której zapisz zwrócony przez konstruktor obiekt - nie zapomnij o słowie kluczowym `new`.

Od teraz jeśli chcielibyśmy dodać nowy produkt, skorzystamy z instancji znajdującej się w `ShopDatabase`:
```
var newItem = ShopDatabase.createItem(options, type)
```

Przetestuj działanie metody i zobacz, czy tworzy ona nowe obiekty w odpowiedni sposób.


### 5. Odkryty Moduł - Omówienie

Dalsza część zadania to wykorzystanie naszej Fabryki wewnątrz Odkrytego Modułu. W pliku `app.js` znajduje się szkielet wzorca - funkcje należy dokończyć w odpowiedni sposób. Wewnątrz szkieletu znajdują się trzy zmienne:
- `listOfItems` - jest to tablica, która będzie przechowywać wszystkie obiekty stworzone przy pomocy Fabryki
- `shopForm` - jest to zmienna odwołująca się do formularza
- `showItems` - jest to zmienna odwołująca się do miejsca, w którym będą dodwane nowe produkty


### 6. Odkryty Moduł - Dodanie obsługi zdarzenia

Do funkcji `attachEvent()` dopisz taki kod, który nałoży formularzowi nasłuchiwanie zdarzenia. Nasłuchiwać będziemy zdarzenia `submit`. Na wysłanie formlarza, niech wywoła się funkcja, która zablokuje standardowe zachowanie przycisku - `event.preventDefault()` oraz wypisz w konsoli wartości wszystkich elemetów `input` oraz `select` w konsoli.


### 6. Odkryty Moduł - Dodanie produktów

Do funkcji `addItemToShop()` dopisz taki kod, który do zmiennej zapisze wywołanie naszej fabryki - `ShopDatabase.createItem()`. Zapisany obiekt do zmiennej dodaj również do tablicy `listOfItems`. 

Wywołaj funkcję `addItemToShop()` wewnątrz `attachEvent()` przekazując funkcji w odpowiedni sposób dane pobrane z formularza.


### 7. Odkryty Moduł - Wyświetlanie produktów

Do funkcji `render()` dopisz taki kod, który spowoduje, że funkcja doda nowy paragraf jako ostatnie dziecko do `showItems`. Niech funkcja `render()` jako parametr przyjmuje obiekt.

Paragraf powinien zawierać wszystkie dane produktu (nazwa, kraj pochodzenia, typ, informację o przechowywaniu).

Wywołaj funkcję `render()` na końcu `addItemToShop()` przekazując funkcji jako parametr obiekt stworzony przez Fabrykę.

### 8. Koniec :)