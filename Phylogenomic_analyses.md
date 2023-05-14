## ANÁLISIS FILOGENÓMICO DE PROTEOMA DE 23 SPP DE VERTEBRADOS

### Resumen del workflow de obtención de árboles de coalescencia y de máxima verosimilitud a partir de proteomas de especies de vertebrados:

En un inicio, claro esta lo principal es obtener los datos potencialmente analizables por filogenómica (en este caso correspondientes a proteomas de 23 spp de vertebrados:elephant:). Luego, al ya tener nuestras secuencias en una carpeta de trabajo, activamos el ambiente `conda` :gear: :desert:. Lo primero es iniciar con la inferencia de ortólogos, nuestro objetivo es rastrear regiones ortólogas entre proteínas. Para ello empleamos el software __Orthofinder__ con el comando `orthofinder` especificandole la carpeta :card_index_dividers: con los respectivos proteomas :dna:, nos generará de esta forma un archivo de secuencia para cada uno en formato __.tsv__. Por último y para facilitar la concatenación, empleamos expresiones regulares para cambiar la estructura de los nombres de las secuencias a `Género_especie` lo más conveniente es usar `sed`. Después, es importante hacer un filtraje de calidad🔎 con `PREQUAL` para descartar todos los posibles errores de ensamblaje o anotación. Con ello, para corroborar homología entre aa realizamos un __alineamiento múltiple__ por `mafft`. Además, removemos:scissors: posibles gaps generados con `BMGE`:wastebasket:. Por útlimo, realizamos un alinamiento cancatenando:abacus: por medio de __FASconCAT__ y `perl`, para así obtener 23 taxa con 21 genes. Con esa información generamos dos árboles por máxima verosimilitud: 1. un solo locus que evoluciona de la misma forma:round_pushpin: y 2. por partición según ubicación de genes, usando `IQTREE`:bar_chart:. Así como un árbol de coalescencia usando `ASTRAL`:comet:.

#### 1) Árbol de máxima verosimilitud (no segmentado):
![unpartitioned treefile](https://github.com/StivennGutierrez/parcial_bioinformatica/assets/128840301/65473ddb-c357-4b59-a851-ab12b233e02a)

----

### Resultado:
En general, el árbol señala la formación de distintos clados como el que incluye las especies *Oreochromis niloticus*:tropical_fish:, *Takifugu rubripes*:fish:, *Danio rerio*:fried_shrimp: y *Lepisosteus oculatus*:shrimp:, donde la especie *T. rubripes* señala mayor acumulación de cambios evolutivos a tráves del tiempo :hourglass_flowing_sand:. Cabe resaltar que este grupo monofilético muestra apariciones y cambios evolutivos más lejanos entre cada especie.

Asimismo, es posible identificar otros clados con apariciones y cambios evolutivos no muy lejanos entre sí: el primero, conformado por las especies *Pelodiscus sinensis*:turtle:, *Anolis carolinensis*:lizard:, *Taeniopygia guttata*:bird:, *Meleagris gallopavo*:turkey: y *Gallus gallus*	:rooster:; el segundo, conformado por *Homo sapiens*:tipping_hand_person:, *Mus musculus*:mouse2:, *Canis lupus familiaris*:dog2:, *Dasypus novemcintus*:hedgehog: y *Loxodonta africana*:elephant:, siendo este último grupo monofilético más recientes.

Pero si nos fijamos bien, podemos observar que las especies *Neoceratodus forsteri*:penguin:, *Protopterus annectens*:shrimp: y *Lepidosiren paradoxa*:shrimp: se separan de los clados anteriores un tiempo evolutivo considerable, siendo *N. forsteri* la especie más reciente :new:. Además, la especie *Callorhinchus milli*:shark: sería la especie que menos carcaterísticas compartiría con el resto de clados, sin embargo debe presentar una carcaterística que lo separó del primer clado mencionado, y que a lo largo del tiempo ha permanecido en un constante cambio evolutivo:electron:.

---

#### 2) Árbol de máxima verosimilitud (segmentado):
![partitioned treefile](https://github.com/StivennGutierrez/parcial_bioinformatica/assets/128840301/96ca351e-4e38-476b-972a-caff3a851152)



#### 3) Árbol de coalescencia:
![species_tree_ASTRAL tre](https://github.com/StivennGutierrez/parcial_bioinformatica/assets/128840301/edc4989b-ca1f-4ee0-8424-d5ca3af2c282)

#### Diferencias entre los tres tipos de árboles:
| **Item** | **January** | **February** |
| --- | --- | --- |
| Apples | 5 | 7 |
| Oranges | 7 | 9 |
| Bananas | 2 | 3 |

















