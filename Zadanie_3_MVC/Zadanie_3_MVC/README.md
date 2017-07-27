<img alt="Logo" src="http://coderslab.pl/svg/logo-coderslab.svg" width="400">

# Model View Controller - Slider

## Na czym polega zadanie?

### Zadanie polega na napisaniu prostego slidera. Najpierw należy podbrać dane na temat zdjęć z zewnętrznego zasobu, następnie należy napisać obsługę slidera używając wzorca MVC.

### 1. Przygotowanie do pracy

W pliku `index.html` znajduje się element `div` do którego będziemy dodawać odpowiednie elementy a w `style.css` znajdują się style do tych elementów. Nic nie zmieniaj w tych plikach - edytuj tylko i wyłącznie plik `app.js`. 

W tym zadaniu będziemy korzystać z wzorca MVC. W pliku `app.js` znajduje się już szablon. Nasz kod uruchomi się dopiero przy zdarzeniu `DOMContentLoaded` więc swobodnie możemy operować drzewem DOM. Każda część aplikacji (Model, View, Controller) to inny konstruktor - działać wszystko będzie, dopiero gdy stworzymy obiekty na podstawie konstruktorów i je ze sobą połączymy.

W naszym przypadku:
- `Model` odpowiada za warstwę danych - w naszym przypadku odpowiada za łączenie się z zewnętrznym serwisem i zwracaniem tychże danych,
- `View` odpowiada za warstwę wizualną - w naszym przypadku są to wszystkie modyfikacje drzewa DOM,
- `Controller` odpowiada za połączenie modelu i widoku tak, aby wszystkie trzy części ze sobą współgrały.

Twoim zadaniem będzie napisanie odpowiedniego kodu do istniejących funkcji, tak aby mieć sprawnie działający slider.

 
### 2. Metoda bind()

Zanim przejdziemy dalej, warto nauczyć się, czym jest metoda `bind()`. Jest to metoda, która robi dwie rzeczy:
- tworzy nową funkcję na podstawie istniejącej
- przepina jej kontekst wywołania

Czyli przy pomocy `bind()` tworzymy nową funkcję gdzie możemy sami wskazać, jaki ma być wewnątrz niej `this`.
Przykład:
```
var Abc = {
    name: "Obiekt Abc",
    showName: function() {
        console.log(this.name);
    }
}

var Def = {
    name: "Obiekt Def"
}

Abc.showName(); // Wypisze "Obiekt Abc"

var newFunc = Abc.showName.bind(Def);
newFunc();     // Wypisze "Obiekt Def
```

`bind()` jako pierwszy parametr przyjmuje nowy kontekst, każdy kolejny parametr zostanie wykorzystany jako już normalny parametr dla nowej funkcji.


### 3. Model

Zaczniemy od warstwy danych. Konstruktor `Model` posiada na razie trzy właściwości:
- `url` - jest to adres, z którym będziemy się łączyć,
- `data` - tutaj będziemy przechowywać dane, które pobierzemy (będzie to tablica),
- `items` - jest to długość tablicy z danymi.

`Model` również posiada dwie metody dodane poprzez prototyp:
- `getAllItems(fn)` - która łączy się z adresem zapisanym w `url`, pobiera wszystkie dane i gdy to zrobi to zapisuje je w `data` a długość tablicy w `items` a na samym końcu wywołuje funkcję przekazaną jako parametr,
- `getItem(id)` - która zwraca z tablicy element od danym `id`.

Uzupełnij obie metody w odpowiedni sposób. 

Aby sprawdzić, czy wszystko działa, możesz dodać `console.log()` w odpowiednie miejsca. Nie zapomnij o utworzeniu obiektu na podstawie konstruktora:

```
//...
var sliderModel = new Model();
sliderModel.getAllItems();
//...
```

### 4. View - Cześć Pierwsza

Konstruktor `View` odpowiada za widok - to jego wyłączone zadanie. Obecnie posiada dwie właściwości:
- `element` - jest do element drzewa DOM, do którego będziemy dołączać nowe elementy,
- `onClick` - metoda wywoływana na kliknięcie przycisku - zostanie ona zaimplementowana wewnątrz Kontrolera i przekazana do Widoku później.

Zauważ, że konstruktor `View` jako parametr przyjmuje element z drzewa DOM.

Mamy tu jeszcze jedną metode - `render()`. Przyjmuje ona jako parametr obiekt (taki, jaki znajduje się w tablicy danych w modelu). Zadaniem tej metody jest wstrzyknięcie elementów HTML wypełnionych danymi w odpowiednie miejsce i nałożenie zdarzeń na przyciski do obsługi slidera.

Poniżej znajduje się ciąg znaków reprezentujący elementy, jakie należy wstrzyknąć:
```
//...
this.element.innerHTML = '<h3>' + item.name + '</h3>' +
    '<div class="slider-image" style="background-image: url(' + item.image + ');"></div>' +
    '<h4>' + item.author + '</h4>' + '<button id="prev" >Prev</button>' +
    '<button id="next">Next</button>';
//...
```
gdzie `item` to właśnie obiekt z danymi.

Gdy uzupełnisz metodę `render()`, spróbuj ją wywołać i sprawdzić, czy działa:
```
//...
var myObj = {
    name: "Test",
    image: "http://coderslab.pl/svg/logo-coderslab.svg",
    author: "Coders Lab"
}

var targetElement = document.getElementById('slider');
var sliderView = new View(targetElement);
sliderView.render(myObj);
//...
```


Nie przejmuj się, jeśli coś będzie brzydko wygldąć - ważne, aby działo.

Jeśli widok oraz model działają - mozesz przejść dalej.

### 5. Controller - Start

Nasz konstruktor `Controller` na start posiada trzy właściwości:
- `myView` - jest to odniesienie do widoku stworzonego przy pomocy konstruktora `View`,
- `myModel` - jest to odniesienie do modelu stworzonego przy pomocy konstruktora `Model`,
- `index` - jest to zmienna odpowiadająca za numer aktulnie wyświetlanego elementu z bazy.

Posiada również trzy metody dodane poprzez konstruktor:
- `init()` - metoda ta rozpoczyna działanie całej aplikacji,
- `onClick()` - metoda obsługująca kliknięcie mające na celu zmianę slajdu,
- `show()` - metoda ta wykorzystuje pobieranie danego elementu z naszej "bazy" (czyli `Model.getItem(id)`) i wykorzystuje te dane aby wyświetlić ja na widoku (czyli `View.render(data)`).

Zacznijmy od stworzenia odpowiednich obiektów na podstawie konstruktorów abyśmy mogli działać dalej:

```
//...
var sliderModel = new Model();
var targetElement = document.getElementById('slider');
var sliderView = new View(targetElement);
var sliderController = new Controller(sliderView, sliderModel);
//...
```
Pamiętaj - od teraz wewnątrz kontrolera jesteśmy w stanie odwołać się do modelu i widoku - są one dostępne jako właściwości obiektu stworzonego przez konstruktor `Controller` - `this.myModel` oraz `this.myView`.



### 5. Controller - show()

Metoda `show()` odpowiada za spięcie modelu i widoku w odpowiedni sposób. Wykonuje ona następujące czynności:
- pobiera za pomocą odpowiedniej metody z modelu dane na podstawie obecnego indeksu (`index` w `Controller`),
- zapisuje te dane do zmiennej
- wywołuje metodę `render()` przekazując metodzie jako parametr wcześniej pobrane dane.

Gdy skończysz metodę `show()`, wklej poniższy kod pod koniec pliku:

```
//...
var sliderModel = new Model();
var targetElement = document.getElementById('slider');
var sliderView = new View(targetElement);
var sliderController = new Controller(sliderView, sliderModel);

var sliderHelper = sliderController.show.bind(sliderController);

sliderController.myModel.getAllItems(sliderHelper);
//...
```

Mamy tu utworzenie odpowiednich obiektów na podstawie konstruktorów. Jednak najciekawszy kawałek kodu to:

```
var sliderHelper = sliderController.show.bind(sliderController);
```

Tworzymy funkcję `sliderHelper()`, która działa identycznie jak `sliderController.show()` jednak ma na sztywno ustawiony kontekst (`this`).

Powyższy kod połączy się z serwerem, a gdy otrzyma dane to wywoła nasz `sliderHelper()`. Wynikiem tej operacji powinien być widok pokazujący nazwę zdjęcia, zdjęcia, autora oraz przyciski.


### 6. Controller - onClick()

Przedostatnia metoda to `onClick()`, która jako parametr przyjmuje `event` - metoda będzie wywoływana na kliknięcie. Metoda ta wykonuje następujące czynności:
- wyłącza natrualne zachowanie elementu
- przy pomocy `event.currentTarget` sprawdza po `id` który przycisk został naciśnięty
- jeśli został naciśnięty przycisk `next` to zwiększa `index` o jeden sprawdzając jednocześnie, czy index może istnieć - jeśli nie to ustawia go na zero
- jeśli został naciśnięty przycisk `prev` to zmniejsza `index` o jeden sprawdzając jednocześnie, czy index może istnieć - jeśli nie to ustawia go na ostatni element w tablicy
- wywołuje z modelu metodę `getItem` przekazując metodzie jako parametr nowy `index`
- zapisuje poprzednie wywołanie do zmiennej
- wywołuje metodę `show()` przekazując jej jako parametr zmienną z poprzedniego podpunktu

Aby sprawdzić czy wszystko działa, musimy jeszcze zmienić parę rzeczy.

### 7. Controller - init()

Metoda `init()` ma na celu rozpoczęcie działania całej aplikacji. Nie przyjmuje ona parametrów i wykonuje następujące czynności:
- wywołuje w odpowiedni sposób metodę `getAllItems()` z modelu przekazując jako parametr metodę `show()` z ustawionym na sztywno kontekstem wywołania (w tym przypadku `.bind(this)`)
- przekazuje właściwości `onClick` w modelu metodę `onClick` z kontrolera z ustawionym na sztywno kontekstem (również `.bind(this`)).

Bez przypięcia kontektu na sztywno nasze metody nie mogłby działać gdyż this nie wskazywałby tam, gdzie trzeba.


### 8. View - Część druga

Została nam ostatnia rzecz do zrobienia a mianowicie podpięcie nasłuchiwania zdarzeń do przycisków. Należy to wykonać pod koniec metody `render()`. Kroki do wykonania:
- złapanie przycisku `prev` i zapisanie go do zmiennej
- zlapanie przycisku `next` i zapisanie go do zmiennej
- nałożenie na przyciski nasłuchiwania kliknięcia, gdzie wlaśnie na kliknięcie powinna zadziałąć metoda zapisana pod `this.onClick`, przekazana wcześniej z kontrolera.

Jeśli wszystkie kroki zostały wykonane poprawnie, powinien zostać utworzony slider wykorzystujący jako wzorzec właśnie MVC.

### 9. Koniec :) 




