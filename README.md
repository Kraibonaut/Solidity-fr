# Level 6 - Solidity

![image](https://user-images.githubusercontent.com/16539849/173656651-a46df615-8ec3-43fd-9619-98647a6d2bd2.png)

Dans ce module, vous apprendrez ce qu'est Solidity et la syntaxe de base du langage.

## What is Solidity ?

- Solidity est un langage orienté objet et de haut niveau pour la mise en œuvre de contrats intelligents. Il est conçu pour cibler l'[Ethereum Virtual Machine(EVM)](https://coinmarketcap.com/alexandria/glossary/ethereum-virtual-machine-evm)
- Il est typée statiquement, supporte l'héritage, les bibliothèques et les types complexes définis par l'utilisateur, entre autres caractéristiques.

<Quiz questionId="e7201124-8b84-4e20-99d3-d85189ee1817" />

## Building in Solidity

### Initializing smart contracts

```solidity
// Pour définir la version du compilateur que vous utiliserez
pragma solidity ^0.8.10;

// Commencez par créer un contrat nommé  HelloWorld
contract HelloWorld {

}
```

### Variables and types

Il existe 3 types de variables dans Solidity

- Local
  - Déclaré à l'intérieur d'une fonction et n'est pas stocké sur la blockchain.
- État
  - Déclaré à l'extérieur d'une fonction pour maintenir l'état du smart contract.
  - Stockés sur la blockchain
- Global
  - Fournissent des informations sur la blockchain. Elles sont injectées par la machine virtuelle Ethereum pendant l'exécution.
  - Comprend des choses comme l'expéditeur de la transaction, l'horodatage du bloc, le hachage du bloc, etc.
  - [Examples de variables global](https://docs.soliditylang.org/en/v0.8.9/units-and-global-variables.html)

La portée des variables est définie par l'endroit où elles sont déclarées, et non par leur valeur. Le fait d'attribuer la valeur d'une variable locale à une variable globale ne fait pas d'elle une variable globale, car elle n'est toujours accessible que dans sa portée.

<Quiz questionId="04b4af27-6816-4a2e-9070-16c6e4c783ce" />

```solidity
// Pour définir la version du compilateur que vous utiliserez
pragma solidity ^0.8.10;

// Commencez par créer un contrat nommé  Variables
contract Variables {
    /*
        ******** State variables **********
    */
    /*
     uint signifie "unsigned integer", c'est-à-dire des nombres entiers non négatifs.
    différentes tailles sont disponibles. Par exemple,
        - uint8 va de 0 à 2 ** 8 - 1
        - uint256 va de 0 à 2 ** 256 - 1
    `public` signifie que la variable peut être accédée par le contrat
     par le contrat et peut également être lue par le monde extérieur.
    */
    uint8 public u8 = 10;
    uint public u256 = 600;
    uint public u = 1230; // uint est un alias pour uint256

    /*
    Les nombres négatifs sont autorisés pour les types int. Par exemple,
    - int256 va de -2 ** 255 à 2 ** 255 - 1
    */
    int public i = -123; // int est identique à int256

    // address représente une adresse ethereum
    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;

    // bool signifie booléen
    bool public defaultBoo1 = false;

    // Valeurs par défaut
    // Les variables non assignées ont une valeur par défaut dans Solidity
    bool public defaultBoo2; // false
    uint public defaultUint; // 0
    int public defaultInt; // 0
    address public defaultAddr; // 0x0000000000000000000000000000000000000000

    function doSomething() public {
        /*
        ******** Local variable **********
        */
        uint ui = 456;

        /*
        ******** Global variables **********
        */

        /*
             block.timestamp nous dit quel est l'horodatage du bloc actuel
            msg.sender nous indique quelle adresse a appelé la fonction doSomething.
        */
        uint timestamp = block.timestamp; // Horodatage du bloc actuel
        address sender = msg.sender; // adresse de l'appelant
    }
}
```

<Quiz questionId="2156e5cd-26ac-402d-bbe9-ecaefdd8c87a" />
<Quiz questionId="bd68f893-b126-4f54-aa78-2906f2287629" />
<Quiz questionId="d934fc15-f88d-4cd7-b903-f268f1c16980" />
<Quiz questionId="a531c37c-96f9-4bbd-a80f-893b37a5c94e" />

### Functions, Loops and If/Else

```solidity
// Pour définir la version du compilateur que vous utiliserez
pragma solidity ^0.8.10;

// Commencez par créer un contrat nommé  Conditions
contract Conditions {
    // Variable d'état pour stocker un nombre
    uint public num;

    /*
       Le nom de la fonction est set.
        Elle prend un uint et définit la variable d'état num.
        Elle est déclarée comme une fonction publique, ce qui signifie
        qu'elle peut être appelée à l'intérieur du contrat et aussi à l'extérieur.
    */
    function set(uint _num) public {
        num = _num;
    }

    /*
        Le nom de la fonction est get.
        Elle retourne la valeur de num.
        Elle est déclarée comme une fonction de vue, ce qui signifie
        que la fonction ne change pas l'état d'une variable.
        Les fonctions de vue dans la solidité ne nécessitent pas de gaz.
    */
    function get() public view returns (uint) {
        return num;
    }

    /*
        Le nom de la fonction est foo.
        Elle prend un uint et retourne un uint.
        Elle compare la valeur de x en utilisant if/else.
    */
    function foo(uint x) public returns (uint) {
        if (x < 10) {
            return 0;
        } else if (x < 20) {
            return 1;
        } else {
            return 2;
        }
    }

    /*
        Le nom de la fonction est loop.
        Elle exécute une boucle jusqu'à 10
    */
    function loop() public {
        // for loop = pour la boucle
        for (uint i = 0; i < 10; i++) {
            if (i == 3) {
                // Passer à l'itération suivante avec continue
                continue;
            }
            if (i == 5) {
                // Fin de la boucle/loop avec break
                break;
            }
        }
    }


}

```

<Quiz questionId="b9a5620b-7c92-450b-afa8-1cda0dfba54a" />
<Quiz questionId="f14bb058-720b-42e5-ba85-4c7bfc1b93ee" />

### Arrays, Strings

Un tableau peut avoir une taille fixe au moment de la compilation ou une taille dynamique.

```solidity
pragma solidity ^0.8.10;

contract Array {

    // Déclarer une variable de type chaîne de caractères qui est publique
    string public greet = "Hello World!";
    // Plusieurs façons d'initialiser un tableau
    // Les tableaux initialisés ici sont considérés comme des variables d'état qui sont stockées sur la blockchain.
    // Il s'agit de variables de stockage
    uint[] public arr;
    uint[] public arr2 = [1, 2, 3];
    // Tableau de taille fixe, tous les éléments sont initialisés à 0.
    uint[10] public myFixedSizeArr;
    /*
        Le nom de la fonction est get
        Elle obtient la valeur de l'élément stocké dans l'index d'un tableau.
    */
    function get(uint i) public view returns (uint) {
        return arr[i];
    }

    /*
      Solidity peut retourner le tableau entier.
     Cette fonction est appelée avec et renvoie un uint[] memory.
     mémoire - la valeur est stockée uniquement en mémoire, et non sur la blockchain
              elle n'existe que pendant la durée d'exécution de la fonction.

     Les variables de mémoire et les variables de stockage peuvent être comparées à la RAM et au disque dur.
     Les variables de mémoire existent temporairement, pendant l'exécution de la fonction, tandis que les variables de stockage
     sont persistantes à travers les appels de fonction pour la durée de vie du contrat.
     Ici, le tableau n'est nécessaire que pour la durée d'exécution de la fonction et est donc déclaré comme une variable mémoire.
    */
    function getArr(uint[] memory _arr) public view returns (uint[] memory) {
        return _arr;
    }

     /*
         Cette fonction renvoie la mémoire des chaînes de caractères.
        La raison pour laquelle le mot clé memory est ajouté est que la chaîne fonctionne en interne comme un tableau.
        Ici, la chaîne n'est nécessaire que pendant l'exécution de la fonction.
    */
    function foo() public returns (string memory) {
        return "C";
    }

    function doStuff(uint i) public {
        // Ajouter au tableau
        // Cela augmentera la longueur du tableau de 1.
        arr.push(i);
        // Enlève le dernier élément du tableau
        // Cela diminuera la longueur du tableau de 1
        arr.pop();
        // obtenir la longueur du tableau
        uint length = arr.length;
        // La suppression ne modifie pas la longueur du tableau.
        // Il réinitialise la valeur à l'index à sa valeur par défaut,
        // dans ce cas, il réinitialise la valeur de l'index 1 dans arr2 à 0
        uint index = 1;
        delete arr2[index];
        // créer un tableau en mémoire, seule une taille fixe peut être créée
        uint[] memory a = new uint[](5);
        // crée une chaîne de caractères en mémoire
        string memory hi = "hi";
    }

 }
```

<Quiz questionId="0643be02-c57b-41b0-b7e2-9e855883b7c2" />
<Quiz questionId="ec82e3aa-ec12-4d8b-8f81-f21ff1fb0a54" />
<Quiz questionId="bab86524-5ace-4baa-add4-f66482b51abe" />

## References

[Solidity by Example](https://solidity-by-example.org/)

## Resources for learning extra

- [Cryptozombies](https://cryptozombies.io/)
- [Solidity by Example](https://solidity-by-example.org/)
- [Solidity docs](https://docs.soliditylang.org/en/v0.8.10/)

## DYOR Questions
<Quiz questionId="685cf7f3-aafe-4199-a40e-06256a9191f9" />
<Quiz questionId="36ea5014-f51e-4fb3-8fa2-027062b1c895" />

<SubmitQuiz />
