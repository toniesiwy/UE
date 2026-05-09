# Regresja liniowa w R — notatka do zrozumienia

## 1. Kod w R

```r
lm(PKB ~ inwestycje, data = ramka) -> regresja
abline(regresja, col = "red")
summary(regresja)
```

Powyższy kod estymuje model regresji liniowej, w którym:

- zmienna zależna: `PKB`,
- zmienna objaśniająca: `inwestycje`.

Model zapisujemy jako:

$$
PKB_i = \alpha_0 + \alpha_1 \cdot inwestycje_i + \varepsilon_i
$$

gdzie:

- $PKB_i$ — wartość PKB dla i-tej obserwacji,
- $inwestycje_i$ — wartość inwestycji dla i-tej obserwacji,
- $\alpha_0$ — wyraz wolny,
- $\alpha_1$ — współczynnik regresji przy zmiennej `inwestycje`,
- $\varepsilon_i$ — składnik losowy, czyli część PKB niewyjaśniona przez model.

Funkcja `lm()` szacuje parametry modelu metodą najmniejszych kwadratów.

Oznacza to, że R dobiera taką prostą regresji, aby suma kwadratów reszt była jak najmniejsza.

---

## 2. Co robi `abline(regresja, col = "red")`

```r
abline(regresja, col = "red")
```

Polecenie `abline()` dodaje do wykresu linię regresji.

Argument `col = "red"` oznacza, że linia będzie czerwona.

Linia regresji pokazuje teoretyczną, oszacowaną zależność między inwestycjami a PKB.

---

## 3. Reszty, czyli `Residuals`

Reszta to różnica między wartością rzeczywistą a wartością przewidywaną przez model.

Wzór:

$$
e_i = y_i - \hat{y}_i
$$

W przypadku modelu PKB:

$$
e_i = PKB_i - \widehat{PKB}_i
$$

Interpretacja:

- jeśli reszta jest dodatnia, model zaniżył wartość, bo rzeczywisty PKB był większy niż przewidywany;
- jeśli reszta jest ujemna, model zawyżył wartość, bo rzeczywisty PKB był mniejszy niż przewidywany;
- jeśli reszta jest bliska zeru, model dobrze dopasował się do obserwacji.

W `summary(regresja)` R zwykle nie wypisuje wszystkich reszt, tylko statystyki opisowe ich wektora:

```text
Min      1Q   Median      3Q      Max
```

Oznaczają one:

- `Min` — najmniejsza reszta,
- `1Q` — pierwszy kwartyl,
- `Median` — mediana reszt,
- `3Q` — trzeci kwartyl,
- `Max` — największa reszta.

Na podstawie tych wartości można ocenić, czy reszty są w przybliżeniu rozłożone wokół zera oraz jak duże są błędy modelu.

---

## 4. Estymatory parametrów regresji liniowej

W regresji liniowej szacujemy parametry:

$$
\alpha_0
$$

oraz

$$
\alpha_1
$$

Prawdziwe wartości parametrów w populacji nie są znane, dlatego na podstawie danych obliczamy ich estymatory.

Przykładowe równanie:

$$
PKB = 715{,}3 + 420{,}3 \cdot inwestycje
$$

Interpretacja:

### Wyraz wolny

$$
\hat{\alpha}_0 = 715{,}3
$$

Oznacza przewidywany poziom PKB, gdy inwestycje wynoszą zero.

Czyli przy `inwestycje = 0` model przewiduje:

$$
PKB = 715{,}3
$$

### Współczynnik regresji

$$
\hat{\alpha}_1 = 420{,}3
$$

Oznacza, że wzrost inwestycji o jedną jednostkę powoduje przeciętny wzrost PKB o około `420,3` jednostki.

Czyli jeśli inwestycje wzrosną o 1, to zgodnie z modelem PKB wzrośnie średnio o `420,3`.

---

## 5. Błędy standardowe estymatorów

Jeśli pod równaniem pojawiają się wartości:

$$
(273{,}5) \quad (57{,}7)
$$

to są to błędy standardowe estymatorów.

Czyli:

$$
SE(\hat{\alpha}_0) = 273{,}5
$$

$$
SE(\hat{\alpha}_1) = 57{,}7
$$

Błąd standardowy informuje, jak dokładnie oszacowano dany parametr.

Im mniejszy błąd standardowy, tym bardziej precyzyjny jest estymator.

---

## 6. Test t-Studenta

Test t-Studenta służy do badania istotności pojedynczego parametru regresji.

Dla każdego parametru testujemy hipotezy:

$$
H_0: \alpha_j = 0
$$

$$
H_1: \alpha_j \neq 0
$$

Hipoteza zerowa oznacza, że parametr nie różni się statystycznie od zera.

Dla zmiennej `inwestycje` hipotezy są następujące:

$$
H_0: \alpha_1 = 0
$$

czyli inwestycje nie mają istotnego wpływu na PKB.

$$
H_1: \alpha_1 \neq 0
$$

czyli inwestycje mają istotny wpływ na PKB.

Statystyka testowa ma postać:

$$
t = \frac{\hat{\alpha}_j - 0}{SE(\hat{\alpha}_j)}
$$

czyli prościej:

$$
t = \frac{\hat{\alpha}_j}{SE(\hat{\alpha}_j)}
$$

Dla współczynnika przy inwestycjach:

$$
t = \frac{420{,}3}{57{,}7}
$$

$$
t \approx 7{,}28
$$

Otrzymana wartość empiryczna statystyki t wynosi około `7,28`.

Im większa wartość bezwzględna statystyki t, tym silniejsze podstawy do odrzucenia hipotezy zerowej.

---

## 7. P-value i istotność statystyczna

Wyniki `summary(regresja)` zawierają również wartości `p-value`.

`P-value` mówi, jakie jest prawdopodobieństwo uzyskania takiego lub bardziej skrajnego wyniku, gdyby hipoteza zerowa była prawdziwa.

Najczęściej przyjmuje się poziom istotności:

$$
\alpha = 0{,}05
$$

Zasada decyzji:

- jeżeli `p-value < 0,05`, odrzucamy hipotezę zerową;
- jeżeli `p-value >= 0,05`, nie ma podstaw do odrzucenia hipotezy zerowej.

Zmienna jest istotna statystycznie, jeżeli jej `p-value` jest mniejsze od przyjętego poziomu istotności.

Dla zmiennej `inwestycje`:

- jeśli `p-value < 0,05`, to inwestycje są istotne statystycznie;
- jeśli `p-value >= 0,05`, to inwestycje nie są istotne statystycznie.

Istotność statystyczna oznacza, że wpływ danej zmiennej na zmienną zależną jest statystycznie różny od zera.

---

## 8. Błąd standardowy reszt — `Residual standard error`

`Residual standard error` oznacza błąd standardowy reszt.

Informuje, o ile przeciętnie wartości rzeczywiste różnią się od wartości przewidywanych przez model.

Wzór:

$$
RSE = \sqrt{\frac{SSE}{n-k-1}}
$$

gdzie:

- $SSE$ — suma kwadratów reszt,
- $n$ — liczba obserwacji,
- $k$ — liczba zmiennych objaśniających,
- $n-k-1$ — liczba stopni swobody.

Interpretacja:

- im mniejszy `Residual standard error`, tym lepiej model dopasowuje się do danych;
- jest wyrażony w jednostkach zmiennej zależnej, czyli tutaj w jednostkach PKB.

---

## 9. Stopnie swobody

Stopnie swobody w regresji liniowej liczymy według wzoru:

$$
df = n - k - 1
$$

gdzie:

- $n$ — liczba obserwacji,
- $k$ — liczba zmiennych objaśniających,
- $1$ — wyraz wolny w modelu.

W prostej regresji liniowej mamy jedną zmienną objaśniającą, więc:

$$
k = 1
$$

Dlatego:

$$
df = n - 1 - 1
$$

$$
df = n - 2
$$

Jeżeli w wyniku mamy 3 stopnie swobody, to:

$$
n - 2 = 3
$$

$$
n = 5
$$

Oznacza to, że model został oszacowany na podstawie 5 obserwacji.

---

## 10. Współczynnik determinacji — R-kwadrat

Współczynnik determinacji oznaczamy jako:

$$
R^2
$$

Mierzy on, jaka część zmienności zmiennej zależnej została wyjaśniona przez model.

W tym przykładzie mówi, jaka część zmienności PKB została wyjaśniona przez inwestycje.

Zakres wartości:

$$
0 \leq R^2 \leq 1
$$

Interpretacja:

- $R^2 = 0$ — model nie wyjaśnia zmienności zmiennej zależnej;
- $R^2 = 1$ — model idealnie wyjaśnia zmienność zmiennej zależnej;
- im większe $R^2$, tym lepsze dopasowanie modelu.

Przykład:

$$
R^2 = 0{,}95
$$

oznacza, że 95% zmienności PKB zostało wyjaśnione przez zmienność inwestycji.

Pozostałe 5% wynika z innych czynników oraz składnika losowego.

Ważne: wysokie $R^2$ nie zawsze oznacza, że model jest poprawny merytorycznie. Trzeba też sprawdzać sens ekonomiczny modelu, istotność parametrów i zachowanie reszt.

---

## 11. Współczynnik zbieżności

Współczynnik zbieżności pokazuje część zmienności zmiennej zależnej, której model nie wyjaśnia.

Liczymy go jako:

$$
\varphi^2 = 1 - R^2
$$

Przykład:

Jeśli:

$$
R^2 = 0{,}95
$$

to:

$$
\varphi^2 = 1 - 0{,}95 = 0{,}05
$$

Oznacza to, że 5% zmienności PKB nie zostało wyjaśnione przez model.

Interpretacja:

- im mniejszy współczynnik zbieżności, tym lepsze dopasowanie modelu;
- im większy współczynnik zbieżności, tym większa część zmienności pozostaje niewyjaśniona.

---

## 12. Skorygowany współczynnik determinacji

Skorygowany współczynnik determinacji w R występuje jako:

```text
Adjusted R-squared
```

Jest to poprawiona wersja zwykłego $R^2$.

Uwzględnia:

- liczbę obserwacji,
- liczbę zmiennych objaśniających w modelu.

Zwykły $R^2$ może rosnąć po dodaniu kolejnej zmiennej do modelu, nawet jeśli ta zmienna niewiele wyjaśnia.

Skorygowany $R^2$ „karze” model za dodawanie niepotrzebnych zmiennych.

Dlatego przy porównywaniu modeli z różną liczbą zmiennych lepiej patrzeć na `Adjusted R-squared` niż na zwykły `R-squared`.

Interpretacja:

- jeśli po dodaniu zmiennej skorygowany $R^2$ rośnie, zmienna poprawia model;
- jeśli skorygowany $R^2$ spada, zmienna prawdopodobnie nie wnosi wystarczająco dużo informacji.

---

## 13. Test F Fishera

Test F bada istotność całego modelu.

W modelu z wieloma zmiennymi hipotezy są następujące:

$$
H_0: \alpha_1 = \alpha_2 = \dots = \alpha_k = 0
$$

$$
H_1: \text{co najmniej jeden parametr jest różny od zera}
$$

Hipoteza zerowa oznacza, że model jako całość nie jest istotny statystycznie.

Hipoteza alternatywna oznacza, że przynajmniej jedna zmienna objaśniająca istotnie wpływa na zmienną zależną.

W prostej regresji liniowej z jedną zmienną objaśniającą hipotezy są prostsze:

$$
H_0: \alpha_1 = 0
$$

$$
H_1: \alpha_1 \neq 0
$$

Statystyka F ma postać:

$$
F = \frac{SSR/k}{SSE/(n-k-1)}
$$

gdzie:

- $SSR$ — suma kwadratów wyjaśniona przez model,
- $SSE$ — suma kwadratów reszt,
- $k$ — liczba zmiennych objaśniających,
- $n$ — liczba obserwacji.

Decyzja na podstawie `p-value`:

- jeśli `p-value < 0,05`, odrzucamy hipotezę zerową i uznajemy model za istotny statystycznie;
- jeśli `p-value >= 0,05`, nie ma podstaw do odrzucenia hipotezy zerowej, czyli model nie jest istotny statystycznie.

W prostej regresji liniowej test F i test t dla jedynej zmiennej objaśniającej prowadzą do tej samej decyzji.

Zachodzi wtedy zależność:

$$
F = t^2
$$

---

## 14. Przykładowa interpretacja całego modelu

Oszacowano model regresji liniowej opisujący zależność PKB od inwestycji.

Otrzymano równanie:

$$
PKB = 715{,}3 + 420{,}3 \cdot inwestycje
$$

Wyraz wolny wynosi `715,3`, co oznacza przewidywany poziom PKB przy zerowym poziomie inwestycji.

Współczynnik przy zmiennej `inwestycje` wynosi `420,3`. Oznacza to, że wzrost inwestycji o jedną jednostkę powoduje przeciętny wzrost PKB o `420,3` jednostki.

Błędy standardowe estymatorów wynoszą:

$$
SE(\hat{\alpha}_0) = 273{,}5
$$

$$
SE(\hat{\alpha}_1) = 57{,}7
$$

Dla zmiennej `inwestycje` wartość statystyki t wynosi:

$$
t = \frac{420{,}3}{57{,}7} \approx 7{,}28
$$

Jeżeli odpowiadające jej `p-value` jest mniejsze od `0,05`, to odrzucamy hipotezę zerową i uznajemy, że inwestycje są istotną statystycznie zmienną wyjaśniającą PKB.

Reszty pokazują różnice między wartościami rzeczywistymi a przewidywanymi przez model. Błąd standardowy reszt informuje o przeciętnym błędzie dopasowania modelu.

Stopnie swobody w prostej regresji liniowej liczymy jako:

$$
df = n - 2
$$

Jeżeli wynoszą one 3, oznacza to, że model został oszacowany na podstawie 5 obserwacji.

Współczynnik determinacji $R^2$ informuje, jaka część zmienności PKB została wyjaśniona przez inwestycje.

Współczynnik zbieżności, czyli $1 - R^2$, pokazuje część zmienności niewyjaśnioną przez model.

Skorygowany współczynnik determinacji uwzględnia liczbę zmiennych w modelu i jest lepszy do porównywania modeli o różnej liczbie zmiennych.

Test F Fishera sprawdza, czy cały model jest istotny statystycznie. Jeśli `p-value` dla testu F jest mniejsze od `0,05`, model jako całość jest istotny statystycznie.

---

## 15. Krótko o `glm()` i regresji logistycznej

Kod:

```r
glm(formula = am ~ mpg + cyl + disp + hp,
    data = mtcars,
    family = binomial) -> logist
```

oznacza estymację uogólnionego modelu liniowego, czyli `generalized linear model`.

Ponieważ użyto:

```r
family = binomial
```

jest to regresja logistyczna.

Regresję logistyczną stosuje się wtedy, gdy zmienna zależna jest zero-jedynkowa, czyli przyjmuje wartości:

$$
0 \quad \text{lub} \quad 1
$$

W danych `mtcars` zmienna `am` oznacza rodzaj skrzyni biegów:

- `0` — automatyczna,
- `1` — manualna.

Model:

```r
am ~ mpg + cyl + disp + hp
```

oznacza, że prawdopodobieństwo posiadania manualnej skrzyni biegów jest wyjaśniane przez zmienne:

- `mpg` — liczba mil na galon,
- `cyl` — liczba cylindrów,
- `disp` — pojemność silnika,
- `hp` — moc silnika.

W regresji logistycznej nie modelujemy bezpośrednio wartości zmiennej zależnej, tylko prawdopodobieństwo, że zmienna przyjmie wartość 1.

Czyli w tym przypadku modelujemy:

$$
P(am = 1)
$$

czyli prawdopodobieństwo, że samochód ma manualną skrzynię biegów.

---

# Najważniejsze rzeczy do zapamiętania

- `lm()` służy do estymacji modelu regresji liniowej.
- `summary()` pokazuje najważniejsze wyniki modelu.
- Reszty to różnice między wartościami rzeczywistymi a przewidywanymi.
- Estymatory parametrów pokazują oszacowaną zależność między zmiennymi.
- Współczynnik regresji mówi, o ile zmienia się zmienna zależna po wzroście zmiennej objaśniającej o jedną jednostkę.
- Test t bada istotność pojedynczego parametru.
- `p-value < 0,05` zwykle oznacza istotność statystyczną.
- `Residual standard error` informuje o przeciętnym błędzie modelu.
- Stopnie swobody w prostej regresji liniowej to `n - 2`.
- $R^2$ pokazuje część zmienności wyjaśnioną przez model.
- $1 - R^2$ pokazuje część zmienności niewyjaśnioną przez model.
- Skorygowany $R^2$ uwzględnia liczbę zmiennych.
- Test F bada istotność całego modelu.
- `glm(..., family = binomial)` oznacza regresję logistyczną.