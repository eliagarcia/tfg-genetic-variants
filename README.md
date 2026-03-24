# tfg-genetic-variants
Codi del meu TFG sobre variants genètiques.


## Descripció Dataset  – Genetic Variant Classification

## Tipus d’informació del dataset

El dataset utilitzat combina tres tipus principals d’informació, provinents de diferents fonts i amb diferents nivells de processament:

### 1. Dades del variant (crues)
- **Variables:** `CHROM`, `POS`, `REF`, `ALT`
- Descriuen el canvi genètic i la seva localització al genoma.
- Representen la informació més bàsica i directa del variant.
- No incorporen interpretació biològica ni clínica.

### 2. Anotacions de ClinVar (clíniques)
- **Variables:** `CLNDN`, `CLNDISDB`, `AF_*`, `CLNVC`, `CLASS`, etc.
- Provenen de la base de dades ClinVar.
- Inclouen informació clínica i poblacional:
  - Malalties associades
  - Freqüències en poblacions
  - Tipus de variant
  - Consens o conflicte entre laboratoris (`CLASS`)
- Representen coneixement mèdic acumulat.

### 3. Anotacions VEP i predictors (bioinformàtica)
- **Variables:** `Consequence`, `IMPACT`, `EXON`, `Protein_position`, `SIFT`, `PolyPhen`, `CADD_*`, etc.
- Generades amb Ensembl VEP i altres eines bioinformàtiques.
- Descriuen l’efecte potencial del variant:
  - Impacte sobre la proteïna
  - Conseqüències funcionals
- Inclouen:
  - Regles biològiques (ex: `missense_variant`)
  - Prediccions d’eines externes (ex: `SIFT`, `CADD`)



#### Taula 1 – Dades del variant (crues)
| Variable | Tipus      | Comentari            |
| -------- | ---------- | -------------------- |
| CHROM    | categòrica | Cromosoma            |
| POS      | numèrica   | Posició genòmica     |
| REF      | categòrica | Al·lel de referència |
| ALT      | categòrica | Al·lel alternatiu    |

#### Taula 2 – Anotacions ClinVar (clíniques)
| Variable     | Tipus               | Derivada? | Comentari                                  |
| ------------ | ------------------- | --------- | ------------------------------------------ |
| AF_ESP       | numèrica            | No gaire  | Freqüència poblacional                     |
| AF_EXAC      | numèrica            | No gaire  | Freqüència poblacional                     |
| AF_TGP       | numèrica            | No gaire  | Freqüència poblacional                     |
| CLNDISDB     | text multivalor     | Sí        | Bases de dades de malalties                |
| CLNDISDBINCL | text multivalor     | Sí        | Similar a l’anterior                       |
| CLNDN        | text multivalor     | Sí        | Noms de malalties                          |
| CLNDNINCL    | text multivalor     | Sí        | Similar a l’anterior                       |
| CLNHGVS      | text estructurat    | Sí        | Notació HGVS                               |
| CLNSIGINCL   | text multivalor     | Sí        | Significances incloses                     |
| CLNVC        | categòrica          | Sí (lleu) | Tipus de variant                           |
| CLNVI        | text multivalor     | Sí        | Identificadors externs                     |
| MC           | text estructurat    | Sí        | Consequence + codi SO                      |
| ORIGIN       | categòrica/numèrica | Sí        | Origen de la variant                       |
| SSR          | categòrica/numèrica | Sí        | Flag                                       |
| CLASS        | binària             | Sí        | Variable objectiu (conflicte/no conflicte) |


#### Taula 3 – Anotacions VEP i predictors
| Variable           | Tipus              | Derivada? | Comentari                      |
| ------------------ | ------------------ | --------- | ------------------------------ |
| Allele             | categòrica         | Sí        | Al·lel usat per VEP            |
| Consequence        | categòrica         | Sí        | Tipus de conseqüència          |
| IMPACT             | categòrica ordinal | Sí        | Severitat                      |
| SYMBOL             | categòrica         | Sí        | Gen                            |
| Feature_type       | categòrica         | Sí        | Transcript / RegulatoryFeature |
| Feature            | categòrica         | Sí        | ID del transcrit               |
| BIOTYPE            | categòrica         | Sí        | Tipus de gen                   |
| EXON               | text estructurat   | Sí        | Format x/y                     |
| INTRON             | text estructurat   | Sí        | Format x/y                     |
| cDNA_position      | numèrica/text      | Sí        | Posició relativa               |
| CDS_position       | numèrica/text      | Sí        | Posició relativa               |
| Protein_position   | numèrica/text      | Sí        | Posició proteïna               |
| Amino_acids        | text estructurat   | Sí        | Canvi aminoàcid                |
| Codons             | text estructurat   | Sí        | Canvi de codó                  |
| DISTANCE           | numèrica           | Sí (lleu) | Distància a gen                |
| STRAND             | categòrica         | Sí (lleu) | 1 o -1                         |
| BAM_EDIT           | flag               | Sí        | Anotació tècnica               |
| SIFT               | categòrica         | Sí (alt)  | Predictor funcional            |
| PolyPhen           | categòrica         | Sí (alt)  | Predictor funcional            |
| MOTIF_NAME         | categòrica         | Sí        | Regió reguladora               |
| MOTIF_POS          | numèrica           | Sí        | Posició                        |
| HIGH_INF_POS       | flag               | Sí        | Alta informació                |
| MOTIF_SCORE_CHANGE | numèrica           | Sí        | Canvi score                    |
| LoFtool            | numèrica           | Sí (alt)  | Score extern                   |
| CADD_PHRED         | numèrica           | Sí (alt)  | Score de deleterietat          |
| CADD_RAW           | numèrica           | Sí (alt)  | Score brut                     |
| BLOSUM62           | numèrica           | Sí (alt)  | Score substitució AA           |



## Variables clau de predicció funcional

Algunes variables del dataset tenen un paper especialment rellevant perquè resumeixen informació funcional o estructural sobre la variant. Aquestes variables no són dades crues, sinó anotacions o prediccions generades per eines bioinformàtiques.

### SIFT
- Predictor bioinformàtic que estima si una substitució aminoacídica pot afectar la funció de la proteïna.
- Valors típics:
  - `tolerated`
  - `deleterious`
  - `deleterious_low_confidence`

### PolyPhen
- Predictor que avalua si un canvi aminoacídic pot afectar l’estructura o la funció de la proteïna.
- Categories:
  - `benign`
  - `possibly_damaging`
  - `probably_damaging`

### CADD_PHRED
- Score numèric de deleterietat.
- Valors més alts indiquen variants més probablement perjudicials.

### CADD_RAW
- Versió crua del score CADD.
- Mesura contínua abans de l’escalat tipus PHRED.

### Consequence
- Anotació funcional del tipus de variant.
- Exemples:
  - `missense_variant`
  - `synonymous_variant`
  - `5_prime_UTR_variant`

### IMPACT
- Classificació simplificada de la severitat associada a `Consequence`.
- Valors típics:
  - `HIGH`
  - `MODERATE`
  - `LOW`
  - `MODIFIER`

> Aquestes variables aporten coneixement funcional precomputat i poden tenir un alt poder predictiu en els models.

## Procés de construcció del dataset

El dataset final no prové directament del fitxer VCF original de ClinVar, sinó que es construeix combinant informació clínica amb anotacions funcionals generades per Ensembl VEP.

### 1. Lectura del fitxer original (`clinvar.vcf.gz`)
S’extreuen variables com:
- `AF_ESP`, `AF_EXAC`, `AF_TGP`
- `CLNDN`, `CLNDISDB`, `CLNHGVS`
- `CLNVC`, `CLNVI`, `MC`, `ORIGIN`, `SSR`

### 2. Construcció de la variable objectiu (`CLASS`)
- `CLASS = 0` → sense conflicte clínic  
- `CLASS = 1` → amb conflicte (`CLNSIGCONF` no nul)

### 3. Filtrat de variants
Només es conserven variants amb múltiples submitters:
- `criteria_provided,_multiple_submitters,_no_conflicts`
- `criteria_provided,_conflicting_interpretations`

### 4. Eliminació de columnes
S’eliminen:
- No necessàries: `ALLELEID`, `RS`, `DBVARID`
- Fuita de target: `CLNSIG`, `CLNSIGCONF`, `CLNREVSTAT`
- Redundants: `CLNVCSO`, `GENEINFO`

### 5. Lectura del fitxer anotat amb VEP (`clinvar.annotated.vcf.gz`)
Inclou el camp `CSQ` amb anotacions funcionals.

### 6. Parseig del camp `CSQ`
El camp es descompon en múltiples variables:
- `Consequence`, `IMPACT`
- `EXON`, `INTRON`
- `Protein_position`
- `SIFT`, `PolyPhen`
- `CADD_PHRED`, `CADD_RAW`, `BLOSUM62`

### 7. Fusió final i exportació
Es combinen totes les anotacions en un dataset final:
- `clinvar_conflicting.csv`


## Redundància i derivació entre variables

Algunes variables del dataset no són independents, sinó que contenen informació parcialment redundant o derivada:

- `IMPACT` depèn directament de `Consequence`
- `MC` i `Consequence` contenen informació funcional similar
- `SIFT`, `PolyPhen`, `CADD_PHRED`, `CADD_RAW`, `LoFtool` i `BLOSUM62` són predictors o scores externs
- `AF_ESP`, `AF_EXAC` i `AF_TGP` són mesures de freqüència poblacional potencialment correlacionades
- `EXON`, `INTRON`, `cDNA_position`, `CDS_position` i `Protein_position` descriuen la localització funcional del variant a diferents nivells
- `Amino_acids` i `Codons` descriuen el canvi molecular de manera complementària

Això implica que el dataset combina informació crua amb informació ja processada per eines externes, fet que cal tenir en compte tant en la interpretació dels models com en l’anàlisi d’importància de variables.


## Fusió final i exportació a CSV
Finalment, es combinen les anotacions de ClinVar amb les anotacions de VEP en un únic fitxer final: `clinvar_conflicting.csv`.
