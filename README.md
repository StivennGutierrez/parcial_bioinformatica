Primer punto:

1)	En la carpeta ‘Archivos_PARCIAL’ dejé el archivo FASTA con las secuencias del popset:
	
En NCBI, filtre por Popset y busqué el código ‘1776322181’, ya en el poset: send to>File>FASTA>download

2)	Para cambiar el nombre de cada secuencia a la estructura Código de la secuencia_Nombre del gen_Género_Epíteto específico:

En Atom:

Find all: (>\w+\.\w+)\s(\w+)\s(\w+).*

Replace all: $1_COI_$2_$3

Y llamé al archivo: Spodoptera_sequences.fasta

3)	Luego, al ya tener listo el archivo, lo subí a una carpeta que cree en el clúster ‘Blast’:
4)	
Para ello me ubiqué en la carpeta de la llave y corrí el siguiente código teniendo en cuenta la ruta en el clúster correspondiente a la carpeta ‘Blast’:

► scp -i bio.pt.pem -P 37022 /mnt/c/Users/stive/Downloads/Archivos_PARCIAL/Spodoptera_sequences.fasta bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_StivennGutierrez/Blast

4)	De esta forma, ya en el clúster en la carpeta ‘Blast’, descargue la secuencia query:
► wget “https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/query_parcial.fasta”

5)	Ya con ambos archivos en la carpeta, antes de generar el BLAST, primero construí la base de datos tipo Blastn a partir de ambos archivos mediante:

► salloc

► module load blast/2.7.1

► makeblastdb -in spidoptera_sequences.fasta -dbtype nucl -parse_seqids -out trans_spidoptera -title "Spidoptera_transcriptome"

6)	Con la base de secuencias tipo Blastn, ahora si corrí el BLAST:

► blastn -query query_parciale.fasta -task megablast -db trans_spidoptera -outfmt 7 -word_size 7 -out blast_spidoptera -num_threads 1

*Análisis de BLAST:

¿Cuál es la identidad más probable de la secuencia?

La especie Spodoptera cilium con un % de identidad igual a 96.9.

¿Basado en qué tomó esta decisión? 

Teniendo en cuenta también el valor de E-value, donde se observa que es 0, es decir el ruido de fondo por aleatoriedad es bajo y por ende la coincidencia es mayor. Así como el valor del Bit-score que indica una alineación de buena calidad con un valor de 1151 para esta especie.

¿Es un gen mitocondrial o nuclear? Mitocondrial

Por último, cabe destacar que todas las secuencias para esta especie ranquearon un puntaje de mayor coincidencia e identidad para la secuencia query en comparación al resto.


Segundo punto:

Para realizar el alineamiento con las secuencias previamente utilizadas, las copie a una nueva carpeta ‘Alignment’:

► cp Spodoptera_sequences.fasta /home/bio.pt/data/Parcial1/parcial1_StivennGutierrez/Alignment

► cp query_parcial.fasta /home/bio.pt/data/Parcial1/parcial1_StivennGutierrez/Alignment

Ya en la carpeta con ambos archivos, los concatene y los guarde en un nuevo FASTA:

► cat Spodoptera_sequences.fasta query_parcial.fasta > ALlsequences.FASTA

Luego, al ya tener todas las secuencias en un mismo FASTA, realicé un alineamiento por MUSCLE:

Para ello:

► Salloc

► module load muscle/3.8.31

► muscle -in ALlsequences.FASTA -out allsequences_spidoptera_muscle.fast

Tras obtener el alineamiento por MUSCLE, y con el propósito de generar un árbol de máxima verosimilitud por iqtree:

Primero, creé una carpeta ‘fasconcat’ donde copié el archivo “FASconCAT-G_v1.05.1.pl”:

► cp FASconCAT-G_v1.05.1.pl /home/bio.pt/data/Parcial1/parcial1_StivennGutierrez/Alignment/fasconcat

Y moví el archivo correspondiente al alineamiento por MUSCLE:

► mv allsequences_spidoptera_muscle.fast fasconcat

Con ambos archivos dentro de la carpeta de fasconcat, cargue FASCONCAT:

► ./FASconCAT-G_v1.05.1.pl

En la interfaz, cambie los parámetros Nexus para la opción blocked (n), para Phylip (2p) para la opción relaxed y luego ejecuté (s).
De esta forma, ya obtuve la matriz necesaria para generar el árbol de máxima verosimilitud. Para generar el árbol por iqtree:

► module load iqtree

► iqtree -s FcC_supermatrix.phy -m TEST -bb 1000 -pre spidoptera_muscle

Ya con el archivo generado con los valores de soporte, abrí el archivo para copiar toda la información para la construcción del árbol filogenético con [cat].

En phylo.io renderice la información que copie anteriormente y aplique el parámetro ‘Branch Labels/Support’.

Adicionalmente, para enraizar el árbol obtenido me ubiqué en la rama central y le dí a la opción ‘reroot’, de esta forma finalmente obtuve el árbol correspondiente.

*Análisis de árbol de máxima verosimilitud:
![Arbol](https://user-images.githubusercontent.com/128840301/232170859-ac7b8724-ad6d-4a25-ac47-69739c95108e.png)

Se observá que la especie Spodoptera cilium está más estrechamente relacionada con la secuencia query, aunque exhiba un valor de boostrap inconsistente en comparación a las otras especies. Por otro lado, atendiendo a las relaciones filogenéticas inferidaas, las especies Mythimna loreyi y Peridroma saucia son bastante cercanas entre sí y el grupo más reciente dentro de la historia evolutiva del árbol, asimismo presentan unvalor de soporte de rama de mayor confianza con tiempo evolutivos muy similares en cuanto a diversificación de su ancestro en común. Y la especie que se consideraría menos reciente corresponde a Spodoptera exigua.

Tercer punto: 
1)	En la carpeta ‘Archivos_PARCIAL’ descargué el siguiente archivo:

► wget “https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/archivo1.csv”

2)	Para obtener el número de filas y de caracteres:

► wc archivo1.csv

De esta forma, el # de filas son 49 y el # de caracteres 908

3)	Para contar el número de columnas:

► awk -F "," '{print NF; exit}' archivo1.csv

De esta forma, el # de columnas son 4

4)	Para modificar “archivo1.csv” cambiando ‘chr’ por ‘Cromosoma’ y guardarlo en un nuevo archivo:

► sed 's/chr/Cromosoma/' archivo1.csv | head -n 49 > result_file1.bed

5)	Para modificar “archivo1.csv” reemplazando ‘,’ por ‘\t’ y guardarlo en un nuevo archivo:

► sed 's/,/\t/g' result_file1.bed > result_file2.bed

6)	Con el propósito de extraer todas las filas que compartan el string ‘coding’ y guardarlo en un nuevo archivo:

► grep '.*' result_file2.bed | awk '$4 == "coding"' > result_file_coding1.bed


Cuarto punto:

1)	Para comprimir todos los archivos contenidos en ‘parcial1_StivennGutierrez’ en un archivo zip:

► zip -r parcial1_StivennGutierrez.zip Parcial1

2)	Para poder copiar este archivo zip a mi carpeta ‘Archivos_PARCIAL’, primero obtuve la ruta del archivo en el clúster con [pwd], luego me salí y dentro de la carpeta de la llave:

► scp -r -i bio.pt.pem -P 37022 bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_StivennGutierrez.zip /mnt/c/Users/stive/Downloads/Archivos_PARCIAL
