# 📖 Simpleswap Smart Contract

Smart contract en Solidity que permite realizar intercambios (swaps) de tokens ERC20 y gestión de liquidez entre dos tokens específicos.

## 📜 Descripción

El contrato **Simpleswap** permite:
- Agregar liquidez a un pool entre dos tokens.
- Retirar liquidez.
- Realizar intercambios de tokens a precio de pool.
- Consultar precios y estimar resultados de swaps.

Además, hereda de **ERC20** para representar los tokens de liquidez (LiquidityToken - `LQT`) que recibe el usuario al aportar fondos.

---

## 📦 Variables Públicas

- **tokenA**: Dirección del primer token del pool.
- **tokenB**: Dirección del segundo token del pool.

---

## 📚 Funciones

### 🔹 constructor(address _tokenA, address _tokenB)

Inicializa el contrato definiendo las direcciones de los dos tokens con los que funcionará el pool y crea el token de liquidez `LiquidityToken` (`LQT`).

**Parámetros:**
- `_tokenA`: dirección del token A.
- `_tokenB`: dirección del token B.

---

### 🔹 addLiquidity

Permite a un usuario aportar liquidez al pool depositando ambos tokens a cambio de tokens de liquidez.

**Parámetros:**
- `_tokenA`, `_tokenB`: direcciones de los tokens a aportar.
- `amountADesired`, `amountBDesired`: cantidades que el usuario desea aportar.
- `amountAMin`, `amountBMin`: mínimos aceptables de cada token para evitar slippage.
- `to`: dirección que recibirá los tokens de liquidez.
- `deadline`: tiempo límite para la operación.

**Devuelve:**
- `amountA`: cantidad real de token A aportada.
- `amountB`: cantidad real de token B aportada.
- `liquidity`: cantidad de tokens de liquidez acuñados.

**Lógica:**
- Verifica deadline y par de tokens.
- Obtiene balances actuales.
- Transfiere los tokens del usuario al contrato.
- Calcula cuánto se recibió efectivamente.
- Verifica mínimos aceptables.
- Minta tokens de liquidez equivalentes a la suma de ambos aportes.

---

### 🔹 removeLiquidity

Permite a un usuario retirar liquidez devolviendo los tokens de liquidez y recibiendo proporcionalmente los tokens del pool.

**Parámetros:**
- `_tokenA`, `_tokenB`: direcciones de los tokens.
- `liquidity`: cantidad de tokens de liquidez a quemar.
- `amountAMin`, `amountBMin`: mínimos aceptables para evitar slippage.
- `to`: dirección que recibirá los tokens retirados.
- `deadline`: tiempo límite para la operación.

**Devuelve:**
- `amountA`: cantidad retirada de token A.
- `amountB`: cantidad retirada de token B.

**Lógica:**
- Verifica deadline, par de tokens y que haya liquidez.
- Calcula reservas actuales.
- Determina cantidades proporcionales según el total de tokens de liquidez en circulación.
- Verifica mínimos aceptables.
- Quita los tokens de liquidez al usuario.
- Transfiere los tokens A y B al usuario.

---

### 🔹 swapExactTokensForTokens

Realiza un swap de una cantidad fija de un token por su equivalente en otro token según las reservas del pool.

**Parámetros:**
- `amountIn`: cantidad de token de entrada.
- `amountOutMin`: cantidad mínima aceptable de token de salida.
- `path`: array de direcciones de tokens [tokenIn, tokenOut].
- `to`: dirección que recibirá el token de salida.
- `deadline`: tiempo límite para la operación.

**Devuelve:**
- `amounts`: array con cantidad enviada y cantidad recibida.

**Lógica:**
- Verifica deadline y que el path tenga longitud 2.
- Valida que el par de tokens coincida con los configurados en el contrato.
- - Obtiene reservas.
- Transfiere el token de entrada desde el usuario al contrato.
- Verifica que cumpla con el mínimo aceptable.
- Transfiere el token de salida al usuario.

---

### 🔹 getPrice

Calcula el precio de un token en función del otro, usando las reservas actuales del contrato.

**Parámetros:**
- `tokenA`, `tokenB`: direcciones de los tokens a consultar.

**Devuelve:**
- `price`: precio actual de `tokenB` en unidades de `tokenA` (ajustado a 18 decimales).

**Lógica:**
- Obtiene reservas actuales.

---

### 🔹 getAmountOut

Calcula de forma pura (sin acceder a blockchain) cuánto token de salida recibirías por una cantidad de token de entrada

**Parámetros:**
- `amountIn`: cantidad de token de entrada.
- `reserveIn`: reserva actual de token de entrada.
- `reserveOut`: reserva actual de token de salida.

**Devuelve:**
- `amountOut`: cantidad estimada de token de salida.


---

## 📖 Licencia

Este proyecto está bajo la licencia **GPL-3.0**.

---

## 🚀 Despliegue

El contrato puede desplegarse en Remix, Hardhat o Truffle con Solidity >=0.8.0.

---

## 📌 Requisitos

- Solidity 0.8.x
- OpenZeppelin Contracts (para token ERC20, si se usa versión modular)
- Conexión a red Ethereum-compatible (Sepolia, Goerli, etc)
