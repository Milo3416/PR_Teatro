# Mapa interactivo - Auditorio Jorge Jimenez Cantu

Este paquete contiene el mapa interactivo de asientos, la integracion para registrar reservas en Google Sheets, un dashboard de ventas y herramientas de administracion para eliminar asientos desde la pagina.

## Archivos

- `index.html`: sitio completo en un solo archivo. Incluye HTML, CSS y JavaScript.
- `apps_script.gs`: codigo para pegar en Google Apps Script y conectar la hoja `Teatro_Jimenez_Cantu`.

## Zonas y precios

- GOLDEN: $200
- GENERAL 1: $150
- GENERAL 2: $100

## Estados de asientos

La leyenda maneja cuatro estados:

1. Disponible
2. Seleccionado
3. Ocupado total
4. Ocupado parcial

Al registrar una reserva puedes elegir si los asientos quedan como `Ocupado total` o `Ocupado parcial`. Los asientos parciales quedan bloqueados en el mapa, se pintan en color marron y al pasar el cursor muestran el monto pagado hasta el momento.

## Sincronizacion Google Sheets <-> app web

- Si eliminas un asiento desde la app web, se borra la fila correspondiente en Google Sheets y el asiento queda disponible en el mapa.
- Si borras manualmente una fila en Google Sheets, el asiento dejara de aparecer como ocupado en la app web en la siguiente sincronizacion.
- La app sincroniza al cargar, al volver a la pestana, al enfocar la ventana y de forma automatica cada 15 segundos.
- El boton `Actualizar desde Sheets` fuerza una sincronizacion manual.

## Dashboard incluido

Debajo de los datos de boletos se muestran estas metricas:

1. Ocupado total
2. Ocupado parcial
3. Boletos faltantes
4. Recaudado total
5. Porcentaje de avance
6. Totales por zona: ocupado total, ocupado parcial, faltantes y recaudado

El dashboard se sincroniza con Google Sheets usando la misma URL de Apps Script.

## Administracion desde la pagina

En el panel `Administrar asientos` puedes:

- Actualizar el mapa desde Google Sheets.
- Activar `Modo eliminar` para tocar asientos ocupados y eliminarlos de Google Sheets.
- Escribir IDs separados por coma, por ejemplo `1A, 1B, 12N`, y eliminarlos de Google Sheets.
- Activar `Modo liquidar parcial` para tocar asientos marrones, convertirlos a `Ocupado total`, actualizar Google Sheets y generar el boleto oficial.
- Escribir IDs de pago parcial y liquidarlos sin usar el mapa.
- Limpiar el cache local y volver a sincronizar con Google Sheets.

## Columnas que se registran en Google Sheets

El script crea o usa la pestana `Reservas` con estas columnas:

1. Huella de tiempo
2. Nombre del Cliente
3. Celular / WhatsApp
4. ID del Asiento
5. Costo del boleto
6. Zona
7. ID de reserva
8. Total de reserva
9. Cantidad de boletos
10. Estado
11. Origen
12. Notas
13. Pago realizado
14. Saldo restante

## Configuracion rapida

1. Abre o crea la hoja de calculo llamada `Teatro_Jimenez_Cantu`.
2. Ve a `Extensiones > Apps Script`.
3. Pega el contenido de `apps_script.gs`.
4. Da clic en `Implementar > Nueva implementacion` o `Gestionar implementaciones > Editar`.
5. Tipo: `Aplicacion web`.
6. Ejecutar como: `Yo`.
7. Acceso: `Cualquier usuario`.
8. Si ya tenias una implementacion, selecciona `Nueva version` para conservar la misma URL `/exec`.

## Logica incluida

- Asientos generados dinamicamente por seccion.
- Estados: disponible, seleccionado, ocupado total y ocupado parcial.
- Calculo automatico del total.
- Captura de nombre y WhatsApp del cliente.
- Registro de cada asiento en Google Sheets, una fila por boleto.
- Consulta de asientos ocupados desde Google Sheets para bloquearlos en el mapa.
- Eliminacion de asientos desde la pagina y sincronizacion inversa con Sheets.
- Liquidacion de pagos parciales desde la pagina, actualizando Sheets y generando el boleto oficial.
- Dashboard de ventas con totales y metricas por zona.
- Apertura de WhatsApp solo al numero del cliente.
- Generacion de imagen PNG del boleto por categoria: dorado para Golden, rojo para General 1 y azul para General 2.

URL configurada de Apps Script:
https://script.google.com/macros/s/AKfycbze1fMork7l0U4us6orsP7bJ1kC-B80BfRLcs-l9ZZnR_TIE2T7GPEjg9kkh8z4pgHKoQ/exec

## Mensaje de WhatsApp

El mensaje se genera para el numero del cliente con este formato:

```text
Hola *[Nombre del cliente],* te compartimos tu reserva de boletos para la funcion de las Huntrix - Auditorio Jorge Jimenez Cantu.

Nombre: [Nombre del cliente]
Contacto: [WhatsApp]

GENERAL 2: [asientos]
GENERAL 1: [asientos]
GOLDEN: [asientos]

Total: [$total]
Ubicacion: https://maps.app.goo.gl/t8iTc4FMj5uHDVzK8

*Producciones Rainbow*
```

Nota: los enlaces `wa.me` de WhatsApp permiten precargar el texto, pero por seguridad WhatsApp requiere que el usuario presione enviar. Para la imagen del boleto, el sitio genera y descarga el PNG para que se pueda adjuntar manualmente cuando el navegador no permita compartir archivos directamente.


## Actualizacion de pagos parciales

- En `Ocupado total` se genera la imagen PNG del boleto oficial.
- La imagen incluye la leyenda: `Esta ya es una entrada 100% oficial y aprobada para el espectaculo de Producciones Rainbow`.
- En `Ocupado parcial` aparece el campo `Pago recibido hasta el momento`.
- Para pagos parciales no se genera imagen; WhatsApp envia el total, el pago realizado y el saldo pendiente.
- Google Sheets guarda por asiento el `Pago realizado` y el `Saldo restante`.
- Los asientos parciales se muestran en marron y el tooltip dice `Pago Parcial de $monto`.
- Desde administracion puedes liquidar uno o varios asientos parciales; al hacerlo pasan a `Ocupado total`, se actualiza el pago a precio completo, el saldo queda en `$0` y se abre WhatsApp con el boleto oficial.

Recuerda pegar el nuevo `apps_script.gs` en Apps Script y publicar una nueva version de la Web App.
