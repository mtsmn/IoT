---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Visualización de datos en tiempo real utilizando paneles y tarjetas
{: #boards_and_cards}


**Importante:** estamos lanzando una versión Beta con una nueva forma de definir reglas en los datos del dispositivo IoT como parte de un programa más ambicioso de cambios para mejorar la forma en que {{site.data.keyword.iot_full}} distribuye reglas y acciones.

Para ver más información, consulte la publicación del blog sobre [Un enfoque alternativo a la definición de reglas en datos de IoT ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}.

Para empezar a definir sus propias reglas, consulte la documentación sobre [Creación de reglas incorporadas (Beta)](information_management/im_rules.html).

## Acerca de paneles y tarjetas

Crear paneles y tarjetas para crear y compartir sus propios paneles de instrumentos que visualizan los datos de dispositivo en tiempo real.

Al utilizar los paneles y las tarjetas, puede visualizar gráficamente valores de conjuntos de datos de uno o varios dispositivos para proporcionar una visión general rápida y mejorar la comprensión de los datos. Cree paneles y añada tarjetas que muestren los datos como números en bruto, gráficos en tiempo real, indicadores, etc. Añada miembros a los paneles para compartirlos con otros usuarios de su organización. Organice las tarjetas y añada divisores de texto explicativo para ajustar la presentación.  

También puede expandir el conjunto predeterminado de tarjetas mediante la [creación de sus propias tarjetas personalizadas](custom_cards/custom-cards.html).

![Mostrando datos en tiempo real con tarjetas.](images/boards_and_cards.svg "Mostrando datos en tiempo real con tarjetas.")

## Paneles predeterminados
{: #default_boards}
El panel de control de {{site.data.keyword.iot_short_notm}} tiene los siguientes paneles predeterminados:

|Nombre de panel | Descripción | Tarjetas incluidas
|:---|:---|:---|  
|Visión general de uso  | Estadísticas de uso para su organización. Lista tipos de dispositivos y los datos que se consumen. | <ul><li>Tipos de dispositivos<li>Datos transferidos</ul>
|Analítica centrada en las reglas | Reglas para su organización. Las alertas desencadenadas de lista de tarjetas adicionales, los dispositivos asociados, las propiedades de dispositivos y la información de alertas. | <ul><li>Reglas que gestiono<li>Alertas de reglas<li>Información de alerta de reglas<li>Dispositivos asociados<li>Información de dispositivo<li>Propiedades de dispositivo</ul>  
|Analítica centradas en los dispositivos | Dispositivos conectados a su organización. Las tarjetas adicionales muestran alertas para un dispositivo seleccionado, información para un dispositivo seleccionado, propiedades de dispositivos e información de alerta. | <ul><li>Dispositivos que me interesan<li>Información de dispositivo<li>Alertas de regla para ese dispositivo<li>Información de alerta de reglas<li>Propiedades de dispositivo</ul>
|Visión general de riesgo y seguridad (Beta) | Estado general de seguridad de su organización. Las operaciones de sistemas y los analistas de seguridad pueden ver detalles sobre la conformidad, el estado de conexión de dispositivos, las causas de las anomalías de conexión y los dispositivos que están bloqueados o que se permiten mediante una lista negra o una lista blanca.  Desde la tarjeta de conformidad de conexión, los usuarios pueden acceder a un informe detallado sobre los dispositivos no conformes y pueden exportar el informe a Excel. | <ul><li>Conformidad de política<li>Seguridad de conexión<li>Conformidad de lista blanca/lista negra</ul>

Puede actualizar estos paneles añadiendo, actualizando y eliminando tarjetas.

Para restablecer un panel predeterminado a su estado original, puede suprimirlo. El panel se volverá a crear con las tarjetas originales.
{: tip}

## Creación de paneles y tarjetas
{: #visualizing_data}

{{site.data.keyword.iot_short_notm}} proporciona un panel de control incorporado que puede utilizar para visualizar los datos en tiempo real que está devolviendo el dispositivo. De forma predeterminada, la página Visión general muestra información de uso sobre la organización de {{site.data.keyword.iot_short_notm}}, como por ejemplo los datos y el espacio de almacenamiento que se consume. Para ver datos de dispositivos en tiempo real a medida que fluyan, añada tarjetas específicas del dispositivo a esta página.

Para obtener una receta de instrucciones paso a paso para visualizar datos de dispositivo en tiempo real, consulte la receta [Configuración de paneles y tarjetas en el nuevo panel de control de Watson IoT ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/configuring-the-cards-in-the-new-watson-iot-dashboard/){: new_window}.
{: tip}

Para crear un panel y añadirle una tarjeta:
1. En el panel de control de {{site.data.keyword.iot_short_notm}}, seleccione **Paneles**.
2. Seleccione un panel para el que tenga derechos de edición para crear un nuevo panel.
3. En el panel, pulse **Añadir nueva tarjeta**.
3. Seleccione un tipo de tarjeta.  
**Sugerencia:** Si no está seguro sobre qué visualización elegir para una tarjeta de tipo de dispositivos, seleccione **Visualización genérica**. Puede cambiar el tipo de tarjeta más adelante.
<dl>
<dt>Dispositivos</dt>
<dd><table>
<thead>
<tr>
<th>Tipo</th>
<th>Datos que se visualizan</th>
</tr>
</thead>
<tbody>
<tr>
<td>Visualización genérica</td>
<td>El valor de uno o más conjuntos de datos. </br>**Consejo:** Para ver hasta tres valores de punto de datos en una tabla pequeña, elija el tamaño grande de widgets. </td>
</tr>
<tr>
<td>Gráfico de líneas</td>
<td>Uno o varios conjuntos de datos en un diagrama de desplazamiento en tiempo real. Utilice el menú Valores para establecer el rango y la retención de datos, el aspecto de los gráficos, etc. </td>
<tr>
<td>Gráfico de barras</td>
<td>Valores de conjuntos de datos en barras etiquetadas. Utilice el menú Valores para conmutar la dirección de barras horizontales o verticales.</td>
</tr>
<tr>
<td>Diagrama de anillo</td>
<td>Dos o varios conjuntos de datos en una representación circular.</td>
</tr>
<tr>
<td>Valor</td>
<td>El valor original de uno o varios conjuntos de datos.</td>
</tr>
<tr>
<td>Indicador</td>
<td>El valor de un conjunto de datos que se muestra en forma de medidor. Utilice el menú Valores para establecer opcionalmente umbrales de medidor para los rangos de datos lower, middle y upper.  </td>
</tr>
<tr>
<td>Propiedades de dispositivo</td>
<td>Propiedades específicas de uno o más dispositivos.</td>
</tr>
<tr>
<td>Todas las propiedades de dispositivo</td>
<td>Todas las propiedades específicas de uno o más dispositivos.</td>
</tr>
<tr>
<td>Lista de dispositivos</td>
<td>Una lista para supervisar varios dispositivos. Se puede utilizar una lista como un origen de datos para otras tarjetas. </br>Puede filtrar la lista por tipo e ID de dispositivo en los valores de la tarjeta. Las listas de dispositivo de tamaño L o mayores también se pueden filtrar de forma interactiva pulsando el icono de filtro en la tarjeta. Las entradas de filtro se pueden añadir como entradas individuales, rangos (x-y) o separadas por comas.</br> De forma predeterminada, una lista visualiza ID y tipos de dispositivo. Es posible configurar los valores de la tarjeta de lista para que también visualicen otros metadatos de dispositivo.  </td>
</tr>
<tr>
<td>Información de dispositivo</td>
<td>Información básica para un dispositivo individual.</td>
<tr>
<td>Correlación de dispositivo</td>
<td>Ubicación de los dispositivos en la lista de dispositivos.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Análisis</dt>
<dd>
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Datos que se visualizan</th>
</tr>
</thead>
<tbody>
<tr>
<td>Reglas</td>
<td>Una lista de las reglas que tienen alertas.</td>
</tr>
<tr>
<td>Alertas de reglas</td>
<td>Una lista de alertas para un dispositivo.</td>
</tr>
<tr>
<td>Información de alertas</td>
<td>Información básica para una alerta individual.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Gestión de riesgos (Beta)</dt>
<dd>Disponible únicamente con organizaciones con [seguridad avanzada](reference/security/RM_security.html).
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Datos que se visualizan</th>
</tr>
</thead>
<tbody>
<tr>
<td>Conformidad de política</td>
<td>Una visión general de la seguridad de los dispositivos y de los dispositivos en listas blancas y listas negras.</td>
</tr>
<tr>
<td>Conformidad de lista blanca/lista negra</td>
<td>Número de dispositivos en listas blancas y listas negras.</td>
</tr>
<tr>
<td>Seguridad de conexión</td>
<td>Número de dispositivo que no han pasado la comprobación de seguridad de conexión.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Uso</dt>
<dd>
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Datos que se visualizan</th>
</tr>
</thead>
<tbody>
<tr>
<td>Tipos de dispositivos</td>
<td>Gráfico circular que visualiza el número de dispositivos registrados por tipo de dispositivo en su organización.</td>
</tr><tr>
<td>Datos transferidos</td>
<td>Las estadísticas de uso para los datos transferidos para su organización.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Básico</dt>
<dd>
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Datos que se visualizan</th>
</tr>
</thead>
<tbody>
<tr>
<td>Separador</td>
<td>Un separador horizontal para estructurar y agrupar tarjetas en el panel.</td>
</tr>
</tbody>
</table>
</dd>
</dl>

4.	Seleccione los datos de origen de la tarjeta.  
Seleccione uno o varios orígenes de datos de tarjeta y, a continuación, pulse **Siguiente**.  
Los orígenes de datos pueden ser dispositivos registrados individuales u otras tarjetas. Para utilizar un origen de datos de tarjeta, en el panel debe existir una lista o correlación de tarjetas.  
5. Añada uno o más conjuntos de datos para cada uno de sus orígenes de datos.
 - Dispositivos
    2. Seleccione un suceso que incluya el punto de datos que desea visualizar.
    3.	Seleccione la propiedad que representa el punto de datos.
    1.	Otorgue al conjunto de datos un nombre de identificación.
    4.	Establezca el tipo, la unidad, la precisión y los valores mínimo y máximo para el punto de datos.  
    Cuando haya terminado, pulse **Nuevo conjunto de datos** para añadir más conjuntos de datos o pulse **Siguiente**.
 - Listas
    2. Seleccione un tipo de dispositivo o seleccione **Cualquier tipo de dispositivo**.
    2. Seleccione un suceso que incluya el punto de datos que desea visualizar.
    3.	Seleccione la propiedad que representa el punto de datos.
    1.	Otorgue al conjunto de datos un nombre de identificación.
    4.	Establezca el tipo, la unidad, la precisión y los valores mínimo y máximo para el punto de datos.  
    Cuando haya terminado, pulse **Nuevo conjunto de datos** para añadir más conjuntos de datos o pulse **Siguiente**.
5.	Personalice la visualización de la tarjeta en la vista previa de tarjeta.  
 7. Seleccione el tamaño de presentación.  
Además de establecer el tamaño de la tarjeta en su panel, el valor del tamaño de la tarjeta también controla otros aspectos de la presentación como, por ejemplo, el número de dispositivos que se listan, los metadatos gráficos a visualizar, etc.   
**Sugerencia: ** Pulse diferentes etiquetas de tamaño para obtener vistas previas de las tarjetas con distintos tamaños.
 8. Configure cualquier otro valor adicional.  
Si la tarjeta lo soporta, pulse **Valores** para ver valores adicionales que puede configurar como, por ejemplo, rangos de datos para tarjetas de tipo de indicador y opciones de filtrado para tarjetas de lista de dispositivos.
6. Actualice la información de la tarjeta.  
 1. Proporcione un título y una descripción para la tarjeta y, opcionalmente, seleccione un esquema de color.   
 2. Pulse **Enviar** para crear la tarjeta.
7.	Coloque la nueva tarjeta en el panel arrastrándolo en la ubicación que desee.  
