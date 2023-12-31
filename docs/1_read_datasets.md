read_datasets
================
Equipo 5
2023-11-20

# Lectura de los datos desde github

Los pixeles híperespectrales de los sets de datos se encuentran
guardados en github en archivos de formato csv agrupando los pixeles de
una misma clase en un mismo archivo. Siguiendo este modo, se tienen las
posiciones de los pixeles en archivos csv separados. Para nuestro
análisis, se guardarán los sets de datos por separado en archivos RData.

## Asignar ruta y nombre base de los archivos con los datos

La ruta principal hacia las carpetas del repositorio es la siguiente:

``` r
ruta <- "https://github.com/BeatrizPaulina/RosisDataCsv/raw/main"
```

Los archivos siguen la nomenclatura “nombre_dataset_class_n.csv” para
los archivos de entrada y “nombre_dataset_class_n_dct.csv” para los de
salida, donde “n” es el dígito identificador de la clase, por lo tanto,
se necesita guardar el nombre base para iterar.

``` r
pavia_uni_base <- file.path(ruta, "pavia_uni", "pavia_uni") #Pavia University
pavia_right_base <- file.path(ruta, "pavia_right", "pavia_right") #Pavia Right
pavia_left_base <- file.path(ruta, "pavia_left", "pavia_left" ) #Pavia Left
ruta_output <- file.path("..","..","data")
```

Se leen los datos guardando la información de cada csv en un elemento de
lista a través de las siguientes funciones:

``` r
leer_pixeles_csv <- function(nombre_base,lista_clases) {
   n_clases <- length(lista_clases)
   lista_pixeles_clases <- vector("list", length = n_clases)
   ind_aux <- 1
   for(i in lista_clases) {
     archivo_clase <- paste0(nombre_base,"_class_",i,".csv") #Conforma el nombre del archivo
     print(paste("Procesando",archivo_clase)) #Mensaje al usuario
     df_clase <- read.csv(archivo_clase, header=FALSE) #Lee el csv sin header
     matriz_clase <- unname(data.matrix(df_clase)) #Convierte a matriz numérica
     lista_pixeles_clases[[ind_aux]] <- matriz_clase #Agrega los datos a la lista del dataset
     ind_aux <- ind_aux + 1
   }
   return(lista_pixeles_clases)
}
```

``` r
leer_posiciones_csv <- function(nombre_base,lista_clases) {
   n_clases <- max(lista_clases)
   lista_posiciones <- vector("list", length = n_clases)
   ind_aux <- 1
   for(i in lista_clases) {
     archivo_clase <- paste0(nombre_base,"_pixel_pos_",i,".csv") #Conforma el nombre del archivo
     print(paste("Procesando",archivo_clase)) #Mensaje al usuario
     df_clase <- read.csv(archivo_clase, header=FALSE) #Lee el csv sin header
     matriz_clase <- unname(data.matrix(df_clase)) #Convierte a matriz numérica
     lista_posiciones[[ind_aux]] <- matriz_clase #Agrega los datos a la lista del dataset
     ind_aux <- ind_aux + 1
   }
   return(lista_posiciones)
}
```

Del mismo modo, se descargan los mapas de referencia para clasificación
o Ground Truths (GT) utilizando la siguiente función:

``` r
leer_gt_csv <- function(nombre_archivo) {
  df_clase <- read.csv(nombre_archivo, header=FALSE) #Lee el csv sin header
  matriz_gt <- unname(data.matrix(df_clase)) #Convierte a matriz numérica
  return(matriz_gt)
}
```

## Leer datos

A continuación se leen los datos de Pavia University, Pavia left y Pavia
Right:

### Pavia University

``` r
lista_clases_pavia_uni <- c(1,2,3,4,5,6,7,8,9) #Pavia University tiene 9 clases
lista_pixeles_pavia_uni <- leer_pixeles_csv(pavia_uni_base, lista_clases_pavia_uni)
```

    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_1.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_2.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_3.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_4.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_5.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_6.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_7.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_8.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_class_9.csv"

``` r
lista_posiciones_pavia_uni <- leer_posiciones_csv(pavia_uni_base, lista_clases_pavia_uni)
```

    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_1.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_2.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_3.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_4.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_5.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_6.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_7.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_8.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_uni/pavia_uni_pixel_pos_9.csv"

### Pavia Right

``` r
lista_clases_pavia_right <- c(1,2,3,4,5,6,7,8,9) #Pavia Right tiene 9 clases
lista_pixeles_pavia_right <- leer_pixeles_csv(pavia_right_base, lista_clases_pavia_right)
```

    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_1.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_2.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_3.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_4.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_5.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_6.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_7.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_8.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_class_9.csv"

``` r
lista_posiciones_pavia_right <- leer_posiciones_csv(pavia_right_base, lista_clases_pavia_right)
```

    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_1.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_2.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_3.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_4.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_5.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_6.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_7.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_8.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_right/pavia_right_pixel_pos_9.csv"

### Pavia Left

``` r
lista_clases_pavia_left <- c(1,2,3,4,5,6,8,9) #Pavia Left tiene 8 clases
lista_pixeles_pavia_left <- leer_pixeles_csv(pavia_left_base, lista_clases_pavia_left)
```

    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_1.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_2.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_3.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_4.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_5.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_6.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_8.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_class_9.csv"

``` r
lista_posiciones_pavia_left <- leer_posiciones_csv(pavia_left_base, lista_clases_pavia_left)
```

    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_1.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_2.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_3.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_4.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_5.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_6.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_8.csv"
    ## [1] "Procesando https://github.com/BeatrizPaulina/RosisDataCsv/raw/main/pavia_left/pavia_left_pixel_pos_9.csv"

### Ground Truths (GT)

Las imágenes de referencia etiquetadas o Ground Truths (GT) se
encuentran en una carpeta diferente en el repositorio. Los GT consisten
en archivos csv con matrices que indican la etiqueta en la posición del
pixel.

``` r
ruta_gt <- file.path(ruta,"ground_truths")

pavia_uni_gt <- leer_gt_csv(file.path(ruta_gt,"pavia_uni_gt.csv"))
pavia_right_gt <- leer_gt_csv(file.path(ruta_gt,"pavia_right_gt.csv"))
pavia_left_gt <- leer_gt_csv(file.path(ruta_gt,"pavia_left_gt.csv"))
```

### Guardar estructuras de datos

Se guardan las estructuras de datos.

``` r
save(lista_clases_pavia_uni, lista_pixeles_pavia_uni, lista_posiciones_pavia_uni, pavia_uni_gt,  file = file.path(ruta_output,"pavia_uni.RData"))
save(lista_clases_pavia_right, lista_pixeles_pavia_right, lista_posiciones_pavia_right, pavia_right_gt, file = file.path(ruta_output,"pavia_right.RData"))
save(lista_clases_pavia_left, lista_pixeles_pavia_left, lista_posiciones_pavia_left, pavia_left_gt, file = file.path(ruta_output,"pavia_left.RData"))
```
