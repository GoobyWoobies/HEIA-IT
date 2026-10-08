---
description: Représenter les nombres négatifs en complément à 2 — pourquoi cette méthode, comment coder et décoder, les plages, la soustraction par addition et le dépassement signé.
icon: plus-minus
cover: https://placehold.co/1600x500/0f172a/ec4899?text=Compl%C3%A9ment+%C3%A0+2
coverY: 0
---

# 6. Nombres signés : complément à 2

<mark style="color:blue;">**Un registre ne contient pas de signe « moins » : on code les négatifs en utilisant le cercle des nombres.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

En **complément à 2** sur n bits :

* le bit de gauche (**msb**) indique le signe : **0 = positif**, **1 = négatif** ;
* on code −x en **inversant tous les bits de x puis en ajoutant 1** ;
* la plage est de $$-2^{n-1}$$ à $$2^{n-1} - 1$$ (sur 8 bits : **−128 à 127**) ;
* l'avantage énorme : **soustraire = additionner l'opposé**, avec le même circuit.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · L'idée : reculer sur le cercle

&#x20;

On a vu que sur n bits, les valeurs forment un **cercle**. Sur 8 bits, si on part de 0 et qu'on **recule** d'un cran, on tombe sur `1111'1111` (255). Si on recule de 2 crans, `1111'1110` (254), etc.

&#x20;

L'idée du complément à 2 : puisque `1111'1111` est **un cran avant 0**, on décide qu'il représente **−1**. De même, `1111'1110` représente −2, et ainsi de suite.

&#x20;

$$\Large \text{code de } (-x) = 2^n - x$$

&#x20;

| Valeur | Calcul sur 8 bits | Code      |
| ------ | ----------------- | --------- |
| −1     | 256 − 1 = 255     | 1111'1111 |
| −2     | 256 − 2 = 254     | 1111'1110 |
| −5     | 256 − 5 = 251     | 1111'1011 |
| −128   | 256 − 128 = 128   | 1000'0000 |

&#x20;

On coupe le cercle **en deux moitiés** : la moitié où le msb vaut 0 garde les **positifs** (0 à 127), la moitié où le msb vaut 1 devient les **négatifs** (−128 à −1).

&#x20;

<figure><img src="../../.gitbook/assets/tn-cercle-8bits-b3.png" alt="Cercle des nombres sur 8 bits en complément à 2" width="520"><figcaption><p>À droite (vert) les positifs, à gauche (rouge) les négatifs. Chaque valeur est placée sur son code non signé (entre parenthèses).</p></figcaption></figure>

&#x20;

<details>

<summary>Pourquoi pas simplement un bit de signe devant le nombre ?</summary>

&#x20;

On pourrait coder −5 comme `1000'0101` (bit de signe 1, puis 5). Mais cette méthode a deux gros défauts :

* il y a **deux zéros** : `0000'0000` (+0) et `1000'0000` (−0) ;
* l'addition **ne marche plus** : `0000'0101` (5) + `1000'0101` (−5) = `1000'1010` = −10 au lieu de 0. Il faudrait un circuit spécial pour les négatifs.

En complément à 2, **un seul zéro**, et l'addition normale donne directement le bon résultat : `0000'0101` + `1111'1011` = `1'0000'0000` → on garde 8 bits → `0000'0000` = 0. C'est pour ça que **tous** les processeurs utilisent le complément à 2.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Coder un nombre négatif

&#x20;

**Exemple :** coder −45 sur 8 bits.

&#x20;

{% tabs %}
{% tab title="Méthode 1 : inverser + 1" %}
<mark style="color:orange;">1. Coder la valeur positive :</mark> 45 = `0010'1101`

<mark style="color:orange;">2. Inverser tous les bits</mark> (on appelle ça le complément à 1) : `1101'0010`

<mark style="color:orange;">3. Ajouter 1 :</mark> `1101'0010` + 1 = **`1101'0011`**

&#x20;

**Pourquoi ça marche ?** Inverser tous les bits de x revient à calculer $$(2^n - 1) - x$$ (car `1111'1111` − x inverse chaque bit sans aucun emprunt). En ajoutant 1, on obtient $$2^n - x$$ : exactement la définition.
{% endtab %}

{% tab title="Méthode 2 : 2ⁿ − x" %}
On calcule $$256 - 45 = 211$$, puis on convertit 211 en binaire :

$$211 = 128 + 64 + 16 + 2 + 1 = 1101'0011_{(2)}$$

&#x20;

Pratique quand on est à l'aise avec les conversions décimales.
{% endtab %}

{% tab title="Méthode 3 : raccourci" %}
On part de x = `0010'1101` et on lit **de droite à gauche** :

* on **recopie** les bits jusqu'au **premier 1 inclus** : ici, le premier bit est déjà un 1 → on recopie `1` ;
* on **inverse** tous les bits suivants : `0010'110` → `1101'001`.

Résultat : `1101'0011`. Même résultat, sans addition.
{% endtab %}
{% endtabs %}

&#x20;

{% hint style="success" %}
**Vérification** — un nombre plus son opposé doit donner 0 sur 8 bits : `0010'1101` + `1101'0011` = `1'0000'0000` → les 8 bits valent 0. Le 1 qui déborde est perdu, et c'est voulu.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Lire un nombre en complément à 2

&#x20;

{% stepper %}
{% step %}
### Regarder le msb

**0** → nombre **positif** : on le lit normalement. **1** → nombre **négatif**.
{% endstep %}

{% step %}
### Si négatif : deux façons de le lire

* **Poids négatif** : le msb vaut $$-2^{n-1}$$ (−128 sur 8 bits), les autres bits gardent leur poids positif.
* **Inverser + 1** : on applique la même opération qu'au codage, on lit la valeur, on met un signe moins.
{% endstep %}
{% endstepper %}

&#x20;

**Exemple :** `1001'1101`

* Poids négatif : $$-128 + 16 + 8 + 4 + 1 = -99$$.
* Inverser + 1 : `0110'0010` + 1 = `0110'0011` = 99 → donc **−99**.
* Ou encore : en non signé c'est 157, et $$157 - 256 = -99$$.

&#x20;

| Poids du bit (8 bits, signé) | bit 7    | bit 6 | bit 5 | bit 4 | bit 3 | bit 2 | bit 1 | bit 0 |
| ---------------------------- | -------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Poids                        | **−128** | 64    | 32    | 16    | 8     | 4     | 2     | 1     |

&#x20;

{% hint style="info" %}
**Les mêmes bits, deux interprétations** — `1111'0010` vaut **242** si on le lit en **non signé** et **−14** en **signé** (242 − 256). Le registre ne « sait » pas ce qu'il contient : c'est **le programmeur** qui décide comment interpréter les bits. C'est pour ça que les exercices demandent les deux colonnes « décimal non signé » et « décimal signé ».
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Les plages de valeurs

&#x20;

Sur **n bits** en complément à 2 :

$$\Large -2^{n-1} \;\leq\; x \;\leq\; 2^{n-1} - 1$$

&#x20;

| Bits | Non signé       | Signé (complément à 2) | Plus petit (binaire) | Plus grand (binaire) |
| ---- | --------------- | ---------------------- | -------------------- | -------------------- |
| 4    | 0 … 15          | −8 … 7                 | 1000                 | 0111                 |
| 6    | 0 … 63          | −32 … 31               | 10'0000              | 01'1111              |
| 8    | 0 … 255         | −128 … 127             | 1000'0000            | 0111'1111            |
| 10   | 0 … 1023        | −512 … 511             | 10'0000'0000         | 01'1111'1111         |
| 16   | 0 … 65 535      | −32 768 … 32 767       | 1000…0000            | 0111…1111            |

&#x20;

**Pourquoi un négatif de plus que de positifs ?** Les $$2^n$$ codes sont partagés en deux moitiés égales ($$2^{n-1}$$ chacune). Mais **0** est dans la moitié « msb = 0 », avec les positifs. Il reste donc $$2^{n-1} - 1$$ positifs non nuls, contre $$2^{n-1}$$ négatifs.

&#x20;

{% hint style="warning" %}
**Le cas de −128 (sur 8 bits)** — son code est `1000'0000`. Si on lui applique « inverser + 1 » : `0111'1111` + 1 = `1000'0000`, on retombe sur lui-même ! C'est normal : +128 **n'existe pas** sur 8 bits signés. −128 est le seul nombre sans opposé.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Soustraire = additionner l'opposé

&#x20;

$$a - b = a + (-b)$$ : on code −b en complément à 2 et on **additionne**. Le circuit n'a besoin que d'un additionneur.

&#x20;

**Exemple :** 28 − 15 sur 8 bits.

&#x20;

<mark style="color:orange;">1.</mark> 15 = `0000'1111` → −15 = `1111'0000` + 1 = `1111'0001`.

<mark style="color:orange;">2.</mark> On additionne :

```
retenues     1110'0000
               0001'1100     (28)
             + 1111'0001     (-15)
             -----------
           1 | 0000'1101     (13)
```

<mark style="color:orange;">3.</mark> On **ignore** la retenue sortante : il reste `0000'1101` = **13**. Juste.

&#x20;

**Pourquoi peut-on ignorer cette retenue ?** −15 a été codé comme $$256 - 15$$. L'addition a donc calculé $$28 + 256 - 15 = 256 + 13$$. Le « 256 » est exactement la retenue qui sort du 8e bit : en la jetant, on garde 13. Sur le cercle : avancer de 241 crans revient à reculer de 15.

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · Le dépassement en signé

&#x20;

En signé, le dépassement se produit quand on traverse l'étoile **entre 127 et −128** (sur 8 bits), ou **entre 31 et −32** sur 6 bits (voir le [cercle 6 bits](05-arithmetique-et-depassement.md)) : **30 + 2 = −32** au lieu de 32.

&#x20;

{% hint style="danger" %}
**Règle du dépassement signé**

* **positif + positif** donne un résultat **négatif** (msb = 1) → dépassement ;
* **négatif + négatif** donne un résultat **positif** (msb = 0) → dépassement ;
* **positif + négatif** → **jamais** de dépassement (le résultat est forcément entre les deux).

La **retenue sortante ne compte pas** en signé : elle apparaît normalement dans 28 − 15, sans aucune erreur.
{% endhint %}

&#x20;

### Le même calcul, deux verdicts

&#x20;

**Exemple (exercice B.7) :** `0101'0110` + `1100'0010` sur 8 bits.

```
retenues     1000'1100
               0101'0110
             + 1100'0010
             -----------
           1 | 0001'1000
```

&#x20;

| Interprétation | Calcul         | Résultat lu  | Cohérent ?                                           |
| -------------- | -------------- | ------------ | ---------------------------------------------------- |
| non signé      | 86 + 194 = 280 | 24           | <mark style="color:red;">non</mark> : retenue sortante, 280 > 255 |
| signé          | 86 + (−62) = 24 | 24          | <mark style="color:green;">oui</mark> : positif + négatif, jamais de dépassement |

&#x20;

C'est le **même** calcul binaire : seule l'interprétation change.

&#x20;

***

&#x20;

## <mark style="color:purple;">07</mark> · Changer de taille : l'extension de signe

&#x20;

Pour passer un nombre signé de 8 à 10 bits (ou plus), on **recopie le msb** dans les nouveaux bits de gauche :

&#x20;

| Valeur | Sur 8 bits  | Sur 10 bits      |
| ------ | ----------- | ---------------- |
| 73     | `0100'1001` | `00'0100'1001`   |
| −73    | `1011'0111` | `11'1011'0111`   |

&#x20;

**Pourquoi des 1 pour un négatif ?** Sur 10 bits, −73 se code $$1024 - 73 = 951$$ = `11'1011'0111`. Ajouter des 1 à gauche, c'est ajouter $$512 + 256 = 768$$ au code 8 bits (183) : $$183 + 768 = 951$$. Les 1 « prolongent » le cercle sans changer la valeur.

&#x20;

### Lire un registre de 10 bits en hexadécimal

&#x20;

Sur 10 bits, le msb est le bit 9 (poids 512 = `0x200`). Donc :

* un code **< 0x200** est **positif** : `0x1FC` = 508 ;
* un code **≥ 0x200** est **négatif** : `0x2BD` = 701 en non signé → $$701 - 1024 = -323$$.

&#x20;

***

&#x20;

## <mark style="color:purple;">08</mark> · Exercices

&#x20;

<details>

<summary>1. Code −10 et −89 sur 8 bits</summary>

&#x20;

* 10 = `0000'1010` → inverser `1111'0101` → +1 → **`1111'0110`**.
* 89 = `0101'1001` → inverser `1010'0110` → +1 → **`1010'0111`**.

&#x20;

</details>

<details>

<summary>2. Que vaut 1100'1100 en signé (8 bits) ?</summary>

&#x20;

$$-128 + 64 + 8 + 4 = -52$$ (ou 204 − 256).

&#x20;

</details>

<details>

<summary>3. Sur 8 bits signés, calcule 100 + 50. Cohérent ?</summary>

&#x20;

`0110'0100` + `0011'0010` = `1001'0110` : positif + positif donne un msb à 1 → **dépassement**. On lit −106 au lieu de 150 (150 > 127).

&#x20;

</details>

<details>

<summary>4. Plus petite et plus grande valeur signée sur 12 bits ?</summary>

&#x20;

$$-2^{11} = -2048$$ et $$2^{11} - 1 = 2047$$.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">09</mark> · À retenir

&#x20;

* Code de −x = $$2^n - x$$ = **inverser les bits de x, puis +1**.
* msb = 0 → positif, msb = 1 → négatif ; le msb pèse $$-2^{n-1}$$.
* Plage : $$-2^{n-1}$$ à $$2^{n-1} - 1$$ ; sur 8 bits **−128 à 127**.
* Soustraction = addition de l'opposé ; la retenue sortante est **ignorée**.
* Dépassement signé : deux nombres de **même signe** donnent un résultat de **signe opposé**.
* Extension de signe : **recopier le msb** à gauche.

&#x20;

{% hint style="info" %}
**Page suivante** → [7. Nombres réels : virgule fixe](07-virgule-fixe.md)
{% endhint %}
