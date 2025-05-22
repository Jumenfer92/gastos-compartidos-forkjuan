[toc]

# Especificación funcional: Gestor de gastos compartidos

## 1. Objetivo del proyecto

**Creación de un gestor de gastos compartidos ligero y sencillo**, que permita dicha gestión entre varios participantes y para una actividad en concreto. El objetivo final es **obtener las cantidades exactas que deberán facilitar** los participantes con saldo negativo (deudores) a aquellos con saldo positivo (los que adelantaron el dinero). **En este proyecto primará la funcionalidad, eficacia, eficiencia y experiencia de usuario frente a otros factores como la estética.**



## 2. Alcance

Esta versión inicial del proyecto estará compuesta por una única vista y toda su funcionalidad se desarrollará exclusivamente en el entorno cliente (frontend), funcionando íntegramente en el navegador. No se implementará todavía ninguna característica relacionada con el backend, de lógica de negocio o la persistencia de datos.



## 3. Requisitos funcionales

- **RF1**: El sistema debe permitir al usuario añadir participantes por su nombre. Debe prevenir duplicidades en nombres de participantes.

- **RF2:** El usuario podrá ver la lista de participantes añadidos actualizada en tiempo real.

- **RF3**: El sistema debe permitir al usuario eliminar un participante existente. Se solicitará confirmación.

  - Si el participante no tiene gastos asociados, se eliminará directamente.

  - Si tiene gastos como pagador o beneficiario, se ofrecerá la opción de:
    1. **Eliminar** todos los gastos en los que participa.
    2. **Reasignar** sus gastos a otro participante de la lista.
    3. **Cancelar** la eliminación.


- **RF4**: El sistema debe permitir al usuario registrar un gasto indicando el concepto, el importe, la persona que lo pagó, quién ha disfrutado ese gasto y en qué proporción en %, fracción (a/b), o cantidad (€). En caso de no indicar nada para uno o varios participantes, se repartirán a partes iguales la restante proporción.

- **RF5**: El usuario podrá ver la lista de gastos registrados y todos sus detalles.

- **RF6**: El sistema debe permitir al usuario editar cualquier detalle de un gasto registrado.

- **RF7**: El sistema debe permitir al usuario eliminar un gasto registrado. Se solicitará confirmación.

- **RF8**: El sistema debe permitir calcular automáticamente y mostrar:

  - Un resumen de cuentas.

    - Se listará cada participante con su total pagado, total disfrutado y el saldo resultante.

    - El saldo se mostrará con formato positivo (a favor) o negativo (en contra).

    - El usuario podrá volver a calcular el resumen en cualquier momento tras modificar los datos.


  - Los pagos recomendados para que el número de transacciones sea mínimo. Se mostrará:
    - el nombre de los participantes que harán el/los pago/s.
    - cantidades.
    - nombre de los perceptores.


- **RF9**: El usuario podrá exportar un informe en formato PDF con toda la información registrada.
   El PDF incluirá:

  - Fecha y hora


  - Relación de gastos: concepto, importe, pagador y proporciones de disfrute.

  - Resumen de cuentas: participante, total pagado, total disfrutado y saldo, con color verde para saldos positivos y rojo para negativos.

  - Pagos recomendados: listado de transferencias sugeridas para saldar las cuentas.


- **RF10**: El usuario podrá vaciar todos los datos sin necesidad de reiniciar la aplicación



## 4. Requisitos no funcionales

- **RNF1**: La interfaz debe ser intuitiva y usable sin necesidad de formación previa.

- **RNF2:** El sistema debe evitar malentendidos, será claro en visualización y fiable.

- **RNF3**: El sistema debe prevenir errores de usuario como la introducción de porcentajes inválidos o vacíos.

- **RNF4**: El sistema debe funcionar sin conexión a Internet, ya que es completamente en frontend.
- **RNF5**: La aplicación debe ser ligera y ejecutable en navegadores modernos sin instalaciones adicionales.
- **RNF6**: Debe prevenir duplicidades en nombres de participantes.



## 5. Flujo básico de uso

- Añadir a participantes
- Opcional: eliminar participantes
- Registro de gastos
- Opcional: editar o eliminar gastos registrados
- Cálculo y visualización del resumen de cuentas y pagos recomendados
- Opcional: exportación a pdf.
- Opcional: vaciado de datos para empezar de nuevo 



## 6. Elementos de la interfaz (sin diseño visual)

### Sección de Participantes

- Campo de entrada de tipo texto para nombre de participante y botón "añadir". 

- Lista visible de participantes: tabla con 2 columnas: participante  y eliminar.

  - Si el participante tiene gastos, al hacer clic en eliminar:

    "Este participante tiene gastos asociados. ¿Qué deseas hacer?" (modal)

    - 🔁 Reasignar sus gastos a otro participante (desplegable seleccionable + botón)

    - 🗑️ Eliminar también todos sus gastos (botón)

    - ❌ Cancelar (botón)

### Sección de gastos

- Botón "añadir gasto" muestra formulario con:

  - Concepto - campo de texto

  - Importe - campo de texto

  - Pagador - desplegable

  - Checkbox "avanzado" muestra Disfrutado - campo de texto por cada participante + desplegable:

    | Opción            | Significado lógico                                           | Acción visual               |
    | ----------------- | ------------------------------------------------------------ | --------------------------- |
    | **Repartir**      | Reparto equitativo del resto entre quienes lo tengan. **POR DEFECTO** | Campo de texto se desactiva |
    | **No disfrutado** | No participa o disfruta del gasto                            | Campo de texto se desactiva |
    | **%**             | Interpreta valor como porcentaje                             | Campo de texto habilitado   |
    | **a/b**           | Interpreta valor como fracción                               | Campo de texto habilitado   |
    | **€**             | Interpreta valor como cantidad en euros                      | Campo de texto habilitado   |

  - Botón guardar (registra gasto y oculta el formulario)

  - Botón cancelar (cancela y oculta el formulario)

- Lista visible de gastos - tabla con 6 columnas: concepto, importe, pagador, disfrutado, botón editar, botón eliminar.

### Sección Resumen

- Botón "Calcular" muestra RF8
- Botón "Exportar a PDF"



## 7. Consideraciones de usabilidad y accesibilidad

- Se usarán etiquetas claras y contrastes adecuados para todos los elementos.
- El uso del teclado será suficiente para navegar por toda la aplicación.
- Se priorizarán mensajes de error y confirmación accesibles y descriptivos.



 ## Apéndice A: Validaciones de entrada

### Nombre del participante

- No debe estar vacío.

- No debe repetirse.

### Concepto del gasto

- No debe estar vacío.

### Importe del gasto

- Debe ser un número **real positivo** > 0.
- No se permiten valores negativos ni texto.
- Se aceptan coma o punto como separador decimal.
- Ejemplos válidos: `25`, `100`, `33`, `45,5`, `50.2`
- Ejemplos inválidos: `abc`, `-10.50`,  `0`

### Disfrutado

#### Porcentaje (%)

- Solo números **enteros positivos** sin decimales entre 0 y 100.
- Ejemplos válidos: `25`, `100`, `33`
- Ejemplos inválidos: `abc`, `-10`, `105`, `45,5`, `50.2`

#### Fracción (a/b)
- Dos enteros separados por `/` sin espacios.
- El denominador no puede ser cero.
- Ejemplos válidos: `1/2`, `3/4`, `7/10`
- Ejemplos inválidos: `1/0`, `1 / 2`, `a/b`

#### Cantidad (€)
- Número real positivo.
- Se aceptan coma o punto como separador decimal.
- Ejemplos válidos: `12`, `45.75`, `0`, `45,75`
- Ejemplos inválidos: `-3`, `abc`

### Exportación a pdf

- Solo se permitirá si existen gastos registrados.

- Si no, mostrar mensaje tipo: *"No hay datos para exportar."*