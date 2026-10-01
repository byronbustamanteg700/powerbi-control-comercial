# Medidas DAX

14 medidas, en el orden en que se crean: las que usan `[corchetes]` dependen de las anteriores.

## En `fact_ventas` (9)

```dax
Venta Neta = SUM(fact_ventas[VentaNeta])
```
```dax
Venta Bruta = SUM(fact_ventas[VentaBruta])
```
```dax
Costo = SUM(fact_ventas[CostoTotal])
```
```dax
Margen Bruto = [Venta Neta] - [Costo]
```
```dax
% Margen = DIVIDE([Margen Bruto], [Venta Neta])
```
```dax
Descuento Otorgado = [Venta Bruta] - [Venta Neta]
```
```dax
% Descuento = DIVIDE([Descuento Otorgado], [Venta Bruta])
```
```dax
Margen Potencial = [Venta Bruta] - [Costo]
```
```dax
% Margen Cedido = DIVIDE([Descuento Otorgado], [Margen Potencial])
```

## En `fact_cartera` (5)

```dax
Cartera Pendiente = SUM(fact_cartera[SaldoPendiente])
```
```dax
Cartera Vencida = CALCULATE([Cartera Pendiente], fact_cartera[DiasVencido] > 0)
```
```dax
% Cartera Vencida = DIVIDE([Cartera Vencida], [Cartera Pendiente])
```
```dax
Cartera Mas de 60 = CALCULATE([Cartera Pendiente], fact_cartera[DiasVencido] > 60)
```
```dax
% Cartera Mas de 60 = DIVIDE([Cartera Mas de 60], [Cartera Pendiente])
```

## Formato

- Las cinco medidas de porcentaje: formato **Porcentaje** con 2 decimales.
- Las nueve de dinero: separador de miles.

## Notas de diseño

**`% Margen Cedido`** responde: de todo el margen que se habría obtenido a precio de lista, ¿qué proporción se entregó negociando? Como el costo no baja cuando se descuenta, cada dólar de descuento sale íntegro del margen.

**`DIVIDE` en vez de `/`**: si el denominador es cero devuelve vacío en lugar de un error, y el visual no se rompe.

**`CALCULATE`**: cambia el contexto de filtro en el que se evalúa una expresión. `Cartera Vencida` recalcula el saldo pendiente solo donde los días vencidos son mayores que cero.

**Medidas compuestas**: `% Margen Cedido` encadena cuatro medidas. Cada cálculo se escribe una vez y se reutiliza en todo el informe.
