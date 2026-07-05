# Predykcyjne Utrzymanie Ruchu z Wykorzystaniem Machine Learning

## 👥 Autor projektu
* **Szymon Kozica**

## 📝 Koncepcja projektu i kontekst biznesowy
Nieplanowane przestoje maszyn przemysłowych generują dla zakładów produkcyjnych bardzo wysokie koszty. Celem tego projektu jest przygotowanie systemu **Predictive Maintenance**, który pozwala wykrywać podwyższone ryzyko awarii maszyny zanim dojdzie do poważnej usterki.

Analiza danych sensorycznych umożliwia przejście z podejścia reaktywnego, czyli naprawy po awarii, na podejście proaktywne. Głównym celem modelowania jest znalezienie kompromisu między niewykrywaniem rzeczywistych awarii (**Recall**) a generowaniem zbyt wielu fałszywych alarmów (**Precision**).

## 📊 Opis danych
Zbiór danych zawiera 10 000 obserwacji pracy maszyn produkcyjnych. Najważniejsze cechy wykorzystane w analizie to:

* `Type`: wariant jakości produktu (L/M/H: Low/Medium/High).
* `Air temperature [K]`: temperatura otoczenia maszyny.
* `Process temperature [K]`: temperatura procesu produkcyjnego.
* `Rotational speed [rpm]`: prędkość obrotowa wrzeciona.
* `Torque [Nm]`: moment obrotowy działający na maszynę, silnie ujemnie skorelowany z prędkością obrotową.
* `Tool wear [min]`: czas pracy aktualnie używanego narzędzia.
* `Machine failure`: zmienna docelowa, gdzie `0` oznacza brak awarii, a `1` oznacza awarię lub wysokie ryzyko awarii.

Zbiór danych jest silnie niezbalansowany: awarie stanowią około **3,39%** wszystkich obserwacji.

## 💻 Środowisko i uruchomienie
Projekt został przygotowany jako odtwarzalny workflow w notebooku Jupyter, z wykorzystaniem podstawowych bibliotek do analizy danych i uczenia maszynowego.

### Wymagania
* **Python:** 3.10+
* **Biblioteki:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `plotly`

Aby zainstalować wymagane zależności, należy uruchomić:

```bash
pip install -r requirements.txt
```

### Struktura projektu
```text
├── data/
│   └── ai4i2020.csv                         # Surowe dane telemetryczne
├── notebooks/
│   └── predictive-maintenance.ipynb         # Główny notebook analityczny
├── plots/
│   ├── 01_distribution_of_machine_failures.png
│   ├── 02_correlation_matrix.png
│   ├── 03_feature_importance_optimized_rf.png
│   └── 04_model_performance_comparison.html
├── .gitignore                               # Reguły ignorowania plików tymczasowych
├── README.md                                # Główna dokumentacja po angielsku
├── README_PL.md                             # Polska wersja dokumentacji
├── requirements.txt                         # Zależności projektu
```

## 🧠 Rozwój modeli i porównanie wyników
Ze względu na silne niezbalansowanie klas modele były oceniane przede wszystkim na podstawie metryk **Precision**, **Recall** oraz **F1-score** dla klasy awarii. Do podziału danych wykorzystano stratyfikację, a w modelach zastosowano ważenie klas.

| Model | Precision | Recall | F1-Score | Status |
| :--- | :---: | :---: | :---: | :--- |
| **Logistic Regression** *(model bazowy)* | 0.14 | 0.82 | 0.24 | Baseline |
| **Decision Tree** *(model nieliniowy)* | 0.31 | 0.87 | 0.46 | Intermediate |
| **Optimized Random Forest** *(GridSearchCV)* | **0.50** | **0.79** | **0.62** | **Best Candidate** |

### Najważniejsze obserwacje
* **Model bazowy:** Logistic Regression osiągnął wysoki Recall, ale bardzo niską Precision. Oznacza to, że model wykrywał wiele awarii, lecz generował także dużo fałszywych alarmów.
* **Modele drzewiaste:** Decision Tree oraz Random Forest lepiej uchwyciły nieliniowe zależności między cechami technicznymi, takimi jak moment obrotowy, prędkość obrotowa i zużycie narzędzia.
* **Najlepszy kandydat:** Optimized Random Forest po strojeniu hiperparametrów osiągnął najlepszy kompromis między wykrywaniem awarii a ograniczeniem liczby fałszywych alarmów.

## 🎯 Wnioski końcowe
* **Zgodność z intuicją techniczną:** Najważniejsze cechy modelu, takie jak `Torque`, `Rotational speed` i `Tool wear`, są logicznie powiązane z rzeczywistym ryzykiem awarii maszyn.
* **Znaczenie metryk:** Przy tak niezbalansowanym zbiorze danych sama accuracy byłaby myląca, dlatego projekt koncentruje się na Precision, Recall i F1-score.
* **Potencjalna wartość praktyczna:** Najlepszy model mógłby pomóc wykrywać około 4 z 5 przypadków awarii, jednocześnie ograniczając liczbę niepotrzebnych interwencji względem modelu bazowego.

## 📌 Status projektu
Projekt stanowi akademicki proof of concept: zawiera analizę danych, przygotowanie cech, porównanie kilku modeli, strojenie hiperparametrów oraz interpretację najważniejszych cech modelu.
