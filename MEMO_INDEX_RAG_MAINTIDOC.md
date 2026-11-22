# MaintiDoc — Comprendre les fichiers de l’index RAG

> **À retenir**  
> `chroma.sqlite3` conserve les informations sur les documents et les collections. Les fichiers `.bin` permettent à Chroma de retrouver rapidement les vecteurs proches. `images/` et `index_termine` se trouvent à côté de Chroma, dans le dossier géré par MaintiDoc.

## Où se trouve chaque élément ?

Dans VS Code, l’option **Compact Folders** peut afficher deux dossiers imbriqués sur une seule ligne. Ici, `v1-…` se trouve dans `.maintidoc_rag/`. Les identifiants longs sont abrégés ci-dessous.

```text
MAINTiDOC-AutoVision/
  .maintidoc_rag/
    v1-…/
      chroma/
        17e3ce76-…/
          data_level0.bin
          header.bin
          index_metadata.pickle
          length.bin
          link_lists.bin
        chroma.sqlite3
      images/
      index_termine
  agents/
```

| Élément | Rôle |
| --- | --- |
| `.maintidoc_rag/` | Dossier local où le projet conserve les résultats de l’indexation. |
| `v1-…/` | Dossier d’une instance de l’index. Le suffixe ressemble à une empreinte. Le code de MaintiDoc permet de vérifier à partir de quoi il est calculé. |
| `chroma/` | Dossier de stockage persistant de Chroma. Il rassemble la base SQLite et les fichiers de recherche vectorielle. |
| `chroma.sqlite3` | Base de données locale de Chroma. Elle peut contenir les textes transmis lors de l’indexation, leurs identifiants, leurs métadonnées et la configuration des collections. Des champs comme le modèle du véhicule ou la page existent seulement si l’application les enregistre. |
| `17e3ce76-…/` | Dossier créé par Chroma pour le segment vectoriel. Le nom long est l’identifiant interne du segment. |
| `images/` | Dossier dédié aux fichiers image de cette instance de l’index. La capture ne montre ni son contenu ni l’usage exact des images dans la recherche. |
| `index_termine` | Son nom suggère un repère écrit quand l’indexation se termine. Le code indique à quel moment il est créé et comment il est utilisé au démarrage. |
| `agents/` | Dossier du projet visible à la racine. Pour savoir quels agents y sont définis, ouvrir les fichiers du dossier. |

## Que font les fichiers internes de Chroma ?

Un **embedding** est un vecteur, c’est-à-dire une liste de nombres qui représente un contenu. Pour chercher des passages pertinents, Chroma peut comparer l’embedding d’une question à ceux des passages indexés. **HNSW** organise les vecteurs dans un graphe de voisinage pour accélérer cette recherche.

| Fichier | À quoi il sert |
| --- | --- |
| `header.bin` | Informations techniques nécessaires pour relire l’index, comme ses paramètres de stockage. |
| `data_level0.bin` | Données du niveau de base de l’index HNSW, dont les vecteurs et leurs liens de voisinage à ce niveau. |
| `link_lists.bin` | Listes de liens entre les points dans les niveaux supérieurs du graphe. |
| `length.bin` | Fichier technique de la représentation persistante de l’index. Son format dépend de l’implémentation utilisée par Chroma. |
| `index_metadata.pickle` | Métadonnées internes qui relient notamment les identifiants des éléments aux numéros utilisés dans l’index vectoriel. |

Les fichiers de `chroma/` fonctionnent ensemble. Pour consulter la collection, utilise l’API Chroma depuis le projet. Pour comprendre comment elle a été construite, lis d’abord le code d’indexation.

[Documentation du stockage de Chroma](https://cookbook.chromadb.dev/core/storage-layout/)

## Exemple avec un voyant du tableau de bord

1. **Indexation** — MaintiDoc découpe un manuel en passages et calcule un embedding pour chaque passage indexé. Il peut associer au passage le nom du manuel et le numéro de page.
2. **Recherche** — Pour « Que signifie le voyant de pression des pneus ? », le système calcule un embedding de la question et demande à Chroma les passages proches.
3. **Réponse** — Le modèle reçoit les passages retrouvés et les utilise pour expliquer le voyant. Si le projet transmet aussi une image liée au passage, le modèle peut s’en servir selon le pipeline configuré.

## Si je bloque, où regarder ?

| Question | Où chercher |
| --- | --- |
| Qu’est-ce que je stocke à part les vecteurs ? | Dans le code, repère l’ajout des données à Chroma et regarde les champs `ids`, `documents` et `metadatas`. |
| Pourquoi le projet réutilise-t-il un index existant ? | Dans VS Code, cherche `index_termine` dans tout le projet avec **Ctrl+Shift+F**. Regarde à la fois où il est écrit et où sa présence est vérifiée. |
| Pourquoi y a-t-il une longue chaîne après `v1-` ? | Cherche `.maintidoc_rag` dans le code et suis la construction du chemin du dossier. |
| Comment une image est-elle liée à son passage ? | Cherche où le dossier `images` reçoit ses fichiers et où un nom ou un chemin d’image est associé aux métadonnées. |
| Que veut dire le `U` vert ? | Git indique **Untracked**. Le fichier existe sur ton PC et Git ne le suit pas encore. Vérifie avec `git status --short`. Si l’index est généré localement à partir des sources, la règle `.maintidoc_rag/` dans `.gitignore` permet d’ignorer ces fichiers générés. |
