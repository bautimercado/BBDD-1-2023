# Practica 2 - Normalización.

# Parte 1

## 1. Indicar la opción correcta

Dado el siguiente esquema:

__MapasPublicados__(idMapa, proyección, escalaMapa, idSitioWeb, dominioSitioWeb, especialidadSitioWeb, dueñosSitioWeb, fechaPublicaciónMapa, valorPublicación)

Donde:
- A un sitio web se le cobra un valor (“valorPublicación”) por cada fecha (“fechaPublicaciónMapa”) en la cual publique un mapa.
- Un sitio web puede tener varios dueños (“dueñosSitioWeb”).
- Un sitio web posee un único dominio (“dominioSitioWeb”).
- El identificador de un mapa (“idMapa”) es único.
- El identificador de un sitio web (“idSitioWeb”) es único.
- Un mapa se genera con una proyección y a una escala.
- “especialidadSitioWeb” es la especialidad de un sitio.

Seleccione la frase que considera verdadera
- El esquema tiene una clave candidata -> (V)
    - cc: {idSitioWeb, idMapa, dueñosSitioWeb, especialidadSitioWeb}
- El esquema tiene más de una clave candidata

## 2. Clave candidata

Dado el siguiente esquema donde se cumplen las siguientes dependencias funcionales df1 y df2:

- E(a, b, c, d, e, f)
- df1) a-> b, c
- df2) c-> d, e

¿Cuál de las siguientes CC es la correcta?

1. CC(a,c)
2. CC(a)
3. CC(a,f) -> (V)
4. CC(a,c,f)
5. CC(f)

## 3. Indicar la opción correcta

Dada la relación

ALUMNO (DNI, nyAp, nroLegajo, promedio, #libroUsadoEnCarrera)

En la que se cumple las siguientes dependencias funcionales:

- DF1) DNI → nyAp, nroLegajo, promedio
- DF2) nroLegajo → nyAp, DNI,promedio

¿Cuál de las siguientes afirmaciones es correcta?

- a) La relación ALUMNO tiene dos claves candidatas y tendrá dos claves primarias.
- b) La relación ALUMNO tiene dos claves candidatas y tendrá una clave primaria. -> (V)
- c) No puedo identificar una clave.
- d) Ninguna de las anteriores.

## 4. Dependencias funcionales

Dado el siguiente esquema:

__TIENDA__(#aplicacion, nombre_aplicacion, descripcion, #categoria, #etiqueta,
#desarrollador, nombre_apellido_desarrollador, #actualizacion,
descripcion_cambios)

Donde:

- #aplicacion, #categoria, #etiqueta y #desarrollador son únicos en el sistema.
- Una aplicación tiene un nombre y una descripción, y puede actualizarse muchas
veces
- Para cada actualización de una aplicación se registra un texto con los cambios realizados. El #actualización es secuencial, cada aplicación define los suyos y puede repetirse entre distintas aplicaciones.
- Cada aplicación tiene una única categoría y muchas etiquetas. Las etiquetas pueden ir cambiando con cada actualización de la aplicación (en cada actualización puede haber un conjunto diferente de etiquetas). La categoría nunca cambia, es decir que se mantiene igual sin importar las actualizaciones.
- Una aplicación es realizada por varios desarrolladores de los cuales se conoce su nombre y apellido.

Seleccione las DFs válidas / mínimas: Para las que no se seleccionen, indicar el motivo.
- ~~#aplicacion,#actualizacion -> nombre_aplicacion,descripcion~~
    - No es necesario el #actualización para obtener el nombre y descripción. 
- #aplicacion,#actualizacion -> descripcion_cambios
- ~~nombre_desarrollador -> #desarrollador~~
    - Con el nombre no puedo obtener unívocamente a un desarrollador. Además ese atributo no existe.
- #desarrollador -> nombre_apellido_desarrollador
- #aplicación -> #categoria

Encontró alguna dependencia funcional más, que no se menciona entre las opciones?

- #aplicación -> nombre_aplicación, descripción

# Parte II - Dados los siguientes esquemas, realizar todo el proceso de normalización hasta 4FN

## 6. SUSCRIPCION (#suscripcion, email, nombre_usuario, #plan, nombre_plan, texto_condiciones, precio, email_adicional, nombre_adicional, #contenido, titulo, sinopsis, duracion, fecha_adicional)

- Cada suscripción es realizada por un único usuario (identificado por el email) y un plan, pero además hay usuarios adicionales que la utilizan (email_adicional). De cada usuario adicional que se suma a la suscripción, se guarda la fecha.
- Un plan de suscripción tiene un nombre (que no puede garantizarse que sea único en el sistema), condiciones, y un precio mensual.
- Cada contenido tiene un título, sinopsis y duración. El #contenido es único en el sistema, pero del título no puede garantizarse que lo sea.
- De cada suscripción se sabe qué contenidos fueron reproducidos, sin distinción sobre qué usuario (titular o adicionales) reprodujo cada uno.

<u>Dependencias funcionales:</u>

1. email -> nombre_usuario
2. email_adicional -> nombre_adicional
3. #suscripcion, email_adicional -> fecha_adicional
4. #plan -> nombre_plan, texto_condiciones, precio
5. #contenido -> titulo, sinopsis, duracion
6. #suscripcion -> email, #plan

<u>Claves candidatas:</U>

- cc1: {#suscripcion, email_adicional, #contenido} 

SUSCRIPCION no está en BCNF porque el determinante de la DF1 no es superclave en el esquema. Particiono ese esquema en base a la DF1.

- S1(<u>email</u>, nombre_usuario)
- S2(<u>#suscripcion</u>, email, #plan, nombre_plan, texto_condiciones, precio, <u>email_adicional</u>, nombre_adicional, <u>#contenido</u>, titulo, sinopsis, duracion, fecha_adicional)

No perdí información porque S1 ∩ S2 => {email}

S1 está en BCNF porque email es superclave en ese esquema.

S2 no está en BCNF porque el determinante de la DF2 no es supercalve en el esquema. Particiono ese esquema en base a la DF2.

- S3(<u>email_adicional</u>, nombre_adicional)
- S4(<u>#suscripcion</u>, email, #plan, nombre_plan, texto_condiciones, precio, <u>email_adicional, #contenido</u>, titulo, sinopsis, duracion, fecha_adicional)

No perdí información porque S3 ∩ S4 => {email_adicional}

S3 está en BCNF porque email_adicional es superclave en ese esquema.

S4 no está en BCNF porque el determinante de la DF3 no es superclave en el esquema. Particiono S4 a partir de la DF3.

- S5(<u>#suscripcion, email_adicional</u>, fecha_adicional)
- S6(<u>#suscripcion</u>, email, #plan, nombre_plan, texto_condiciones, precio, <u>email_adicional, #contenido</u>, titulo, sinopsis, duracion)

No perdí información porque S5 ∩ S6 => {#suscripcion, email_adicional}

S5 está en BCNF porque {#suscripcion, email_adicional} es superclave de ese esquema.

S6 no está en BCNF porque el determinante de la DF4 no es superclave del esquema. Particiono por la DF4.

- S7(<u>#plan</u>, nombre_plan, texto_condiciones, precio)
- S8(<u>#suscripcion</u>, email, #plan, <u>email_adicional, #contenido</u>, titulo, sinopsis, duracion)

No perdí información porque S7 ∩ S8 => {#plan}

S7 está en BCNF porque {#plan} es superclave en el esquema.

S8 no está en BCNF porque el determinante de la DF5 no es superclave en dicho esquema. Particiono S8 por la DF5.

- S9(<u>#contenido</u>, titulo, sinopsis, duracion)
- S10(<u>#suscripcion</u>, email, #plan, <u>email_adicional, #contenido</u>)

No perdí información porque S9 ∩ S10 => {#contenido}

S9 está en BCNF porque {#contenido} es superclave del esquema

S10 no está en BCNF porque el determinante de la DF6 no es superclave en ese esquema. Particiono S10 por la DF6.

- S11(<u>#suscripcion</u>, email, #plan)
- S12(<u>#suscripcion, email_adicional, #contenido</u>)

No perdí información porque S11 ∩ S12 => {#suscripcion}

S11 está en BCNF porque {#suscripcion} es superclave del esquema

S12 está en BCNF porque sus atributos componen la clave primaria. Toda dependencia funcional que encuentre será trivial.

Clave primaria -> {<u>#suscripcion, email_adicional, #contenido</u>}

<u>Particiones que quedaron en BCNF:</u>
- S1(<u>email</u>, nombre_usuario)
- S3(<u>email_adicional</u>, nombre_adicional)
- S5(<u>#suscripcion, email_adicional</u>, fecha_adicional)
- S7(<u>#plan</u>, nombre_plan, texto_condiciones, precio)
- S9(<u>#contenido</u>, titulo, sinopsis, duracion)
- S11(<u>#suscripcion</u>, email, #plan)
- S12(<u>#suscripcion, email_adicional, #contenido</u>)

<u>Dependencias multivaluadas de S12:</u>

1. #suscripcion ->> #contenido
2. #suscripcion ->> email_adicional

La partición S12 no está en 4FN porque existen dependencias multivaluadas que no son triviales en ese esquema. Particiono ese esquema por la DM1.

- S13(<u>#suscripcion, #contenido</u>)
- S14(<u>#suscripcion, email_adicional</u>)

S13 está en 4FN porque la DM1 es trivial en ese esquema.
La partición S14 también está en 4FN porque la DM2 también es trivial para ese esquema.

<u>Particiones que quedaron en 4FN:</u>

- S1(<u>email</u>, nombre_usuario)
- S3(<u>email_adicional</u>, nombre_adicional)
- S5(<u>#suscripcion, email_adicional</u>, fecha_adicional)
- S7(<u>#plan</u>, nombre_plan, texto_condiciones, precio)
- S9(<u>#contenido</u>, titulo, sinopsis, duracion)
- S11(<u>#suscripcion</u>, email, #plan)
- S13(<u>#suscripcion, #contenido</u>)
- S14(<u>#suscripcion, email_adicional</u>)  => Proyección de S5
- CP(<u>#suscripcion, email_adicional, #contenido</u>)

- S1, S3, S5, S7, S9, S11 están en 4FN porque carecen de DMs.

## 7. MEDICION_AMBIENTAL(medicion, #pozo, valor_medicion, #parametro, fecha_medicion, cuil_operario, #instrumento, nombre_parametro, valor_ref, descripcion_pozo, fecha_perforacion, nombre_parametro, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)

- Cada medición es realizada por un operario en un pozo, en una fecha determinada. En ella se miden varios parámetros, y para cada uno se obtiene un valor. Notar que un mismo parámetro (#parametro) puede ser medido en diferentes mediciones.
Independientemente de las mediciones, todo parámetro tiene un nombre y valor de referencia, y el #parametro es único en el sistema.
- En cada medición se utilizan varios instrumentos, independientemente de los parámetros medidos. De cada instrumento se conoce la marca y modelo.
- De cada operario se conoce su cuit, nombre, apellido y fecha de nacimiento.
- La empresa cuenta con vehículos, y de cada uno se conoce la fecha en la que fue adquirido. El dominio (patente) de cada vehículo es único en el sistema.
- Un pozo tiene una descripción y una fecha de perforación. El identificador #pozo es único en el sistema.

<u>Dependencias funcionales:</u>

1. medicion -> cuil_operario, #pozo, fecha_medicion
2. medicion, #parametro -> valor_medicion
3. #instrumento -> marca_instrumento, modelo_instrumento
4. cuil_operario -> apellido_operario, nombre_operario, fecha_nacimiento
5. #parametro -> nombre_parametro, valor_ref
6. #pozo -> descripcion_pozo, fecha_perforacion
7. dominio_vehiculo -> fecha_adquisicion

<u>Claves candidatas:</u>
- cc1 : {medicion, #parametro, #instrumento, dominio_vehiculo}

MEDICION_AMBIENTAL no está en BCNF porque el determinante de la DF4 no es superclave en ese esquema. Lo particiono por la DF4.

- M1(<u>cuil_operario</u>, apellido_operario, nombre_operario, fecha_nacimiento)
- M2(<u>medicion</u>, #pozo, valor_medicion, <u>#parametro</u>, fecha_medicion, cuil_operario, <u>#instrumento</u>, nombre_parametro, valor_ref, descripcion_pozo, fecha_perforacion, nombre_parametro, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo</u>, fecha_adquisicion)

No perdí información porque M1 ∩ M2 => {cuil_operario}

M1 está en BCNF porque {cuil_operario} es superclave en ese esquema.

M2 no está en BCNF porque el determinante de la DF6 no es superclave en dicho esquema. Lo particiono por la DF6.

- M3(<u>#pozo</u>, descripcion_pozo, fecha_perforacion)
- M4(<u>medicion</u>, #pozo, valor_medicion, <u>#parametro</u>, fecha_medicion, cuil_operario, <u>#instrumento</u>, nombre_parametro, valor_ref, nombre_parametro, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo</u>, fecha_adquisicion)

No perdí información porque M3 ∩ M4 => {#pozo}

M3 está en BCNF porque {#pozo} es superclave del esquema.

M4 no está en BCNF porque el determinante de la DF1 no es superclave en dicho esquema. Lo particiono por la DF1.

- M5(<u>medicion</u>, #pozo, cuil_operario, fecha_medicion)
- M6(<u>medicion</u>, valor_medicion, <u>#parametro</u>, <u>#instrumento</u>, nombre_parametro, valor_ref, nombre_parametro, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo</u>, fecha_adquisicion)

No perdí información porque M5 ∩ M6 => {medicion}

M5 está en BCNF porque {medicion} es superclave del esquema.

M6 no está en BCNF porque el determinante de la DF5 no es superclave. Particiono M6 por la DF5.

- M7(<u>#parametro</u>,nombre_parametro, valor_ref)
- M8(<u>medicion</u>, valor_medicion, <u>#parametro</u>, <u>#instrumento</u>, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo</u>, fecha_adquisicion)

No perdí información porque M7 ∩ M8 => {#parametro}

M7 está en BCNF porque {#parametro} es superclave en ese esquema.

M8 no está en BCNF porque el determinante de la DF3 no es superclave en ese esquema. Particiono M8 por la DF3.

- M9(<u>#instrumento</u>, marca_instrumento, modelo_instrumento)
- M10(<u>medicion</u>, valor_medicion, <u>#parametro</u>, <u>#instrumento</u>, <u>dominio_vehiculo</u>, fecha_adquisicion)

No perdí información porque M9 ∩ M10 => {#instrumento}

M9 está en BCNF porque {#instrumento} es superclave en ese esquema.

M10 no está en BCNF porque el determinante de la DF2 no es superclave del esquema. Particiono M10 por la DF2.

- M11(<u>medicion, #parametro</u>, valor_medicion)
- M12(<u>medicion</u>, <u>#parametro</u>, <u>#instrumento</u>, <u>dominio_vehiculo</u>, fecha_adquisicion)

No perdí información porque M11 ∩ M12 => {medicion, #parametro}

M11 está en BCNF porque {medicion, #parametro} es superclave en el esquema.

M12 no está en BCNF porque el determinante de la DF7 no es superclave en el esquema. Particiono M12 por la DF7.

- M13(<u>dominio_vehiculo</u>, fecha_adquisicion)
- M14(<u>medicion, #parametro, #instrumento,dominio_vehiculo</u>)

No perdí información porque M13 ∩ M14 => {dominio_vehiculo}

M13 está en BCNF porque {dominio_vehiculo} es superclave en el esquema.

M14 está en BCNF porque los atributos forman parte de la clave. Toda dependencia funcional que encuentre será trivial.

<u>Clave primaria:</u> {medicion, #parametro, #instrumento,dominio_vehiculo}

<u>Particiones que quedaron en BCNF:</u>
- M1(<u>cuil_operario</u>, apellido_operario, nombre_operario, fecha_nacimiento)
- M3(<u>#pozo</u>, descripcion_pozo, fecha_perforacion)
- M5(<u>medicion</u>, #pozo, cuil_operario, fecha_medicion)
- M7(<u>#parametro</u>,nombre_parametro, valor_ref)
- M9(<u>#instrumento</u>, marca_instrumento, modelo_instrumento)
- M11(<u>medicion, #parametro</u>, valor_medicion)
- M13(<u>dominio_vehiculo</u>, fecha_adquisicion)
- M14(<u>medicion, #parametro, #instrumento,dominio_vehiculo</u>)

<u>Dependencias multivaluadas en M14:</u>

1. medicion ->> #parametro
2. medicion ->> #instrumento   //Deberían ser una sola DM?
3. ∅ ->> dominio_vehiculo

M14 no está en 4FN, ya que al menos la DM1 no es trivial en esa partición. Particiono por la DM1

- M15(<u>medicion, #parametro</u>)
- M16(<u>medicion, #instrumento, dominio_vehiculo</u>)

M15 está en 4FN porque la DM1 es trivial para esa partición.

M16 no está en 4FN porque la DM3 no es trivial para esa partición. Particiono por la DM3.

- M17(<u>dominio_vehiculo</u>)
- M18(<u>medicion, #instrumento</u>)

Tanto M17 como M18 están en 4FN porque las DMs existentes en cada una son triviales.

<u>Particiones que quedaron en 4FN:</u>

- M1(<u>cuil_operario</u>, apellido_operario, nombre_operario, fecha_nacimiento)
- M3(<u>#pozo</u>, descripcion_pozo, fecha_perforacion)
- M5(<u>medicion</u>, #pozo, cuil_operario, fecha_medicion)
- M7(<u>#parametro</u>,nombre_parametro, valor_ref)
- M9(<u>#instrumento</u>, marca_instrumento, modelo_instrumento)
- M11(<u>medicion, #parametro</u>, valor_medicion)
- M13(<u>dominio_vehiculo</u>, fecha_adquisicion)
- CP(<u>medicion, #parametro, #instrumento,dominio_vehiculo</u>)
- M15(<u>medicion, #parametro</u>)  =>  Proyección de M11
- M17(<u>dominio_vehiculo</u>)   =>  Proyección de M13
- M18(<u>medicion, #instrumento</u>)

M1, M3, M5, M7, M9, M11, y M13 están en 4FN porque carecen de DMs.


## 8. FESTIVALES (#festival, denominacion_festival, localidad, cuil_musico, nombre_musico, fecha_nacimiento, #banda, nombre_banda, estilo_musical, #tema, nombre_tema, duracion, instrumento, cuil_auspiciante, url_plataforma_entradas, #locacion)

- Para cada festival se conoce su denominación y la localidad en la que se realiza. Más de un festival podría tener la misma denominación.
- De cada banda se conoce su nombre y estilo musical.
- De cada músico se conoce su nombre y su fecha de nacimiento. Tenga en cuenta que varios músicos podrían tener el mismo nombre.
- Para cada tema interpretado por una banda en un festival se conoce su nombre y duración. Además, de cada músico que participó en el tema se sabe con qué instrumento lo hizo.
- Los #tema pueden repetirse para las distintas bandas.
- Un festival puede tener varios auspiciantes, y se vendieron entradas al mismo a través de varias plataformas.
- Se tiene además un registro de todas las posibles locaciones donde se pueden realizar los festivales.

<u>Dependencias funcionales:</u>

1. #festival -> denominacion_festival, localidad
2. #banda -> nombre_banda, estilo_musical
3. cuil_musico -> nombre_musico, fecha_nacimiento
4. #festival, #banda, #tema -> nombre_tema, duracion
5. #festival, #banda, #tema, cuil_musico -> instrumento

<u>Claves candidatas</u>

- cc1: {#festival, #banda, cuil_musico, #tema, cuil_auspiciante, url_plataforma_entradas, #locacion}

FESTIVALES no está en BCNF, ya que al menos el determinantes de la DF1 no es superclave del esquema. Particiono FESTIVALES por la DF1.

- F1(<u>#festival</u>, denominacion_festival, localidad)
- F2(<u>#festival, cuil_musico</u>, nombre_musico, fecha_nacimiento, <u>#banda</u>, nombre_banda, estilo_musical, <u>#tema</u>, nombre_tema, duracion, instrumento, <u>cuil_auspiciante, url_plataforma_entradas, #locacion</u>)

No perdí información porque F1 ∩ F2 => {#festival}

F1 está en BCNF porque {#festival} es superclave en el esquema.

F2 no está en BCNF porque el determinante de la DF2 no es superclave en esa partición. La vuelvo a particionar por la DF2.

- F3(<u>#banda</u>, nombre_banda, estilo_musical)
- F4(<u>#festival, cuil_musico</u>, nombre_musico, fecha_nacimiento, <u>#banda, #tema</u>, nombre_tema, duracion, instrumento, <u>cuil_auspiciante, url_plataforma_entradas, #locacion</u>)

No perdí información porque F3 ∩ F4 => {#banda}

F3 está en BCNF porque {#banda} es superclave en el esquema.

F4 no está en BCNF porque el determinante de la DF3 no es superclave en el esquema. Particiono F4 por la DF3

- F5(<u>cuil_musico</u>, nombre_musico, fecha_nacimiento)
- F6(<u>#festival, cuil_musico, #banda, #tema</u>, nombre_tema, duracion, instrumento, <u>cuil_auspiciante, url_plataforma_entradas, #locacion</u>)

No perdí información porque F5 ∩ F6 => {cuil_musico}

F5 está en BCNF porque {cuil_musico} es superclave en el esquema.

F6 no está en BCNF porque el determinante de la DF4 no es superclave en el esquema. Particiono F6 por la DF4.

- F7(<u>#festival, #banda, #tema</u>, nombre_tema, duracion)
- F8(<u>#festival, cuil_musico, #banda, #tema</u>, instrumento, <u>cuil_auspiciante, url_plataforma_entradas, #locacion</u>)

No perdí información porque F7 ∩ F8 => {#festival, #banda, #tema}

F7 está en BCNF porque {#festival, #banda, #tema} es superclave del esquema.

F8 no está en BCNF porque el determinante de la DF5 no es superclave en ese esquema. Particiono F8 por la DF5.

- F9(<u>#festival, #banda, #tema, cuil_musico</u>, instrumento)
- F10(<u>#festival, cuil_musico, #banda, #tema, cuil_auspiciante, url_plataforma_entradas, #locacion</u>)

No perdí información porque F9 ∩ F10 => {#festival, #banda, #tema, cuil_musico}

F9 está en BCNF porque {#festival, #banda, #tema, cuil_musico} es superclave de ese esquema.

F10 está en BCNF porque todos los atributos de ese esquema son parte de la clave. Cualquier DF que encontremos es trivial.

<u>Clave primaria:</u> {#festival, #banda, cuil_musico, #tema, cuil_auspiciante, url_plataforma_entradas, #locacion}

<u>Particiones que quedaron en BCNF:</u>

- F1(<u>#festival</u>, denominacion_festival, localidad)
- F3(<u>#banda</u>, nombre_banda, estilo_musical)
- F5(<u>cuil_musico</u>, nombre_musico, fecha_nacimiento)
- F7(<u>#festival, #banda, #tema</u>, nombre_tema, duracion)
- F9(<u>#festival, #banda, #tema, cuil_musico</u>, instrumento)
- F10(<u>#festival, cuil_musico, #banda, #tema, cuil_auspiciante, url_plataforma_entradas, #locacion</u>)

<u>Dependencias multivaluadas de F10:</u>

1. ∅ ->> #locacion
2. #festival ->> cuil_auspiciante
3. #festival ->> url_plataforma_entradas
4. #festival, #banda, #tema ->> cuil_musico

La partición F10 no está en 4FN porque al menos la DM1 no es trivial en F10. Particiono F10 por la DM1.

- F11(<u>#locacion</u>)
- F12(<u>#festival, cuil_musico, #banda, #tema, cuil_auspiciante, url_plataforma_entradas</u>)

La partición F11 cumple con la definción de 4FN porque la DM1 es trivial en ese esquema.

La partición F12 no está en 4FN porque al menos la DM2 no es trivial. Particiono F12 por DM2.

- F13(<u>#festival, cuil_auspiciante</u>)
- F14(<u>#festival, cuil_musico, #banda, #tema, url_plataforma_entradas</u>)

La partición F13 está en 4FN porque la DM2 es trivial en ese esquema.

La partición F14 no cumple con la definción de 4FN porque al menos la DM3 no es trivial para ese esquema. Particiono F14 por DM3.

- F15(<u>#festival, url_plataforma_entradas</u>)
- F16(<u>#festival, cuil_musico, #banda, #tema</u>)

La partición F15 está en 4FN porque la DM3 es trivial en ese esquema.

En F16 está la DM4 que es trivial, por lo tanto F16 está en 4FN.

- //Consultar solución. Debería seguir particionando??