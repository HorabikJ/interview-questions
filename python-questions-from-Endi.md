# Python questions:


### 1. Jaka jest różnica pomiędzy list a tuple i jakie to niesie za sobą konsekwencje?

`list` to kolekcja mutowalna, do której możemy dowolnie dodawać elementy.
`tuple` jest kolekcją immutable. Po stworzeniu nie da się dodać elementów.

Konsekwencją jest fakt, że możemy używać tuple jako klucz w dict (słowniku), ponieważ obiekty immutable są hashowalne.

### 2. Czy typowanie w Pythonie jest wymagane? Jak wygląda składania otypowanej funkcji?

Nie jest, aczkolwiek bardzo ułatwia pracę z kodem, bo IDE może podpowiadać typy a statyczne analizatory kodu wyłapywać
bugi (takie jak mypy).

Przykład otypowanej funkcji:
```python
def add(x: int, y: int) -> int:
return x + y
```

### 3. Czym jest i po co używać venv.

`venv`, czyli virtual environment, to mechanizm na stworzenie lokalnego interpretera Pythona wraz z wymaganymi do projektu
bibliotekami. Dzięki temu możemy mieć kilka projektów z odseparowanymi bibliotekami, np. jeden projekt może mieć
bibliotekę w wersji `X`, a drugi w wersji `Y` i nie stworzy do konfliktu pomiędzy wersjami, ponieważ każde środowisko jest
odseparowane.

### 4. Jakie znasz narzędzia do zarządania bibliotekami do projektu Pythonowego.

pip - wbudowane polecenie do instalowania bibliotek.

poetry - zewnętrzna "biblioteka" do zarządzania instalowaniem bibliotek. Pozwala na deterministyczną konfigurację wersji
bibliotek, dzięki czemu każdy członek zespołu (oraz CI) będą miały zainstalowane dokładnie te same dependencje (a przede
wszystkim dependencje dependencji, itd.)

pipenv - bardzo podobne do poetry, aczkolwiek poetry jest bardziej popularne dzisiaj.

### 5. Wytłumacz różne metody zrównoleglania wykonywania kodu Pythonowego oraz wspomnij czym jest GIL.

`GIL` - global interpreter lock.

Jego konsekwencją jest to, że wiele wątków w ramach tego samego procesu może wykonywać kod Pythona tylko wtedy, gdy
wątek otrzyma locka. Jednym słowem, wielowątkowość w Pythonie tak realnie nie istnieje. Jedyną korzyścią z
wielowątkowości jest przypadek, gdy mamy dużo operacji IO-bound (czekamy na połączenie sieciowe, dysk, ogólnie IO).
Wtedy faktycznie możemy przyśpieszyć program. Gdy mamy heavy computation workload (CPU-bound), wielowątkowość nic nam
nie da, gdyż w jednym momencie tylko jeden wątek uzyska dostęp do wykonywania kodu Pythonowego.

Aby zrównoleglić obliczenia, mamy też wieloprocesowość (multiprocessing). Wtedy odpalamy wiele procesów na raz, gdzie
każdy z nich jest odseparowany od siebie i możemy podzielić jedno duże zadanie obliczeniowe na różne procesy, a później
połączyć ich wyniki.

async/await to trzeci mechanizm, aczkolwiek wszystko dzieje się w ramach jednego wątku. Pomaga to tylko w przypadku
zadań IO-bound. Python trzyma listę wszystkich "tasków", które ma zrealizować i jeżeli napotka na wywołanie funkcji
asynchronicznej (oznaczonej słowem async) za pomocą await, a funkcja czeka na zakończenie operacji IO, to w tym czasie
inny task może być wykonywany do momentu kolejnego await. JavaScript posiada ten mechanizm wbudowany od praktycznie
samego początku (event loop).

### 6. Podaj znane frameworki do pisania kodu backendowego.

Django - duży framework, posiada swój własny ORM oraz system renderowania stron po stronie backendowej wykorzystując
Jinja2 template language.

Flask - mikroframework, gdzie definiujemy jako funkcje endpointy. Zazwyczaj wykorzystuje się przy nim wiele osobnych
bibliotek.

FastAPI - dosyć nowy ale zyskujący ogromną popularność framework. Posiada automatyczną serializację i deserializację
danych wejściowych (request) i wyjściowych (response) wykorzystując bibliotekę Pydantic. Oprócz serializacji posiada
wbudowaną walidację tychże danych, system dependencji (do dependency injection) oraz przede wszystkim całe API FastAPI
polega na typowaniu, co gigantycznie ułatwia pracę z kodem, gdyż IDE nam we wszystkim elegancko pomaga.
