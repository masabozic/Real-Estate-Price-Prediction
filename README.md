# Real-Estate-Price-Prediction


## 📋 Opis problema i skup podataka
Projekat rešava **regresioni problem** predviđanja kontinualne vrednosti cene nekretnine izražene u evrima (EUR). Dataset sadrži **5000 zapisa** koji su generisani sintetički, simulirajući realne zakonitosti i ekonomske trendove na tržištu nekretnina.

### Atributi (Features):
* `Area_m2` — Ukupna korisna površina u kvadratnim metrima
* `YearBuilt` — Godina izgradnje objekta
* `Furnished` / `Renovated` / `HasBasement` — Binarni atributi (Yes/No)
* `Location` — Lokacija nekretnine (*Centar, Novi Beograd, Vračar, Zemun, Periferia, Predgrađe*)
* `PropertyType` — Tip nekretnine (*Stan, Kuća, Studio, Vila*)
* `ParkingSpaces` / `Bedrooms` / `Bathrooms` — Numerički brojači komfora
* `Condition` — Opšte stanje nekretnine (*Poor, Fair, Good, Excellent*)
* **`Price`** — Ciljna promenljiva (Cena u EUR)

---

## 🛠️ Metodologija i Koraci implementacije

### 1. Preprocesiranje i čišćenje podataka
* **Nedostajuće vrednosti:** Imputacija numeričkih promenljivih izvršena je preko *medijane*, dok su kategoričke promenljive (`Condition`, `Furnished`) popunjene *modom*.
* **Uklanjanje outliera:** Podaci su filtrirani unutar realnih tržišnih granica (površina od 15 do 350 $m^2$, cena od 20.000 € do 700.000 €).
* **Enkodiranje kategoričkih promenljivih:** * Binarni atributi su mapirani u `1/0`.
  * `Condition` je enkodiran ordinalno (1-4) zbog prirodnog redosleda.
  * `Location` i `PropertyType` su transformisani kroz *One-Hot Encoding* (`drop_first=True`).

### 2. Eksplorativna analiza podataka (EDA)
Izvršena je vizuelna i statistička provera distribucija atributa, korelacija sa cenom (gde površina dominira sa $r = 0.73$) i uticaja pojedinačnih lokacija/tipova na cenu nekretnina.

### 3. Treniranje i Evaluacija Modela
U projektu su testirana 3 algoritma (sa podelom podataka 80% trening / 20% test):
1. **Linear Regression** (uz `StandardScaler` normalizaciju)
2. **Random Forest Regressor**
3. **Gradient Boosting Regressor**

---

## 📊 Rezultati i Finalne Metrike

Nakon fine optimizacije hiperparametara kroz **GridSearchCV** i provere stabilnosti putem **10-Fold Cross-Validation-a**, **Gradient Boosting** se izdvojio kao najbolji model:

| Metrika | Vrednost |
| R² | 0.9752 |
| MAE | 8,295 EUR |
| RMSE | 11,348 EUR |
| MAPE | 7.22% |

Analiza važnosti atributa pokazala je da **površina objekta (`Area_m2`) odnosi čak 56.94% važnosti** pri donošenju odluke modela.

---

## 📦 Deployment i Eksportovanje Model-a
Projekat uključuje implementaciju faza za puštanje u rad (deployment):
* Model je uspešno serijalizovan (zamrznut) u fajl `model_nekretnine.pkl` pomoću biblioteke `joblib`.
* Sačuvan je i skaler `standard_scaler.pkl` za potrebe transformacije novih, realnih unosa.
* Kreiran je interaktivni kalkulator u kodu koji omogućava trenutnu procenu cene nekretnine na osnovu proizvoljnih korisničkih inputa.

