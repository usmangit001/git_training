# Aprén els conceptes de `git`, no les ordres

**Un tutorial interactiu Git destinat a ensenyar-vos com funciona Git, no només quins ordres d'executar.**

Per tant, voleu utilitzar Git, veritat?

Però no només voleu aprendre ordres, voleu entendre el que utilitzeu?

Aleshores, això està pensat per a vosaltres!

Comencem!

---

> Basat en el concepte general de la publicació del bloc de Rachel M. Carmena sobre [How to teach Git](https://rachelcarmena.github.io/2018/12/12/how-to-teach-git.html).
>
> Si bé trobe que molts tutorials de Git a Internet estan massa centrats en què fer en lloc de com funcionen les coses,
> el recurs més inestimable per a tots dos (i font per a aquest tutorial!) És el [llibre de Git](https://git-scm.com/book/en/v2) i
> la [pàgina de referència](https://git-scm.com/docs).
> 
> Així que si encara us interessa quan hageu acabat, reviseu-los. Espere que el concepte un poc diferrent diferent d’aquest tutorial
> t'ajudarà a comprendre totes les altres funcions de Git detallades.

---
- [Aprén els conceptes de `git`, no les ordres](#aprén-els-conceptes-de-git-no-les-ordres)
  - [Visió general](#visió-general)
  - [Obtindre un repositori _Remot_ (clone)](#obtindre-un-repositori-remot)
  - [Afegint noves coses](#afegint-noves-coses)
  - [Fent canvis](#fent-canvis)
  - [Branques](#branques)
  - [Fusionar (merge)](#fusionar)
    - [Fusió directa](#fusió-directa)
    - [Fusió de diferents branques](#fusió-de-diferents-branques)
    - [Resolució de conflictes](#resolució-de-conflictes)
  - [Rebase](#rebase)
    - [Resolució de conflictes](#resolució-de-conflictes-1)
  - [Actualització de l'_Entorn de Desenvolupament_ amb canvis remots](#actualitzacio-de-lentorn-de-desenvolupament-amb-canvis-remots)
   - [Obtindre canvis (fetch)](#obtindre-canvis)
   - [Incorporar canvis (pull)](#obtindre-i-incorporar-canvis)
   - [Guardar canvis (stash)](#guardar-canvis-stash)
   - [Incorporar canvis amb conflictes](#incorporar-canvis-amb-conflictes)
  - [Selecció de canvis (cherry picking)](#seleccio-de-canvis)
  - [Reescriure l'història](#reescriure-lhistòria)
    - [Modificar l'ultim canvi](#modificar-lultim-canvi)
    - [Rebase interactiu](#rebase-interactiu)
    - [Arreglar un canvi en el passat](#arreglar-un-commit-en-el-passat)
    - [Història pública, per què no hauries de reescriure-la, però com fer-ho d'una manera segura](#historia-publica-per-que-no-hauries-de-reescriurela-pero-com-fer-ho-duna-manera-segura)
  - [Llegir l'història](#llegir-lhistoria)
---

## Visió general

A la imatge següent, veieu quatre caixes. Un d’elles es troba sola, mentre que les altres tres s’agrupen en el que anomenaré el vostre _Entorn de Desenvolupament_.

![git components](../../img/components.png)

Començarem amb el que es troba per sol. El _Desòsit Remot o Remote Repository_ és on envieu els vostres canvis quan vulgueu compartir-los amb altres persones
i d'on obteniu els canvis. Si heu utilitzat altres sistemes de control de versions, no hi ha res interessant.

L’_Entorn de Desenvolupament_ és el que teniu a la vostra màquina local.

Les tres parts d’aquest són el vostre _Directori de Treball o Working Directory_, l’_Àrea de Preparació o Staging Àrea_ i el _Repositori Local_.
Aprendrem més sobre aquells quan comencem a utilitzar Git.

Trieu un lloc on vulgueu posar el vostre _Entorn de Desenvolupament_.

Només cal que aneu a la vostra carpeta d'inici o allà on vulgueu posar els vostres projectes. No cal crear una carpeta nova per al vostre _Entorn de Desenvolupament_

## Obtindre un repositori _Remot_ (clone)

Ara volem agafar un _Repositori Remot_ i posar el que hi ha a la vostra màquina.

Suggeriria que utilitzem aquest ([https://github.com/unseenwizzard/git_training.git](https://github.com/unseenwizzard/git_training.git)
si no esteu llegint açò a GitHub encara).

> Per fer-ho, pots utilitzar `git clone https://@github.com/Unseenwizzard/git_training.git`
> 
> But as following this tutorial will need you to get the changes you make in your _Dev Environment_ back to the _Remote Repository_, and github doesn't just allow anyone to do that to anyone's repo, you'll best create a _fork_ of it right now. There's a button to do that on the top right of this page. 
>
> Però, a mesura que seguiu aquest tutorial, necessitareu afegir els canvis que feu al vostre
> _Entorn de Desenvolupament_ de nou al _Repositori Remot_ i GitHub no deixa a qualsevol persona
> modificar el repositori de qualsevol persona. El millor seria crear un _fork_. Hi ha un botó per fer-ho a la part superior dreta d'aquesta pàgina.

Ara que teniu una còpia del meu _Repositori Remot_, és hora de posar-la a la vostra màquina.

Per això, utilitzem `git clone https://github.com/{EL VOSTRE NOM D'USUARI}/git_training.git`

Com es pot veure al diagrama següent, això copia el _Deòsit Remot_ en dos llocs, el vostre _Direcotri de Treball_ i el _Repositori Local_.

Ara veieu com Git és un control de la versions _distribuït_. El _Repositori Local_ és una còpia del _Remote_, i actua de la mateixa manera.
L’única diferència és que no el compartiu amb ningú.

El que també fa `git clone` és crear una carpeta nova allà on el vau cridar. Hi hauria d'haver una carpeta `git_training`. Obri-la.

![Cloning the remote repo](../../img/clone.png)


## Afegint noves coses

Algú ja ha posat un fitxer anomenat `Alice.txt` al _Repositori Remot_.
Éstà un poc solitari allí, així que creem un fitxer nou i anomenem-lo `bob.txt`.

El que has acabat de fer és afegir el fitxer al vostre _Directori de Treball_
Hi ha dos tipus de fitxers _Directori de Treball_:
Fitxers _localitzats (tracked)_ que Git coneix i
_no localitzats (untracked)_ fitxers que Git no coneix (encara).

Per veure què passa al vostre _Directori de Treball_ pots executar
`git status`, que et dirà en quina branca esteu, si el vostre _Repositori Local_
és diferent del _remot_ i l'estat de fitxers _tracked_ i _untracked_.

Veureu que `bob.txt` està untracked i, fins i tot, `git status` et diu
com canviar-ho. A la imatge següent, podeu veure què passa quan seguiu
els consells i executeu `git add bob.txt`: heu afegit el fitxer a
l'_Àrea de Preparació o Staging Area_, on recopileu tots els canvis que
voleu fer al _repositori_.

![Afegint canvis a l’àrea d’assaig](../../img/add.png)

Quan hàgeu afegit tots els canvis (que ara mateix només està Bob),
esteu preparats per a _fer canvis o commit_ el que heu fet al _Repositori Local_.

Els canvis recopilats que fas (_commit_) són un tros de treball significatiu,
de manera que quan ara executeu `git commit` un editor de text s'obrirà i
et permetrà escriure un missatge dient tot el que has fet.
Quan guardeu i tanqueu el fitxer de missatges, el vostre _commit_ s’afegeix
al _Repositori Local_.

![Fent canvis al repositori local](../../img/commit.png)

També podeu afegir el vostre missatge de _commit_ a la línia d’ordres
si crideu a `git commit` així:` git commit -m "afegeix bob" `.
Però perquè voleu escriure [bons missatges de commit](https://chris.beams.io/posts/git-commit/)
realment hauríeu de prendre-vos el vostre temps i utilitzar l’editor.

Ara els vostres canvis es troben al vostre dipòsit local,
que és un bon lloc perquè estiguen sempre que ningú més els necessite
o encara no estigueu preparats per compartir-los.

Per tal de compartir els vostres canvis amb el _Depòsti Remot_,
heu de enviar-los amb l'ordre `git push`.

![Pushing repositori local](../../img/push.png)

Una vegada executeu `git push`, els canvis s'enviaran al _Repositori Remot_.
Al diagrama següent, veieu l'estat després del vostre `push`.

![Estat de tots els components despres de push](../../img/after_push.png)


## Fent canvis
Fins ara només hem afegit un fitxer nou. Evidentment, la part més interessant
del control de versions és la modificació de fitxers.

Fes una ullada a `Alice.txt`.

Conté algun text, però `Bob.txt` no, així que anem a modificar-lo i posar-hi
`Hi!! I'm Bob, I'm new here.`

Si executes `git status`, voràs que `Bob.txt` està _modified_.

En aquest estat, els canvis només es troben al teu _Directori de Treball_.

Si vols veure què ha canviat al teu _Direcoti de Treball_,
pots executar `git diff` i ara voràs això:

```Diff
diff --git a/Bob.txt b/Bob.txt
index e69de29..3ed0e1b 100644
--- a/Bob.txt
+++ b/Bob.txt
@@ -0,0 +1 @@
+Hi!! I'm Bob. I'm new here.
```

Segueix endavant i executa `git add Bob.txt` com has fet abans.
Com sabem, això mou els canvis a l'_Àrea de Preparació_.

Volem veure els canvis que acabem d'afegir (_stated_), així que tornem a
mostrar la diferència amb `git diff`! Veureu que aquesta vegada l'eixida
està buida. Això succeeix perquè `git diff` funciona només en els canvis al
teu _Directori de Treball_.

Per mostrar quins canvis són ja s'han afegit (_staged_), podem utilitzar
`git diff --staged` i veurem les mateixes diferències que abans.

Acabe d'adonar-me que vam hem posat dos signes d'exclamació després de 'Hola'.
No m'agrada, així que tornem a canviar `Bob.txt`, de manera que només siga "Hola!".

Si ara executem `git status`, veurem que hi ha dos canvis:
el que ja hem afegit (_stated_) on hem afegit text, i el que acabem de fer,
que encara només es troba al directori de treball.

Podem fer una ullada a la `git dif` entre el _Directori de Treball_ i el que
ja haviem traslladat a l'_Àrea de Preparació_, per mostrar el que ha canviat des que
vam afegir per última vegada (_stage_) i els nostres canvis per fer un _commit_.

```Diff
diff --git a/Bob.txt b/Bob.txt
index 8eb57c4..3ed0e1b 100644
--- a/Bob.txt
+++ b/Bob.txt
@@ -1 +1 @@
-Hi!! I'm Bob. I'm new here.
+Hi! I'm Bob. I'm new here.
```

Com que el canvi és el que volíem fer, anem a afegir l’estat actual del fitxer
mitjançant `git add Bob.txt`

Ara estem preparats per _cometre_ `commit` el que acabem de fer.
Executem `git commit -m "Add text to Bob"` perquè per a un canvi tan xicotet
escriure una línia seria suficient.

Com sabem, els canvis es troben ara al _Repositori local_.

Encara podríem saber quin canvi ens acabem de _cometre (commit)_
i què hi havia abans.

Podem fer-ho comparant els «commits».

Tots els «commits» de Git tenen un hash únic pel qual es referèncien.

Si fem una ullada al `git log`, no només veurem una llista de tots
els «commits» amb el seu _hash_, així com l'_Autor_ i la _Data_, sinó també
veurem l'estat del _Repositori Local_ i la última informacó local sobre les 
_branques remotes_.

Ara mateix, el `git log` és una cosa així:

```ShellSession
commit 87a4ad48d55e5280aa608cd79e8bce5e13f318dc (HEAD -> master)
Author: {YOU} <{YOUR EMAIL}>
Date:   Sun Jan 27 14:02:48 2019 +0100

    Add text to Bob

commit 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e (origin/master, origin/HEAD)
Author: {YOU} <{YOUR EMAIL}>
Date:   Sun Jan 27 13:35:41 2019 +0100

    Add Bob

commit 71a6a9b299b21e68f9b0c61247379432a0b6007c 
Author: UnseenWizzard <nicola.riedmann@live.de>
Date:   Fri Jan 25 20:06:57 2019 +0100

    Add Alice

commit ddb869a0c154f6798f0caae567074aecdfa58c46
Author: Nico Riedmann <UnseenWizzard@users.noreply.github.com>
Date:   Fri Jan 25 19:25:23 2019 +0100

    Add Tutorial Text

      Changes to the tutorial are all squashed into this commit on master, to keep the log free of clutter that distracts from the tutorial

      See the tutorial_wip branch for the actual commit history
```
