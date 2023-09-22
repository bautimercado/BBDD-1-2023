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