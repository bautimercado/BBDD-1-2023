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

# 5.- Choferes (6)

- DUEÑO(<u>id_dueño</u>, nombre, teléfono, dirección, dni)
- CHOFER(<u>id_chofer</u>, nombre, teléfono, dirección,fecha_licencia_desde, fecha_licencia_hasta, dni)
- AUTO(<u>patente</u>, id_dueño, id_chofer, marca, modelo, año)
- VIAJE(<u>patente</u>, hora_desde, hora_hasta, origen, destino, tarifa, metraje)

## a) Listar el dni, nombre y teléfono de todos los dueños que NO son choferes

```cpp
dueños ← π dni (DUEÑO)
choferes ← π dni (CHOFER)
solo_dueños ← dueños - choferes

π nombre, teléfono, dni (DUEÑO ⨝ solo_dueños)
```

## b) Listar la patente y el id_chofer de todos los autos a cuyos choferes les caduca la licencia el 01/01/2024

```cpp
choferes_con_licencia_vencida_en_2024 ← π id_chofer (σ fecha_licencia_hasta like '01012024' (CHOFER))

π patente, id_chofer (AUTO ⨝ choferes_con_licencia_vencida_en_2024)
```

# 6.- Estudiantes y carreras (7)

- ESTUDIANTE(<u>#legajo</u>, nombreCompleto, nacionalidad, añoDeIngreso, códigoDeCarrera)
- CARRERA(<u>códigoDeCarrera</u>, nombre)
- INSCRIPCIONAMATERIA(<u>#legajo, códigoDeMateria</u>)
- MATERIA(<u>códigoDeMateria</u>, nombre)

## a) Obtener el nombre de los estudiantes que ingresaron en 2019.

```cpp
π nombreCompleto (σ añoDeIngreso == '2019' (ESTUDIANTE))
```

## b) Obtener el nombre de los estudiantes con nacionalidad “Argentina” que NO estén en la carrera con código “LI07”

```cpp
ESTUDIANTES_ARGENTINOS ← π #legajo (σ nacionalidad="Argentina" (ESTUDIANTE))

ESTUDIANTES_ARGENTINOS_LI07 ← π #legajo (σ códigoDeCarrera="LI07" (ESTUDIANTES_ARGENTINOS))

ESTUDIANTES_ARGENTINOS_NO_LI07 ← ESTUDIANTES_ARGENTINOS - ESTUDIANTES_ARGENTINOS_LI07

π nombreCompleto (ESTUDIANTE ⨝ ESTUDIANTES_ARGENTINOS_NO_LI07)
```

## c) Obtener el legajo de los estudiantes que se hayan anotado en TODAS las materias.

```cpp
TODAS_LAS_MATERIAS ← códigoDeMateria (MATERIA)
INSCRIPCIONAMATERIA % TODAS_LAS_MATERIAS
```

# 7.- Cursos (8)

- LUGAR_TRABAJO(<u>#empleado, #departamento</u>)
- CURSO_EXIGIDO(<u>#departamento, #curso</u>)
- CURSO_REALIZADO(<u>#empleado, #curso</u>)

## a) ¿Quiénes son los empleados que han hecho todos los cursos, independientemente de qué departamento los exija?

```cpp
CURSOS ← π #curso (CURSO_EXIGIDO ⨯ CURSO_REALIZADO)
CURSO_REALIZADO % CURSOS
```

## b) ¿Quiénes son los empleados que ya han realizado todos los cursos exigidos por sus departamentos?

```cpp
CursosARealizar ← π	#empleado, #curso (LUGAR_TRABAJO ⨝ CURSO_EXIGIDO)
EmpleadosConCursosNoHechos ← CursosARealizar - CURSO_REALIZADO
π #empleado (LUGAR_TRABAJO) - π #empleado (EmpleadosConCursosNoHechos)
```