# Simulador PTE modelo 27: planta termoeléctrica de 600 MW de Usoro (1977)

Simulador en línea del modelo de 27 estados («Standard Model») de la caldera-turbina
de la unidad termoeléctrica de 600 MW de Usoro (1977), con sus lazos de control PI
originales. Corre por completo en el navegador: no necesita MATLAB ni instalar nada.

**Abrir el simulador:** https://marioabdel01-arch.github.io/simulador-pte-modelo-27/

**Guía en video** (unos 7 minutos, narrada y con subtítulos): botón «▶ Guía en video» de la página, o
https://marioabdel01-arch.github.io/simulador-pte-modelo-27/#video

**Modelo de 47 estados:** el simulador con la misma interfaz para el modelo de 47 estados,
con el controlador auto-sintonizable de Amin et al. (1984), está en
https://marioabdel01-arch.github.io/simulador-pte-modelo-47/

## Qué simula

- **Planta:** el modelo no lineal de 27 estados de Usoro, traducido del programa en
  MATLAB de Quintero-Mármol (2019) e integrado con Runge–Kutta de cuarto orden y paso
  de 0,1 s.
- **Control:** los lazos PI originales del modelo: demanda maestra de caldera, aire,
  combustible con límite cruzado, recirculación de gases, temperatura del recalentado
  (inclinación de quemadores y atemperación), temperatura del sobrecalentado
  (atemperación) y turbina.
- **Variables graficadas:** la temperatura del vapor sobrecalentado (TSSO), la del
  recalentado (TRHO) y la presión del vapor (PSSO), con sus consignas, y los mandos de
  demanda maestra de caldera (CBMD), inclinación de quemadores (CXGG) y atemperación
  del sobrecalentador (CWSY).
- **Pruebas:** las cuatro rampas de carga de Usoro (1977, pruebas V.1 a V.4): de 100 a
  77,5 %, de 77,5 a 50 %, de 50 a 77,5 % y de 77,5 a 100 % a 15 %/min desde t = 10 s,
  cada una con su estado inicial, con duración de 700 s (la de la tesis) o 1400 s.
- **Índices:** IAE e ITAE de cada salida respecto de su consigna.
- **Exportar:** al pausar o terminar una corrida se descargan un informe en PDF, los
  datos en CSV (cada 1 s; la configuración y los índices van en el encabezado, en líneas
  que empiezan con `#`) y las seis gráficas en PNG. En MATLAB:
  `T = readtable('archivo.csv', 'CommentStyle', '#');`
- **Pestaña «Planta»:** el esquema de la unidad con los valores del modelo en el
  instante actual y un panel de gráficas por área (caldera, sobrecalentador,
  recalentador y turbina) con las 16 variables que registra el programa del modelo. El
  modelo de 27 estados no incluye bombas, ventiladores, condensador ni deareador.

## Verificación

El programa en MATLAB del modelo avanza cada paso de 0,1 s con la derivada fija al
inicio del paso (llama a `ode45` sobre una derivada constante), lo que equivale al
método de Euler. Con ese mismo integrador, las 16 variables registradas en las cuatro
pruebas de Usoro coinciden con MATLAB con un error relativo menor a 2·10⁻¹¹.

La página integra con Runge–Kutta de cuarto orden: con Euler y paso de 0,1 s, la
oscilación entre la velocidad de la turbina y el ángulo del generador crece poco a poco
y, a carga constante, la planta pierde el sincronismo hacia los 4000 s. Con Runge–Kutta
se mantiene estable y coincide con Euler a paso de 0,01 s. En las pruebas de 700 s la
diferencia contra MATLAB es de 0,2 MW, 0,6 psia y 0,12 °R como máximo.

```
node verificacion/verifica.js
```

## Estructura

| Archivo | Contenido |
|---|---|
| `index.html`, `css/`, `js/app.js` | interfaz |
| `guia_simulador.mp4` | guía en video del simulador |
| `js/digpte27.js` | modelo de 27 estados de Usoro (traducido de MATLAB) |
| `js/sim.js` | integrador, rampas de carga, registro e índices |
| `js/datos.js` | parámetros de la planta y estados iniciales de las pruebas |
| `img/esquema_planta.svg` | esquema de la planta (vectorial) |
| `verificacion/` | corridas de referencia del programa en MATLAB y el script que las compara |

## Insertarlo en otra página

```html
<iframe src="https://marioabdel01-arch.github.io/simulador-pte-modelo-27/"
        width="100%" height="1100" style="border:0"></iframe>
```

## Licencia

[MIT](LICENSE): se puede usar, copiar y modificar libremente, siempre que se conserve
el aviso de autoría. Si lo usa en un trabajo académico, cite el simulador.

## Créditos

- Usoro, P. B. (1977). *Modeling and simulation of a drum boiler-turbine power plant
  under emergency state control* [Tesis de maestría, MIT].
- Quintero-Mármol Márquez, E. (2019). *Programa en MATLAB del modelo estándar de 27
  estados de Usoro* [Código]. TecNM/CENIDET.
- Quintero-Mármol Márquez, E. (2025). *Plantas termoeléctricas: análisis dinámico,
  modelado, simulación y control*. TecNM/CENIDET. (Esquema de la planta, redibujado en
  formato vectorial.)

Simulador: Ing. Mario Abdel Guerra Pérez y Dr. Enrique Quintero-Mármol Márquez,
TecNM/CENIDET, Departamento de Ingeniería Electrónica.

El PDF se genera con [jsPDF](https://github.com/parallax/jsPDF) 2.5.1 (licencia MIT),
incluido en `js/jspdf.umd.min.js`.
