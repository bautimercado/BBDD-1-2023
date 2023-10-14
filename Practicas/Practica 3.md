# 1.- Indique las opciones correctas

## A) Indique cuáles de las siguientes operaciones son válidas:

- `A(a, b, c) ∪ B(a, b, d)`
  - ❌ Inválida, la union debe ser compatible
- `(A(a, b, c) ⨝ B(a, b)) - C(a, b, c)`
  - ✅ Válida
- `(A(a, b, c) ⨝ B(a, d, e)) ∩ D(a, b, c, d, e)`
  - ✅ Válida
- `(A(a, b, c) ⨝ B(a, b, d)) ∩ D(a, b, c, d)`
  - ❌ Inválida, recordar que el producto no elimina elementos duplicados.

## B) Para la operación de resta es necesario que los esquemas involucrados sean compatibles, es decir, deben cumplir las siguientes condiciones:

- Deben tener la misma cantidad de columnas[✅] 
- Las columnas deben ser del mismo dominio[✅]
- El orden de los columnas debe ser el mismo[❌]
- Las columnas deben tener igual nombre[❌]

# 2.- ¿Para cuáles de las siguientes operaciones es necesario que los operandos sean union compatibles? Marque todas las opciones correctas:

- resta - [✅]
- división % [❌]
- unión ∪ [✅]
- producto cartesiano ⨯ [❌]
- producto natural ⨝ [❌]

# 3.- Dados los siguientes esquemas

- COMPRA(#compra, fecha, monto_total)
- COMPRA_PRODUCTO(#compra, cantidad, #producto)
- PRODUCTO(#producto, nombre, precio)

Indique qué formato (conjunto de atributos) tiene el resultado de aplicar la siguiente operación.

`COMPRA_PRODUCTO % (π #producto (PRODUCTO))`

- [✅] (#compra, cantidad)
- [❌] (#compra, cantidad, #producto)
- [❌] (#compra)

# 4.- Dado el siguiente esquema:

- PASAJERO (#pasajero, nombre, dni, puntaje)
- PASAJERO_RESERVA (#pasajero, #reserva)
- RESERVA (#reserva, #vuelo, fecha_reserva, monto, #asiento)
- VUELO (#vuelo, aeropuerto_salida, aeropuerto_destino, fecha_vuelo)

Indicar si las siguientes consultas obtienen el resultado correcto (sin importar la optimización).

- A) Obtener los pasajeros que tengan reservas sobre vuelos del próximo año, listando #pasajero, #vuelo y #asiento.
  ```cpp
  VUELOS_PROX_AÑO <— σ fecha_vuelo >= 1/1/2024 and fecha_vuelo <= 31/12/2024 VUELO

  RES <— π #pasajero,#vuelo,#asiento (VUELOS_PROX_AÑO ⨝ RESERVA ⨝ PASAJERO_RESERVA)
  ```
    - La consulta es correcta.

- B) Obtener el listado de montos de reservas realizadas para vuelos efectuados el pasado Agosto desde Buenos Aires a Córdoba.
    ```cpp
    VUELOS_BUE_CBA <— σ ciudad_salida=“Buenos Aires” and ciudad_destino=“Córdoba” (VUELO)

    RESERV_AGO <— (σ fecha_reserva >= 1/8/2023 and fecha_reserva <= 31/8/2023 (RESERVA)) |X| (VUELOS_BUE_CBA)

    RES <— π monto_total (RESERV_AGO)
    ```
    - La consulta no es correcta ya que no existe el atributo `monto_total`.


- C) Obtener el/los pasajeros con reservas en vuelos pendientes que tengan menor puntaje

    ```cpp
    PUN_ALTOS <— σ pun < puntaje ((PASAJERO) ⨯ (ρ (#p, nom, d, pun) (PASAJERO)))

    PUN_BAJOS <— (π puntaje (PASAJEROS)) - (π puntaje (PUN_ALTOS))

    RES <— PASAJEROS ⨝ PUN_BAJOS
    ```
    - La consulta no es correcta ya que nunca se verifica que los pasajeros tengan vuelos pendientes.

- D) Obtener el/los pasajeros que solo hayan reservado vuelos cuyo aeropuerto de salida sea el aeropuerto “Ministro Pistarini”. Listar el nombre y dni de los pasajeros.

    ```cpp
    VUELOS_PISTARINI <- π #vuelo (σ aeropuerto_salida=“Ministro Pistarini” (VUELO))
    
    RESERVA_PISTARINI <- π #pasajero (VUELOS_PISTARINI ⨝ RESERVAS)
    
    PASAJEROS_PISTARINI <- π nombre,dni (RESERVA_PISTARINI ⨝ PASAJERO)
    ```
    - La consulta no es correcta.
      - Intenta proyectar #pasajero cuando no posee ese atributo.
      - En ningún momento se queda con los pasajeros que __solo__ tengan reservas con aeropuerto origen "Ministro Pistarini". Tendría que obtener todos los pasajeros que no tengan reservas con aeropuerto de salida de "Ministro Pistarini" y restarlo con __PASAJEROS_PISTARINI__.

- E) Obtener el/los id/s de los pasajeros que hayan realizado reservas por un monto superior a $99000

    ```cpp
    RESERVAS_MAS_99000 <- π #pasajero (σ monto < 99000 (RESERVAS))
    ```
    - La consulta no es correcta.
      - El atributo #pasajero no existe en la relación RESERVAS
      - Me trae las reservas cuyo monto es menor qué 99000