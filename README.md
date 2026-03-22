# tfg-genetic-variants
Codi del meu TFG sobre variants genètiques.


## Descripció Dataset  – Genetic Variant Classification

#### Tipus d’informació del dataset
El dataset utilitzat combina tres tipus principals d’informació, provinents de diferents fonts i amb diferents nivells de processament:
#### 1. Dades del variant (crues)
Variables: CHROM, POS, REF, ALT
Descriuen el canvi genètic i la seva localització al genoma.
Representen la informació més bàsica i directa del variant.
No incorporen interpretació biològica ni clínica.

#### 2. Anotacions de ClinVar (clíniques)
Variables: CLNDN, CLNDISDB, AF_*, CLNVC, CLASS, etc.
Provenen de la base de dades ClinVar.
Inclouen informació clínica i poblacional:
Malalties associades
Freqüències en poblacions
Tipus de variant
Consens o conflicte entre laboratoris (CLASS)
Representen coneixement mèdic acumulat.

#### 3. Anotacions VEP i predictors (bioinformàtica)
Variables: Consequence, IMPACT, EXON, Protein_position, SIFT, PolyPhen, CADD_*, etc.
Generades amb Ensembl VEP i altres eines bioinformàtiques.
Descriuen l’efecte potencial del variant:
Impacte sobre la proteïna
Conseqüències funcionals
Inclouen:
Regles biològiques (ex: missense_variant)
Prediccions d’eines externes (ex: SIFT, CADD)



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


