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

## 9. DISPOSITIVOS (Marca_id, descripMarca, modelo_id, descripModelo, equipo_tipo_id, descripEquipoTipo, nombreEmpresa, cuit, direcciónEmpresa, usuario_id, apyn, direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)

- Para cada equipo interesa conocer su tipo, modelo, imei, fecha en que se dio de alta, fecha en que se da de baja y las observaciones que sean necesarias.
- De cada marca se conoce su descripción
- De cada modelo se conoce su descripción y a qué marca pertenece.
- Para cada plan, se registra qué empresa lo brinda, descripción e importe del mismo.
- Para cada tipo de equipo se conoce la descripción
- Para cada empresa se registra el nombre, cuit y dirección
- De cada usuario se registra su nombre y apellido, número de documento, dirección y CUIL
- Para cada línea se necesita registrar qué plan posee, la fecha de alta de la línea, la fecha de baja, el equipo que la posee y el usuario de la misma.

<u>Dependencias funcionales:</u>

1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_id, equipo_tipo_id, modelo_id, fec_alta, fec_baja, observaciones
3. marca_id -> descripMarca
4. modelo_id -> descriptModelo, marca_id
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> apyn, direcciónUsuario, usuario_id
10. linea_id -> plan_id, fec_alta_linea, fec_baja_linea, equipo_id, cuil
11. linea_id -> plan_id, fec_alta_linea, fec_baja_linea, imei, cuil
12. linea_id -> plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id
13. linea_id -> plan_id, fec_alta_linea, fec_baja_linea, imei, usuario_id

<u>Claves candidatas:</u>

- cc1: {linea_id}

DISPOSITIVOS no cumple con la definición de BCNF porque al menos el determinante de la DF3 no es superclave en el esquema. Particiono DISPOSITIVOS por la DF3.

- D1(<u>marca_id</u>,descripMarca)
- D2(Marca_id, modelo_id, descripModelo, equipo_tipo_id, descripEquipoTipo, nombreEmpresa, cuit, direcciónEmpresa, usuario_id, apyn, direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja, observaciones, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D1 ∩ D2 => {marca_id}

En D1 vale la DF3, donde el determinante {marca_id} es superclave, por lo tanto está en BCNF

D2 no está en BCNF porque al menos el determinante de la DF4 no es superclave en la partición. Particiono D2 por la DF4.

- D3(<u>modelo_id</u>, descripModelo, marca_id)
- D4(modelo_id, equipo_tipo_id, descripEquipoTipo, nombreEmpresa, cuit, direcciónEmpresa, usuario_id, apyn, direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja, observaciones, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D3 ∩ D4 => {modelo_id}

En D3 vale la DF4, donde el determinante {modelo_id} es superclave, por lo tanto está en BCNF.

D4 no está en BCNF porque al menos el determinante de la DF6 no es superclave en la partición. Particiono D4 por la DF6.

- D5(<u>equipo_tipo_id</u>, descripEquipoTipo)
- D6(modelo_id, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, usuario_id, apyn, direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja, observaciones, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D5 ∩ D6 => {equipo_tipo_id}

En D5 vale la DF6, donde el determinante {equipo_tipo_id} es superclave, por lo tanto está en BCNF.

D6 no está en BCNF porque al menos el determinante de la DF7 no es superclave en la partición. Particiono D6 por la DF7.

- D7(<u>cuit</u>, nombreEmpresa, direcciónEmpresa)
- D8(modelo_id, equipo_tipo_id, cuit, usuario_id, apyn, direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja, observaciones, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D7 ∩ D8 => {cuit}

En D7 vale la DF7, donde el determinante {cuit} es superclave, por lo tanto está en BCNF.

D8 no está en BCNF porque al menos el determinante de la DF5 no es supercalve en la partición. Particiono D8 por la DF5.

- D9(<u>plan_id</u>, cuit, descripPlan, importe)
- D10(modelo_id, equipo_tipo_id, usuario_id, apyn, direcciónUsuario, cuil, plan_id, equipo_id, imei, fec_alta, fec_baja, observaciones, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D9 ∩ D10 => {plan_id}

En D9 vale la DF9, donde el determinante {plan_id} es superclave, por lo tanto está en BCNF.

D10 no está en BCNF porque al menos el determinante de la DF8  no es superclave en la partición. Particiono D10 por la DF8

//Consultar a partir de este punto

- D11(<u>usuario_id</u>, apyn, direcciónUsuario, cuil)
- D12(modelo_id, equipo_tipo_id, usuario_id, plan_id, equipo_id, imei, fec_alta, fec_baja, observaciones, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D11 ∩ D12 => {usuario_id}

En D11 vale la DF8, donde el determinante {usuario_id} es superclave, por lo tanto está en BCNF.

La DF9 no vale ni en D11 ni en D12, aplico el algoritmo de pérdida de DFs para ver si se perdió la DF9:

```cpp
Res = cuil
i = 1
Res = cuil ∪ ((cuil ∩ {marca_id,descripMarca})+ ∩ {marca_id,descripMarca}) = cuil

i = 3
Res = cuil ∪ ((cuil ∩ {modelo_id, descripModelo, marca_id})+ ∩ modelo_id, descripModelo, marca_id) = cuil

i = 5
Res = cuil ∪ ((cuil ∩ {equipo_tipo_id, descripEquipoTipo})+ ∩ {equipo_tipo_id, descripEquipoTipo}) = cuil

i = 7
Res = cuil ∪ ((cuil ∩ {cuit, nombreEmpresa, direcciónEmpresa})+ ∩ {cuit, nombreEmpresa, direcciónEmpresa}) = cuil

i = 9
Res = cuil ∪ ((cuil ∩ {cuit, nombreEmpresa, direcciónEmpresa})+ ∩ {cuit, nombreEmpresa, direcciónEmpresa}) = cuil

i = 11
Res = cuil ∪ ((cuil ∩ {usuario_id, apyn, direcciónUsuario, cuil})+ ∩ {usuario_id, apyn, direcciónUsuario, cuil}) = cuil ∪ ((cuil)+ ∩ {usuario_id, apyn, direcciónUsuario, cuil}) = {usuario_id, apyn, direcciónUsuario, cuil}
```

- Con el atributo cuil logré obtener a los atributos que determinaba en la DF9. Por lo tanto dicha DF no se perdió.

D12 no está en BCNF porque al menos el determinante de la DF1 no es superclave en la partición. Particiono D12 por la DF1.

- D13(<u>equipo_id</u>, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones)
- D14(usuario_id, plan_id, equipo_id, <u>línea_id</u>, fec_alta_linea, fec_baja_linea)

No perdí información porque D13 ∩ D14 => {equipo_id}

En D13 vale la DF1, cuyo determinante {equipo_id} es superclave, por lo tanto D13 cumple con BCNF.

La DF2 no vale en D13 ni en D14. Aplico el algoritmo para analizar si DF2 se perdió:

```cpp
Res = imei

i = 1
Res = imei ∪ ((imei ∩ {marca_id,descripMarca})+ ∩ {marca_id,descripMarca}) = imei

i = 3
Res = imei ∪ ((imei ∩ {modelo_id, descripModelo, marca_id})+ ∩ {modelo_id, descripModelo, marca_id}) = imei

i = 5
Res = imei ∪ ((imei ∩ {equipo_tipo_id, descripEquipoTipo})+ ∩ {equipo_tipo_id, descripEquipoTipo}) = imei

i = 7
Res = imei ∪ ((imei ∩ {cuit, nombreEmpresa, direcciónEmpresa})+ ∩ {cuit, nombreEmpresa, direcciónEmpresa}) = imei

i = 9
Res = imei ∪ ((imei ∩ {plan_id, cuit, descripPlan, importe})+ ∩ {plan_id, cuit, descripPlan, importe}) = imei

i = 11
Res = imei ∪ ((imei ∩ {usuario_id, apyn, direcciónUsuario, cuil})+ ∩ {usuario_id, apyn, direcciónUsuario, cuil}) = imei

i = 13
Res = imei ∪ ((imei ∩ {equipo_id, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones})+ ∩ {equipo_id, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones}) = imei ∪ ((imei)+ ∩ {equipo_id, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones}) = {equipo_id, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones}
```

- Con el atributo imei pude recuperar todos los atributos que determinaba en la DF2, por lo tanto dicha DF no se perdió.

D14 no está en BCNF porque el determinante de la DF12 no es superclave en esa partición. Particiono D14 por la DF12

- D15(<u>linea_id</u>, plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id)
- D16(<u>linea_id</u>)

No perdí información porque D15 ∩ D16 => {linea_id}

D15 está en BCNF porque el determinante de la DF12 es superclave en ese esquema.

La DF11 no está en D15 ni en D16. Aplico el algoritmo de pérdidas de DFs para verificar si se perdió la DF11:

```cpp
Res = linea_id

i = 1
Res = linea_id ∪ ((linea_id ∩ {marca_id,descripMarca})+ ∩ {marca_id,descripMarca}) = linea_id

i = 3
Res = linea_id ∪ ((linea_id ∩ {modelo_id, descripModelo, marca_id})+ ∩ {modelo_id, descripModelo, marca_id}) = linea_id

i = 5
Res = linea_id ∪ ((linea_id ∩ {equipo_tipo_id, descripEquipoTipo})+ ∩ {equipo_tipo_id, descripEquipoTipo}) = linea_id

i = 7
Res = linea_id ∪ ((linea_id ∩ {cuit, nombreEmpresa, direcciónEmpresa})+ ∩ {cuit, nombreEmpresa, direcciónEmpresa}) = linea_id

i = 9
Res = linea_id ∪ ((linea_id ∩ {plan_id, cuit, descripPlan, importe})+ ∩ {plan_id, cuit, descripPlan, importe}) = linea_id

i = 11
Res = linea_id ∪ ((linea_id ∩ {usuario_id, apyn, direcciónUsuario, cuil})+ ∩ {usuario_id, apyn, direcciónUsuario, cuil}) = linea_id

i = 13
Res = linea_id ∪ ((linea_id ∩ {equipo_id, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones})+ ∩ {equipo_id, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones}) = linea_id

i = 15
Res = linea_id ∪ ((linea_id ∩ {linea_id, plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id})+ ∩ {linea_id, plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id}) = linea_id ∪ ((linea_id)+ {linea_id, plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id}) = {linea_id, plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id}
```

- Con el atributo linea_id pude obtener todos los atributos que determina en la DF11, por lo tanto dicha DF no se perdió.
- Lo mismo aplica para la DF10 y DF13

La partición D16 está en BCNF porque el atributo que la conforma es la clave. Todas las DFs que encuentre serán triviales.

Todas las particiones están en 4FN al no haber dependencias multivaluadas.

<u>Esquemas en 4FN: </u>

- D1(<u>marca_id</u>,descripMarca)
- D3(<u>modelo_id</u>, descripModelo, marca_id)
- D5(<u>equipo_tipo_id</u>, descripEquipoTipo)
- D7(<u>cuit</u>, nombreEmpresa, direcciónEmpresa)
- D9(<u>plan_id</u>, cuit, descripPlan, importe)
- D11(<u>usuario_id</u>, apyn, direcciónUsuario, cuil)
- D13(<u>equipo_id</u>, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones)
- D15(<u>linea_id</u>, plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id)
- CP(<u>linea_id</u>)

## 10. ORGANIZACION_EVENTOS (#evento, fecha_evento, motivo_evento, #salon, nombre_salon, #grupo, nombre_grupo, nro_integrantes_grupo, #organizador, nombre_organizador, telefono_organizador, años_exp_organizador, #staff, nombre_staff, telefono_staff, rol_staff)

- De cada evento se conoce un identificador, que es único, la fecha, el motivo, el salón de fiestas donde se desarrollará y el grupo que tocará en el mismo.
- De cada salón de fiestas posible se conoce un número identificador, único en el sistema y su ubicación.
- De los grupos se conoce un identificador (único) su nombre y la cantidad de integrantes que lo conforman. Además, se sabe que cada grupo de los registrados en el sistema tiene un contrato de exclusividad con un único organizador.
- De los organizadores se conoce su nombre, teléfono y los años de experiencia que lleva en su trabajo. También tiene asociado un número que lo identifica inequívocamente.
- Cada organizador tiene contrato con muchos grupos, sin embargo este solo organiza cada una de sus fechas disponibles con un único grupo, que será el que toque la noche del evento.   // Preguntar por esto
- Cada evento contrata a una serie de personas que serán el staff del mismo. De cada uno de estos se conoce un identificador, único en el sistema, el nombre, el teléfono y el rol que ocupa.

<u>Dependencias funcionales: </u>

- (df1): #evento -> fecha_evento, motivo_evento, #salon, #grupo
- (df2): #salon -> nombre_salon
- (df3): #grupo -> nombre_grupo, nro_integrantes_grupo, #organizador
- (df4): #organizador -> nombre_organizador, telefono_organizador, años_exp_organizador
- (df5): #staff -> nombre_staff, telefono_staff, rol_staff
- (df6): #organizador, fecha_evento -> #grupo  //Preguntar

<u>Claves candidatas:</u>

- cc1: {#evento, #staff}
- cc2: {#organizador, fecha_evento, #staff}

ORGANIZACION_EVENTOS no está en BCNF porque al menos el determinante de la DF2 no es superclave en el esquema. Lo particiono por la DF2.

- O1(<u>#salon</u>, nombre_salon)
- O2(<u>#evento</u>, fecha_evento, motivo_evento, #salon, #grupo, nombre_grupo, nro_integrantes_grupo, #organizador, nombre_organizador, telefono_organizador, años_exp_organizador, <u>#staff</u>, nombre_staff, telefono_staff, rol_staff)

No perdí información porque O1 ∩ O2 => {#salon}

O1 está en BCNF porque el determinante {#salon} de la DF2 es superclave en la partición.

O2 no está en BCNF porque al menos el determinante de la DF4 no es superclave en el esquema. Particiono O2 por la DF4.

- O3(<u>#organizador</u>, nombre_organizador, telefono_organizador, años_exp_organizador)
- O4(<u>#evento</u>, fecha_evento, motivo_evento, #salon, #grupo, nombre_grupo, nro_integrantes_grupo, #organizador, <u>#staff</u>, nombre_staff, telefono_staff, rol_staff)

No perdí información porque O3 ∩ O4 => {#evento}

O3 está en BCNF porque el determinante {#organizador} de la DF4 es superclave en la partición.

O4 no está en BCNF porque al menos el determinante de la DF5 no es superclave en O4. Particiono por DF5.

- O5(<u>#staff</u>, nombre_staff, telefono_staff, rol_staff)
- O6(<u>#evento</u>, fecha_evento, motivo_evento, #salon, #grupo, nombre_grupo, nro_integrantes_grupo, #organizador, <u>#staff</u>)

No perdí información porque O5 ∩ O6 => {#staff}

O5 está en BCNF porque el determinante {#staff} de la DF5 es superclave en O5.

O6 no está en BCNF porque al menos el determinante de la DF3 no es superclave en O6. Particiono por DF3.

- O7(<u>#grupo</u>, nombre_grupo, nro_integrantes_grupo, #organizador)
- O8(<u>#evento</u>, fecha_evento, motivo_evento, #salon, #grupo, <u>#staff</u>)

No perdí información porque O7 ∩ O8 => {#grupo}

O7 está en BCNF porque el determinante {#grupo} de la DF3 es superclave en O7.

O8 no está en BCNF porque al menos el determinante de la DF1 no es superclave en O8.

¿Qué pasó con la DF6 {#organizador, fecha_evento -> #grupo}? Aplico el algoritmo de perdida de DFs para verificar si se perdió la DF6:

```cpp
Res = {#organizador, fecha_evento}

PRIMERA ITERACION
i=1
Res = {#organizador, fecha_evento} ∪ (({#organizador, fecha_evento} ∩ {#salon, nombre_salon})⁺ ∩ {#salon, nombre_salon}) = {#organizador, fecha_evento}

i=3
Res = {#organizador, fecha_evento} ∪ (({#organizador, fecha_evento} ∩ {#organizador, nombre_organizador, telefono_organizador, años_exp_organizador})⁺ ∩ {#organizador, nombre_organizador, telefono_organizador, años_exp_organizador}) => {#organizador, fecha_evento} ∪ ((#organizador)⁺ ∩ {#organizador, nombre_organizador, telefono_organizador, años_exp_organizador}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

// Res cambió

i=5
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#staff, nombre_staff, telefono_staff, rol_staff})⁺ ∩ {#staff, nombre_staff, telefono_staff, rol_staff}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

i=7
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#grupo, nombre_grupo, nro_integrantes_grupo, #organizador})⁺ ∩ {#grupo, nombre_grupo, nro_integrantes_grupo, #organizador}) => {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ ((#organizador)⁺ ∩ {#grupo, nombre_grupo, nro_integrantes_grupo, #organizador}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

i=8
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#evento, fecha_evento, motivo_evento, #salon, #grupo, #staff})⁺ ∩ {#evento, fecha_evento, motivo_evento, #salon, #grupo, #staff}) => {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ ((fecha_evento)⁺ ∩ {#evento, fecha_evento, motivo_evento, #salon, #grupo, #staff}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

//RES cambió, volvemos a iterar
SEGUNDA ITERACION
i=1
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#salon, nombre_salon})⁺ ∩ {#salon, nombre_salon}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

i=3
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#organizador, nombre_organizador, telefono_organizador, años_exp_organizador})⁺ ∩ {#organizador, nombre_organizador, telefono_organizador, años_exp_organizador}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

i=5
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#staff, nombre_staff, telefono_staff, rol_staff})⁺ ∩ {#staff, nombre_staff, telefono_staff, rol_staff}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

i=7
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#grupo, nombre_grupo, nro_integrantes_grupo, #organizador})⁺ ∩ {#grupo, nombre_grupo, nro_integrantes_grupo, #organizador}) => {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ ((#organizador)⁺ ∩ {#grupo, nombre_grupo, nro_integrantes_grupo, #organizador}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}

i=8
Res = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ (({#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∩ {#evento, fecha_evento, motivo_evento, #salon, #grupo, #staff})⁺ ∩ {#evento, fecha_evento, motivo_evento, #salon, #grupo, #staff}) => {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador} ∪ ((fecha_evento)⁺ ∩ {#evento, fecha_evento, motivo_evento, #salon, #grupo, #staff}) = {#organizador, fecha_evento, nombre_organizador, telefono_organizador, años_exp_organizador}
```

- Con el determinante de la Df6 {#organizador, fecha_evento} no pude obtener #grupo, por lo tanto el esquema no puede ser llevado a BCNF.

Por cada Dependencia Funcional hacemos una partición:

- O1(<u>#evento</u>, fecha_evento, motivo_evento, #salon, #grupo)
- O2(<u>#salon</u>, nombre_salon)
- O3(<u>#grupo</u>, nombre_grupo, nro_integrantes_grupo, #organizador)
- O4(<u>#organizador</u>, nombre_organizador, telefono_organizador, años_exp_organizador)
- O5(<u>#staff</u>, nombre_staff, telefono_staff, rol_staff)
- O6(<u>#organizador, fecha_evento</u>, #grupo)
- O7(<u>#evento, #staff</u>)
  - Necesaria para tener la clave primaria.

Dependencias multivaluadas de O7:

- (dm1): #evento ->> #staff

//Preguntar: Por como está conformada la partición O7, ya se encuentra en 4FN, ya que solo mantiene esos dos atributos {#evento, #staff}, además, la DM1 es trivial para dicho esquema.

Esquemas que están en 4FN:
- O1(<u>#evento</u>, fecha_evento, motivo_evento, #salon, #grupo)
- O2(<u>#salon</u>, nombre_salon)
- O3(<u>#grupo</u>, nombre_grupo, nro_integrantes_grupo, #organizador)
- O4(<u>#organizador</u>, nombre_organizador, telefono_organizador, años_exp_organizador)
- O5(<u>#staff</u>, nombre_staff, telefono_staff, rol_staff)
- O6(<u>#organizador, fecha_evento</u>, #grupo)
- O7(<u>#evento, #staff</u>)

O1, O2, O3, O4, O5 y O6 están en 4FN porque carecen de DMs.