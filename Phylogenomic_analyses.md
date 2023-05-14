## ANÁLISIS FILOGENÓMICO DE PROTEOMA DE 23 SPP DE VERTEBRADOS

### Resumen del workflow de obtención de árboles de coalescencia y de máxima verosimilitud a partir de proteomas de especies de vertebrados:

En un inicio, claro esta lo principal es obtener los datos potencialmente analizables por filogenómica (en este caso correspondientes a proteomas de 23 spp de vertebrados:elephant:). Luego, al ya tener nuestras secuencias en una carpeta de trabajo, activamos el ambiente `conda` :gear: :desert:. Lo primero es iniciar con la inferencia de ortólogos, nuestro objetivo es rastrear regiones ortólogas entre proteínas. Para ello empleamos el software __Orthofinder__ con el comando `orthofinder` especificandole la carpeta :card_index_dividers: con los respectivos proteomas :dna:, nos generará de esta forma un archivo de secuencia para cada uno en formato __.tsv__. Por último y para facilitar la concatenación, empleamos expresiones regulares para cambiar la estructura de los nombres de las secuencias a `Género_especie` lo más conveniente es usar `sed`. Después, es importante hacer un filtraje de calidad🔎 con `PREQUAL` para descartar todos los posibles errores de ensamblaje o anotación. Con ello, para corroborar homología entre aa realizamos un __alineamiento múltiple__ por `mafft`. Además, removemos:scissors: posibles gaps generados con `BMGE`:wastebasket:. Por útlimo, realizamos un alinamiento cancatenando:abacus: por medio de __FASconCAT__ y `perl`, para así obtener 23 taxa con 21 genes. Con esa información generamos dos árboles por máxima verosimilitud: 1. un solo locus que evoluciona de la misma forma:round_pushpin: y 2. por partición según ubicación de genes, usando `IQTREE`:bar_chart:. Así como un árbol de coalescencia usando `ASTRAL`:comet:.

#### 1) Árbol de máxima verosimilitud (no segmentado):
![species_tree_ASTRAL tre](https://github.com/StivennGutierrez/parcial_bioinformatica/assets/128840301/71fc62e0-757c-4c65-9113-a80960382e69)

<div class="alert alert-info">

This is a blue chart note.

</div>


