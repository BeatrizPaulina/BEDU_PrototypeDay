clean_datasets
================
Equipo 5
2023-11-20

# Homologación de los sets de datos

Los sets de datos Pavia University, Pavia Right y Pavia Left contienen,
cada uno, un mapa de referencia etiquetado con diferentes clases. El
problema que se encuentra es que los sets tienen clases que se
intersectan, sin embargo, en su respectivo mapa de referencia las
etiquetas de las clases son diferentes. Por lo tanto se utiliza la
siguiente tabla para el etiquetado:

|          Clase           | Nueva Etiqueta | P. University | P. Left | P. Right |
|:------------------------:|:--------------:|:-------------:|:-------:|:--------:|
|                          |                |    Reales     | Reales  |  Reales  |
|        **Trees**         |       1        |       4       |    2    |    2     |
|       **Asphalt**        |       2        |       1       |    6    |    6     |
| **Self-Blocking Bricks** |       3        |       8       |    4    |    4     |
|       **Bitumen**        |       4        |       7       |   \-    |    7     |
|       **Shadows**        |       5        |       9       |    9    |    9     |
|       **Meadows**        |       6        |       2       |    3    |    3     |
|      **Bare soil**       |       7        |       6       |    5    |    5     |
|        **Gravel**        |       8        |       3       |   \-    |    \-    |
| **Painted metal sheets** |       9        |       5       |   \-    |    \-    |
|        **Tiles**         |       10       |      \-       |    8    |    8     |
|        **Water**         |       11       |      \-       |    1    |    1     |

## Lectura local de los datos

Es necesario ejecutar antes el escript “read_datasets” del proyecto para
descargar los datos desde github y guardarlos localmente. En este script
se leen los archivos RData.

``` r
ruta_datos <- file.path("..","..","data")
load(file.path(ruta_datos,"pavia_uni.RData"))
load(file.path(ruta_datos,"pavia_right.RData"))
load(file.path(ruta_datos,"pavia_left.RData"))
```

## Ordenamiento de los datos

Para homologar las clases, se ordenan las listas siguiendo el orden dado
en la tabla anterior. Entonces, se utilizan los siguientes vectores para
el ordenamiento:

``` r
# Vectores para ordenar listas
orden_puni <- c(4,1,8,7,9,2,6,3,5)
orden_pright <- c(2,6,4,7,9,3,5,8,1)
orden_pleft <- c(2,6,4,8,3,5,7,1) # 8 es posición 7 y 9 es posición 8 porque originalmente no tiene clase 7.

# Vectores para cambiar GT
orden_puni_gt <- c(2,6,8,1,9,7,4,3,5)
orden_pright_gt <- c(11,1,6,3,7,2,4,10,5)
orden_pleft_gt <- c(11,1,6,3,7,2,0,10,5)

# Lista de clases después del ordenamiento
clases_puni <- c(1,2,3,4,5,6,7,8,9)
clases_pright <- c(1,2,3,4,5,6,7,10,11)
clases_pleft <- c(1,2,3,5,6,7,10,11)
```

Ahora se realiza el ordenamiento de las listas con la siguiente función:

``` r
ordenar_listas <- function(lista_desordenada, orden) {
   n_clases <- length(orden)
   lista_ordenada <- vector("list", length = n_clases)
  for(i in 1:n_clases) {
    lista_ordenada[[i]] <- lista_desordenada[[orden[i]]]
  }
   return(lista_ordenada)
}
```

Mientras que para modificar los GT, se utiliza esta otra función:

``` r
cambiar_gt <- function(gt, orden_etiquetas) {
  etiquetas <- as.integer(orden_etiquetas)
  dim_gt <- dim(gt)
  gt_nuevo <- matrix(0L, nrow = dim_gt[1], ncol = dim_gt[2])
  for(y in 1:dim_gt[1]) {
    for(x in 1:dim_gt[2]) {
      gt_lab <- gt[y, x]
      if(gt_lab != 0) {
        gt_nuevo[y, x] <- etiquetas[gt_lab]
      }
    }
  }
  return(gt_nuevo)
}
```

### Pavia University

``` r
pix_puni <- ordenar_listas(lista_pixeles_pavia_uni, orden_puni)
pos_puni <- ordenar_listas(lista_posiciones_pavia_uni, orden_puni)
gt_puni <- cambiar_gt(pavia_uni_gt, orden_puni_gt)
```

### Pavia Right

``` r
pix_pright <- ordenar_listas(lista_pixeles_pavia_right, orden_pright)
pos_pright <- ordenar_listas(lista_posiciones_pavia_right, orden_pright)
gt_pright <- cambiar_gt(pavia_right_gt, orden_pright_gt)
```

### Pavia Left

``` r
pix_pleft <- ordenar_listas(lista_pixeles_pavia_left, orden_pleft)
pos_pleft <- ordenar_listas(lista_posiciones_pavia_left, orden_pleft)
gt_pleft <- cambiar_gt(pavia_left_gt, orden_pleft_gt)
```

## Guardar estructura de los datos

Se guardan las estructuras ordenadas con nuevos nombres.

``` r
save(clases_puni, pix_puni, pos_puni, gt_puni, clases_pright, pix_pright, pos_pright, gt_pright, clases_pleft, pix_pleft, pos_pleft, gt_pleft, file = file.path(ruta_datos,"datasets.RData"))
```
