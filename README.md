# Predicció de conflictes entre laboratoris i significança clínica de variants genètiques amb aprenentatge automàtic

Treball de Fi de Grau — Classificació automàtica de variants genètiques de ClinVar mitjançant tècniques d'aprenentatge automàtic supervisat.

---

## Descripció

Aquest projecte aplica sis models de classificació supervisada per predir dues tasques sobre variants genètiques extretes de la base de dades **ClinVar**:

- **CLASS** (conflicte entre laboratoris): classificació binària — *No conflictiu* (0) vs. *Conflictiu* (1).
- **CLNSIG** (significança clínica): classificació multiclasse en tres categories — *Benign* (0), *VUS / Uncertain significance* (1) i *Pathogenic* (2).

Per a cadascuna de les dues tasques s'experimenta amb dos conjunts de features:

- **Anotat**: inclou les anotacions funcionals de **VEP** (*Variant Effect Predictor*), com SIFT, PolyPhen, CADD, BLOSUM62, LoFtool, etc.
- **Bàsic**: atributs intrínsecs de la variant sense anotació funcional externa.

---

## Estructura del repositori

```
.
├── data/
│   └── raw/
│       ├── clinvar_clnsig_annotated.csv   # Dataset CLNSIG anotat
│       ├── clinvar_clnsig_basic.csv       # Dataset CLNSIG bàsic (generat al notebook)
│       ├── clinvar_conflicting.csv        # Dataset CLASS anotat
│       └── clinvar_class_basic.csv        # Dataset CLASS bàsic (generat al notebook)
├── notebooks/
│   ├── clnsig_basic.ipynb                 # CLNSIG — dataset bàsic
│   ├── clnsig_annotated.ipynb             # CLNSIG — dataset anotat (VEP)
│   ├── class_basic.ipynb                  # CLASS — dataset bàsic
│   └── class_annotated.ipynb             # CLASS — dataset anotat (VEP)
└── README.md
```

---

## Models implementats

Cada notebook entrena i avalua els sis mateixos models:

1. **LinearSVC** (`LinearSVC`)
2. **Logistic Regression L1** — Lasso (`LogisticRegression(penalty='l1')`)
3. **Logistic Regression L2** — Ridge (`LogisticRegression(penalty='l2')`)
4. **Decision Tree** (`DecisionTreeClassifier`)
5. **Random Forest** (`RandomForestClassifier`)
6. **MLP** — Xarxa neuronal multicapa (`MLPClassifier`)

L'optimització d'hiperparàmetres es fa amb **`GridSearchCV`** i **`RandomizedSearchCV`** amb validació creuada estratificada (`StratifiedKFold`).

---

## Pipeline de preprocessament

Per a tots els notebooks s'aplica el mateix pipeline:

- Eliminació de columnes amb >95% de valors nuls i columnes VEP (en versió bàsica).
- Imputació de valors faltants: mediana per a numèriques, moda per a categòriques (`SimpleImputer`).
- Escalat de variables numèriques amb `StandardScaler`.
- Codificació de variables categòriques amb `OneHotEncoder` (`min_frequency=20`).
- Partició **90% train+val / 10% test** estratificada (`random_state=42`).

---

## Avaluació i interpretabilitat

Mètriques reportades per a cada model:

- F1-score macro i per classe
- Accuracy
- ROC-AUC (one-vs-rest)
- Matriu de confusió

Interpretabilitat amb **SHAP**:

- Summary plots (importància global de features)
- Bar charts comparatius per classe
- Anàlisi de les features més rellevants per a cada model

---

## Requisits

```
python >= 3.9
pandas
numpy
scikit-learn
matplotlib
seaborn
shap
scipy
```

---

## Dades

Les dades provenen de la base de dades pública [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/) del NCBI. Les anotacions funcionals s'han obtingut amb l'[Ensembl Variant Effect Predictor (VEP)](https://www.ensembl.org/Tools/VEP).

---

Treball de Fi de Grau — Enginyeria Informàtica
Universitat de Barcelona · 2026

Èlia Garcia Rovira
