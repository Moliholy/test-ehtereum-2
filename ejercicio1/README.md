# Ejercicio 1 - ENS

> Ejercicio 1 - ENS (1 punto)
>
> Adquiera un dominio bajo el TLD ‘.test’ en la testnet Rinkeby.
>
> Describa el procedimiento seguido paso a paso.
> Demuestre que es usted poseedor del dominio adquirido y obtenga la dirección del
> Resolver utilizado. (Adjunte un pantallazo con todas las instrucciones utilizadas y sus
> outputs).
>
> *Tenga en cuenta que la duración de la propiedad de los dominios en testnet es de 28
> días.

Para la realización de este ejercicio primeramente se ha sincronizado un nodo de la red Rinkeby, y además se le ha
transferido ether de prueba a la primera dirección.

NOTA: para la realización del ejercicio se ha seguido el ejemplo de este
[tutorial](https://michalzalecki.com/register-test-domain-with-ens/).

En primer lugar comprobamos que efectivamente la sincronización ha acabado:

```
> eth.syncing
false
```

Seguidamente vamos a cargar un [script](./ensutils-rinkeby.js) para interactuar con el ENS de Rinkeby:

```
> loadScript("./ensutils-rinkeby.js")
true
```

Antes de reservar el domino es necesario desbloquear la dirección para así poder realizar transacciones:

```
> personal.unlockAccount(eth.coinbase)
Unlock account 0x5531d7617f2ea28be9de4e8fb74702b8a8259f22
Passphrase:
true
```

Una vez desbloqueada ya modemos proceder a reservar el domino ``josemolina``. Primero vamos a comprobar que está disponible:

```
> testRegistrar.expiryTimes(web3.sha3("josemolina"))
0
```

Esto nos indica que el dominio se encuentra disponible, dado que no tiene tiempo de expiración. Procedemos pues
a registrar el dominio:

```
> testRegistrar.register(web3.sha3("josemolina"), eth.coinbase, {from: eth.coinbase})
"0x6b39cedb473ac856ebdfb0c1b35486a44fbde90c5c99123b48ca0d8bbd349cfd"
```

El resultado obtenido nos indica el hash de la transacción, la cual podemos visualizar en el
[explorador](https://rinkeby.etherscan.io/tx/0x6b39cedb473ac856ebdfb0c1b35486a44fbde90c5c99123b48ca0d8bbd349cfd).
Vamos a comprobar de nuevo el tiempo de expiración:

```
> testRegistrar.expiryTimes(web3.sha3("josemolina"))
1564410652
```

Como se puede apreciar ahora el dominio tiene un tiempo de expiración asociado, lo cual indica que está reservado.
Veamos que el dueño es efectivamente la dirección con la que hemos operado:

```
> ens.owner(namehash("josemolina.test"))
"0x5531d7617f2ea28be9de4e8fb74702b8a8259f22"
> eth.coinbase
"0x5531d7617f2ea28be9de4e8fb74702b8a8259f22"
```

Por último, vamos a obtener el resolver:

```
> ens.resolver(namehash("josemolina.test"))
"0x0000000000000000000000000000000000000000"
```