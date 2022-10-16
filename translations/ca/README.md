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

Ací podem vore algunes coses interessants:
* Els dos primers «commits» estan fets per mi.
* El teu commit inicial on vas afegir Bob és el _HEAD_ actual de la branca _master_ en el _Repositori Remot_.
  Tornarem a fixar-nos amb això quan parlem sobre branques i obtindre canvis remots.
* L'últim «commit» en el _Repositori Local_ és el que acabem de fer, i ara podem veure el seu «hash».

> Fixa't que el «commit hash» serà diferent en el teu cas. Si vols averiguar com git obté aquests ID,
> pots consultar [aquest article interessant](https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html).

Per comparar aquest «commit» amb algun anterior, podem fer `git diff <commit>^!` (on `^!` indica a git que
compare el «commit» amb el que s'ha realitzat just abans que aquest). En aquest cas, executariem
`git diff 87a4ad48d55e5280aa608cd79e8bce5e13f318dc^!`.

També podem fer 
`git diff 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e 87a4ad48d55e5280aa608cd79e8bce5e13f318dc`
per obtindre el mateix resultat i, en general, per comparar qualsevol parell de «commits».
El format és `git diff <des de commit> <a commit>`, així que el nou «commit» aniria en segon lloc.

El següent diagrama pots veure les diferents etapes d'un canvi i l'ordre `diff` corresponent.

![States of a change an related diff commands](../../img/diffs.png)

Ara que hem comprovat que hem fet el canvi que voliem, podem publicar els canvis `git push`.

## Branques

Una altra cosa que fa que Git siga fantàstic, és que treballar
amb branques és una part realment fàcil i integral de la manera
de treballar amb Git.

De fet, hem estat treballant en una branca des de que hem començat.

Quan hem clonat (`clone`) el _Repositori Remot_ el nostre
_Entorn de Desenvolupament_ comença automàticament a la branca principal
del repositori, és a dir, _master_.

La majoria de fluxos de treball amb git inclouen fer els canvis en un
_branca_, abans de fusionar-los de nou a a la branca _master_.

Normalment treballaràs en una _branca_ pròpia fins que
no hàges acabat i confies que els canvis realitzats, que després
es fusionaran a la branca _master_.

> Molts gestors de repositoris Git com _GitLab_ i _GitHub_ també permeten
> que les branques estiguen protegides (_protected_), cosa que significa que
> no tothom pot fer modificacions en aquestes branques.
> Normalment la branca principal _master_ està protegida de manera predeterminada.

No et preocupes, tornarem a totes aquestes coses amb
més detall quan les necessitem.

Ara mateix volem crear una branca per fer alguns canvis.
Potser només voleu provar alguna cosa pel vostre compte i
no embolicar l'estat de treball a la vostra branca _master_,
o no teniu permís per publicar (`push`) a la branca _master_.

Les branques existeixen al _Repositori Local_ i al _Repositori Remot_.
Quan creeu una nova branca, el contingut de les serà una còpia de
l’estat actual de l'últim «commit» de qualsevol branca que
esteu treballant actualment.

Fem algun canvi a Alice.txt! Què tal si posem un text a la segona línia?

Volem compartir aquest canvi, però no posar-lo la branca _master_ immediatament,
així que creem una branca amb `git branch <branch name>`.

Per crear una branca nova anomenada `change_alice`, podem executar
`git branch change_alice`.

Aquesta ordre afegeix la nova branca al _Repositori Local_.

El _Directori de Treball_ i l'_Àrea de Preparació_ no es preocupen realment
per les branques, sempre que feu un `commit` es realitzara a la branca en la qual
esteu actualment.

Podeu pensar en les branques en git com a punters,
que apunten a una sèrie de «commits». Quan feu un `commit`,
afegiu els canvis on esteu apuntant actualment.

Al afegir una branca, no et porta directament a aquesta, només crea el punter.
De fet, l'estat en el que es troba el teu _Repositori Local_ es pot veure
com un altre punter, anomenat _HEAD_, que apunta a quina branca i «commit»
et trobes actualment.

Si això pareix complicat, esperem que els diagrames següents ajuden
a aclarir un poc les coses:

![Estat despres d'afegir una branca](../../img/add_branch.png)

Per canviar a la nostra nova branca hauràs d'utilitzar
`git checkout change_alice`. El que fa és simplement moure el _HEAD_
a la branca (o «commit») que especifiqueu.

> Com que normalment voldreu canviar a una branca
> just després de crear-la, hi ha la còmoda opció `-b`
> disponible per a l'ordre `checkout`, que us permet només
> fer directament `checkout' una _nova_ branca,
> de manera que no cal crear-la abans.

> Per tant, per crear i canviar a la nostra branca `change_alice`,
> també podríem haver cridat `git checkout -b change_alice`.

![Estat després de canviar a la branca](../../img/checkout_branch.png)

Veuràs que el vostre _Directori de Treball_ no ha canviat i el canvi que hem fet a `Alice.txt`
encara no està relacionat amb la branca on estem. Ara podeu afegit `add` i cometre `commit` el canvis
a `Alice.txt` tal com vam hem fet abans a _master_,
que _prepararà_ (_stage_) (en aquest moment encara no està relacionat amb capp branca)
i finalment _cometrà_ (_commit_) el canvi a la branca `change_alice`.

Només hi ha una cosa que encara no pots fer.
Intenteu publicar els canvis `git push` al _Repositori Remot_.

Veureu l'error següent i, git que sempre està preparat per ajudar-vos,
et proposarà un suggeriment sobre com resoldre el problema:

```ShellSession
fatal: The current branch change_alice has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin change_alice
```

Però no volem fer-ho a cegues. Estem ací per entendre què està passant realment.
Aleshores, què són les _branques upstream_ i els _remots_?

Recordes quan vam _clonar_ el _Repositori Remot_ abans?
En aquell moment no només contenia aquest tutorial i `Alice.txt`,
sinó que hi havien dues branques.

La branca principal (_master_) on hem treballat i hem fet canvis, i el que vaig anomenar "tutorial_wip"
en el qual estan tots els canvis que s'han fet en aquest tutorial.

Quan es van copiar les coses del _Repositori Remot_ al vostre _Entorn de Desenvolupament_,
uns quants passos adicionals van passar desapercebuts.

Git va configurar el _remot_ (_remote_) del vostre _Repositori Local_
perquè siga el _Repositori Remot_ que vam clonar
i el va anomenar pel nom per defecte `origin`.

> El vostre _Repositori Local_ pot fer un seguiment de diversos _remots_ i poden tindre noms diferents,
> però en aquest tutorial ens quedarem amb `origin`.

Després, es van copiar les dues branques remotes al vostre _Repositori Local_ i,
finalment, es va fer `checkout` a la branca _master_.

En fer-ho, passa un altre pas implícit. Quan feu `checkout` d'un nom de branca que tingui una coincidència exacta a les branques remotes,
obtindreu una nova branca _local_ que està enllaçada a la branca _remota_. La branca _remota_ és la _upstream_ de la vostra _local_.

Als diagrames anteriors podeu veure només les branques locals que teniu.
Podeu veure aquesta llista de branques locals executant `git branch`.

Si també voleu veure les branques _remotes_ que coneix el vostre _Repositori Local_,
podeu utilitzar `git branch -a` per llistar-les.

![Branques locals i remotes](../../img/branches.png)

Ara podem cridat al suggerit `git push --set-upstream origin change_alice`, i publicar (`push`)
els canvis a la nostra branca a un nou _remot_ (_remote_). Això crearà una branca `change_alice`
al _Repositori Remot_ i establirà la nostra branca _local_ `change_alice` per fer un seguiment
d'aquesta nova branca.

> Hi ha una altra opció si realment volem que la nostra branca local
> faça un seguiment d'alguna branca que ja existeix al _Repositori Remot_.
> Potser un company ja ha publicat alguns canvis, mentre estàvem treballant
> en algun tema relacionat a la nostra branca local, i ens agradaria integrar els dos.
> Aleshores, només podríem configurar el _upstream_ per a la nostra branca `change_alice`
> a un nou _remote_ utilitzant `git branch --set-upstream-to=origin/change_alice`
> i des d'allà per fer un seguiment de la branca _remote_.

Després de fer-ho, mireu el vostre _Repositori Remot_ a GitHub. La vostra branca estarà allà,
preparada perquè altres persones la vegen i treballen.

Prompte veurem com podeu incorporar els canvis d'altres persones al vostre _Entorn de Desenvolupament_,
però primer treballarem un poc més amb les branques, per introduir tots els conceptes que també entren
en joc quan obtenim coses noves del _Repositori Remot_.
