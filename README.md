# bigNaturals

L'objectiu d'aquesta pràctica és definir un conjunt de funcions que permeten treballar amb
nombres naturals de “mida il·limitat”

## Les funcions zero i one

Es crea un matriu que conté el número 0 i un altre per a l'1.

```java
public static int[] zero() {
  int[] array_zero = {0};
      return array_zero;
}

public static int[] one() {
  int[] array_one = {1};
      return array_one;
}
```
## La funció de comparació equals

Es compara primer la longitud per descartar directament si no es compleix la igualtat. Si es compleix es compara nombre a nombre fins a arribar al límit de llargada del nombre.

```java
public static boolean equals (int[] nat1, int[] nat2) {
  if (nat1.length != nat2.length) {
    return false;
  }
  for (int i=0; i < nat1.length; i++) {
    if (nat1[i] != nat2[i])
      return false;
  }
  return true;
}
```
## La suma de dos números 

S'agafa el valor de llargada de nombre més gran i es declara el carry a 0.

Després es realitza un bucle on s'efectua la suma normal tenint en compte de si tenim carry d'1 guardant aquest a la variable sum per més tard sumar-ho amb la suma post carry. Aplicant la condició que el nombre no sigui major a la llargada dels dos nombres naturals se li suma els dos naturals a la variable que els guarda que pot contenir el carry.

Per determinar si hem de fer carry mirem que la suma no doni més de 9 en aquest cas se li suma 1 a carry i se li resta 10 per a obtenir el valor resultant aplicant el carry. En cas contrari carry es posa a 0 per no fer carry de l'operació anterior (si és que sa fet). Un cop acabada l'operació s'apunta el valor de la suma en la nova array `result[i]`.

Més endavant s'aplica una condició per saber si fer més gran l'array en cas que necessitem una casella més si hem fet carry en l'última operació. Si és així es fa una nova matriu amb la llargada + 1 del `result`, es copia el contingut del `result` en la nova matriu i se li afegeix el carry que sempre és 1.

Finalment, la variable result hereta el contingut nou i la retorna com a resposta.

```java
public static int[] add(int[] nat1, int[] nat2) {
  int[] result = new int[Math.max(nat1.length, nat2.length)];
  int carry = 0;
  int n1Length = nat1.length;
  int n2Length = nat2.length;
  for (int i = 0; i < result.length; i++) {
    int sum = carry;
    if (i < n1Length) sum += nat1[i];
    if (i < n2Length) sum += nat2[i];
    if (sum > 9) {
      carry = 1;
      sum -= 10;
    } else {
      carry = 0;
    }
    result[i] = sum;
  }
  if (carry > 0) {
    int[] newResult = new int[result.length + 1];
    System.arraycopy(result, 0, newResult, 0, result.length);
    newResult[result.length] = carry;
    result = newResult;
  }
  return result;
}
```
### El desplaçament cap a l'esquerra

El primer `if` verifica si l'array nat és igual a zero. En cas afirmatiu, el mètode torna el mateix array nat sense fer canvis.

Es crea una array nova amb la llargada del nombre natural més les posicions demanades a desplaçar.

Es fa un bucle `for` que copia els elements de l'array nat al nou `arrayShiftleft`, desplaçant-los a l'esquerra a la quantitat de posicions especificades per positions. El cicle comença a la posició positions de l'array `arrayShiftleft` i acaba a la seva última posició. A cada iteració, es copia l'element de nat que es troba a la posició i - positions de l'array nat (és a dir, l'element que està desplaçat positions posicions a la dreta de l'element actual al nou array) a la posició i de l'array  `arrayShiftleft`.

```java
   public static int[] shiftLeft(int[] nat, int positions) {
      if (equals(nat, zero())) {
          return nat;
      }
      int[] arrayShiftleft = new int[positions + nat.length];
      for (int i = positions; i < arrayShiftleft.length; i++) {
          arrayShiftleft[i] = nat[i - positions];
      }
      return arrayShiftleft;
  }
```

## Multiplicació d'un número per un dígit

Primer es descarta fer la multiplicació per a 0.

Es crea un array nomenat resultat que conté la llargada del nombre natural.

Es posa el carry a 0.

Es fa un bucle `for` que iterarà fins a arribar a la llargada del nombre, seguit es multiplica segons itera el bucle el nombre del natural per al dígit i se li suma si té, el carry.

Aleshores dividim entre 10 per obtenir el carry mentre obtenim el resultat de la posició del array a partir de l'obtenció del residu del producte entre 10.

Després s'aplica el carry si n'hi ha de la mateixa manera que hem fet amb la suma.

```java
public static int[] multByDigit(int[] nat, int digit) {
  if (equals(nat, zero()) || digit == 0) {
    return zero();
  }
  int[] result = new int[nat.length];
  int carry = 0;
  for (int i = 0; i < nat.length; i++) {
    int prod = nat[i] * digit + carry;
    carry = prod / 10;
    result[i] = prod % 10;
  }
  if (carry > 0) {
    int[] newResult = new int[result.length + 1];
    System.arraycopy(result, 0, newResult, 0, result.length);
    newResult[result.length] = carry;
    result = newResult;
  }
  return result;
}
```
## Multiplicació de dos números

Aquesta funció hereta gran part de la multiplicació per un dígit.

Inicialitzem el residu o acumulador de sumes parcials

Després li restem a `nat2` una posició de la seva llargada per saber quina estem mulitplicant.

Més endavant utilitzem la funció `multByDigit` per a multiplicar el `nat1` pel `nat2`.

Es desplaça el resultat a l'esquerra segons la posició del dígit amb `shiftLeft`.

Se suma el resultat parcial a l'acumulador de sumes parcials.

```java
public static int[] mult (int[] nat1, int[] nat2) {
  int[] res = {0}; 
  int pos = nat2.length - 1; 
  while (pos >= 0) {
    int[] prod = multByDigit(nat1, nat2[pos]);
    prod = shiftLeft(prod, nat2.length - pos - 1); 
    res = add(res, prod); 
    pos--;
  }
  return res;
}
```
