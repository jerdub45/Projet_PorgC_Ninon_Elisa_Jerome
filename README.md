# 1. En-tête
Intitulé du TD : TD stockage de matrices en C.
| Prénom NOM                | Identifiant GitHub |  
|            ---            |         ---        |
| Ninon LE PARQUIC--SANTAIS | ninonlps           |
| Elisa SIEGEL              | elisasiegel        |
| Jérôme DUBOCQ             | jerdub45           |

Groupe TD : TD3

Date de rendu :

# 2. Introduction
Ce projet est réalisé dans le cadre du cours de programmation C. Il s'agit d'une bibliothèque C ayant pour but la manipulation de matrices de coefficients entiers et les calculs matriciels associés. Cette bibliothèque se découpe en plusieurs modules qui possèdent chacun une utilité qui leur est propre. Les objectifs de ce TD sont :

* Bien faire la différence entre le contrat (.h) et l'implémentation (.c)
* Apprendre à compiler différents modules séparéments
* Apprendre à manipuler correctement la mémoire
* Apprendre à gérer les erreurs et mettre en place des tests
* Apprendre à lire et écrire des données dans un fichier texte

# 3. Conception

# 4. Réalisation
## Exercice 1
### Question (1.a)
```c
  /* (1.a) */
  case MAT_ERR_DIMENSIONS:  return "dimensions incompatibles";
  case MAT_ERR_DEPASSEMENT: return "résultat hors de l'intervalle des int";
  /* (fin 1.a) */
```
Vérification :

![capture_1.a](captures/capture_1.a.png)

### Question (1.b)
```c
  /* (1.b) */
  return m != NULL
    && m->coef != NULL
    && m->nb_lignes >= 1 && m->nb_lignes <= MAT_DIM_MAX
    && m->nb_colonnes >= 1 && m->nb_colonnes <= MAT_DIM_MAX;
  /* (fin 1.b) */
```
Vérification :

![capture_1.b](captures/capture_1b.png)

### Question (1.c)
```c
  /* (1.c) si nb_lignes ou nb_colonnes n'est pas compris
   *       entre 1 et MAT_DIM_MAX, renvoyer MAT_ERR_ARGUMENT.          */
  if ( (nb_lignes < 1 || nb_lignes > MAT_DIM_MAX)
    || (nb_colonnes < 1 || nb_colonnes > MAT_DIM_MAX) )
  {
    return MAT_ERR_ARGUMENT;
  }
  /* fin (1.c) */
```
Vérification :

![capture_1.c](captures/capture_1c.png)

### Question (1.d)
```c
    /* (1.d) */
    for (int i = 0; i < nb_lignes; i++) {
        coef[i] = mat_calloc(nb_colonnes, sizeof(int));     // chaque ligne contient nb_collonnes entiers
        if (coef[i] == NULL) {                  // si une ligne est NULL, 
            for (int k = i - 1; k >= 0; k--)    // on libère toutes les lignes précédentes
                free(coef[k]);                  
            free(coef);                         // et le tableau de pointeurs
            return MAT_ERR_ALLOCATION;
        }
    }
```
En cas d'échec il faut rendre non seulement toutes les lignes précédemment allouées mais aussi le tableau coef lui-même.

![dessin](captures/dessin_1d.png)

Compilation/test :

![capture](captures/capture_1d.png)

On remarque que l'échec d'allocation simulé à bien fonctionné mais que la vérification que les coefficients sont nuls a échouée. Dans un premier temps il nous a semblé qu'un oubli de notre part avait dû être commis mais après réflexion nous pensons que c'est parce qu'à ce stade la structure *res n'est pas encore mise à jour avec les pointeurs fraichement alloués. Cela se vérifiera à la question suivante.

### Question (1.e)
```c
  /* (1.e) : maintenant que tout a réussi, remplir *res */
  res->coef = coef;
  res->nb_lignes = nb_lignes;
  res->nb_colonnes = nb_colonnes;
  return MAT_OK;
  /* fin (1.e) */
```
Vérification :

![capture](captures/capture_1e.png)

En effet, il suffisait d'attendre la question suivante pour pouvoir faire la vérification.

### Question (1.f)
```c
    /* (1.f) : si m->coef n'est pas NULL alors tout libérer */
    if (m->coef != NULL) {
        for (int i = 0; i < m->nb_lignes; i++) {
            free(m->coef[i]);  // libérer chaque ligne
        }
        free(m->coef);         // libérer le tableau de pointeurs lui-même
    }
    /* fin (1.f) */
```
Vérification :

![capture](captures/capture_1f.png)

Valgrind :

![capture](captures/valgrind_1f.png)

Valgrind nous indique bien : `All heap blocks were freed – no leaks are possible` comme attendu pour valider.

# 5. Vérification mémoire

# 6. Démonstration finale

# 7. Bilan et difficultés
