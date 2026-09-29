# Guía de diseño de objetos — Tienda (Herrería) de Carne de Titanes

Documento de referencia para pegar como contexto en un chat de Claude dedicado a diseñar objetos de tienda. Resume el criterio ya usado en la campaña para que cualquier objeto nuevo sea coherente con lo existente.

---

## 1. Qué es esta tienda y qué NO es

La Herrería del portal es el catálogo de compra normal, con un tope de **1.000 PO por objeto**. Solo vende objetos de rareza **Común** e **Infrecuente**.

Los objetos **Raro** o **Muy raro** (como la corona del nigromante, el frasco del brujo o los guantes del monje que ya diseñamos) **no van en esta tienda** — son recompensas narrativas: loot de misión, regalos de PNJ, hallazgos de Loot de Caza o drops de Coloso. Si en el chat de "Tienda" te piden un objeto raro o superior, es una señal de que en realidad es para una misión, no para el catálogo — vale la pena señalarlo antes de diseñarlo como si fuera de venta libre.

---

## 2. Filosofía de diseño (por rareza)

**Común — un solo efecto simple.**
Nada de combos, nada de "1/DL activás X y además Y". Un bonus estático, una ventaja en un tipo de tirada concreto, o una utilidad menor sin coste de acción. Debe poder resumirse en una frase.

**Infrecuente — efecto condicional, situacional, o combo de dos cosas simples.**
Aquí ya entra: "1/descanso largo, cuando pasa X, ocurre Y"; algo que impacta la economía de acciones (dar una acción adicional gratis, una reacción nueva); o dos bonus estáticos menores combinados. El objeto debería generar una decisión táctica, no ser solo "mejor número".

**Principio transversal (aplica también a los objetos raros que no van en esta tienda):** favorecer mecánicas con sabor narrativo y contrapartida riesgo-recompensa real por encima de efectos puramente seguros o cosméticos. Un objeto Infrecuente que castiga un mal uso (ej. "empuja al enemigo pero gastás tu reacción") es mejor diseño que uno que solo suma número sin coste.

---

## 3. Rangos de precio (PO — sistema BG3, sin conversión)

| Rareza | Rango PO | Criterio |
|---|---|---|
| Común | 250–400 | Un efecto simple. Ej: +1 a impacto, +1 a daño, ventaja en un tipo de tirada concreto. |
| Infrecuente | 450–750 | Efecto condicional/situacional, combo de dos efectos simples, o impacto en la economía de acciones (acción adicional, reacción nueva). |

Dentro de cada banda, subí el precio si el efecto:
- Es utilizable **todos los turnos** sin restricción (vs. 1/descanso corto o largo).
- Da **ventaja mecánica en combate** (vs. solo utilidad de exploración/social).
- No tiene **ningún coste ni riesgo** para el usuario.

Bajalo si el efecto:
- Es puramente narrativo/exploración sin peso en combate.
- Tiene un coste real (gasta reacción, acción adicional, o tiene un fallo posible).

*(Fuera de esta tienda: Raro ronda 3.000–5.000 PO y Muy raro 18.000–20.000 PO, para cuando diseñes recompensas de misión en otro chat — no las uses acá.)*

---

## 4. Slots y categorías del catálogo

- **Armas:** Armas Ligeras, Armas Pesadas, Armas a Distancia, Armas a Dos Manos, Varitas
- **Armadura de cuerpo:** Ligera, Intermedia, Pesada
- **Accesorios de armadura:** Casco, Guantes, Botas
- **Otros:** Capas, Amuleto (1 slot), Anillos (2 slots)

Portal objetivo: **6 comunes + 4 infrecuentes por categoría**.

---

## 5. Plantilla de código (formato ya usado en el portal)

Cada categoría del JS pide campos distintos — este es el motivo más común de que salga `UNDEFINED` en las cards, así que respetá el shape exacto:

```js
// ARMAS — necesita 'type' (tipo de arma) y 'dice' (dado + característica)
{name:'', type:'', dice:'', rarity:'comun'|'azul', price:0, desc:''},

// ARMADURA DE CUERPO — necesita 'slot':'Armadura', 'baseAC', 'weight'
{name:'', slot:'Armadura', baseAC:'', weight:'Ligera'|'Intermedia'|'Pesada', rarity:'comun'|'azul', price:0, desc:''},

// CASCO / GUANTES / BOTAS — usa 'slot' (no 'type')
{name:'', slot:'Casco'|'Guantes'|'Botas', rarity:'comun'|'azul', price:0, desc:''},

// CAPAS / AMULETO / ANILLOS (van en el array `accessories`) — necesita 'type' (no 'slot')
{name:'', type:'Capa'|'Amuleto'|'Anillo', rarity:'comun'|'azul', price:0, desc:''},
```

`rarityLabel` usa `comun` → Común y `azul` → Infrecuente. No uses `morado`/`amarillo`/`blanco` en esta tienda — esos son para objetos raros que no venden acá.

**Checklist antes de pasar un objeto nuevo a código:**
1. ¿La descripción es una sola frase clara, sin ambigüedad de qué tirada o CD usa?
2. ¿El precio está dentro de la banda de su rareza?
3. ¿Uso el campo correcto según la categoría (`type`/`dice` para armas, `slot`+`baseAC`+`weight` para armadura de cuerpo, `slot` para casco/guantes/botas, `type` para capas/amuleto/anillos)?
4. Si es Infrecuente, ¿tiene alguna condición, límite de uso (1/DL, 1/DC) o coste — no es solo un número más grande que el común?
