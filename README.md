# Suivi des observations d'especes

Projet XML : modelisation d'observations d'especes animales (espece, lieu, annee, nombre d'individus) en deux versions.

## Fichiers

| Fichier | Role |
|---|---|
| `observations1.dtd` | Regles de la version 1 (DTD) |
| `observations1.xml` | Donnees version 1, validees par la DTD |
| `observations2.xsd` | Regles de la version 2 (XML Schema) |
| `observations2.xml` | Donnees version 2, validees par le schema |

## Version 1 (DTD)

Chaque `observation` contient directement toutes ses informations : l'espece (nom, description), le lieu (nom, latitude, longitude), l'annee et le nombre.

- Racine : `observations`
- L'`id` d'une observation est de type `ID` (unique).
- Les `id` d'espece et de lieu sont de type `NMTOKEN`, car une meme espece ou un meme lieu se repete dans plusieurs observations.

Inconvenient : les especes et les lieux sont recopies a chaque observation.

## Version 2 (XSD)

Chaque lieu et chaque espece n'est decrit qu'une seule fois, et les observations y font reference.

- Racine : `suivi`, avec trois parties : `lieux`, `especes`, `observations`.
- Une observation pointe vers une espece et un lieu avec l'attribut `ref`.
- Contraintes :
  - `xs:key` / `xs:keyref` : les `id` de lieux et d'especes sont uniques et chaque `ref` doit exister.
  - `xs:unique` : une seule observation par espece, lieu et annee.
  - `annee` : `xs:gYear` a partir de 1900.
  - `latitude` entre -90 et 90, `longitude` entre -180 et 180.
  - `nombre` : entier positif ou nul.

## Validation

```bash
xmllint --noout --valid observations1.xml
xmllint --noout --schema observations2.xsd observations2.xml
```
