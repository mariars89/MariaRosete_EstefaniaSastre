# Almacenamiento en Docker

> Realizado por Estefanía Sastre y María Rosete
>
> Enlace a repositorio de GitHub de Estefanía Sastre y María Rosete:
>
> https://github.com/mariars89/MariaRosete_EstefaniaSastre.git

[TOC]

# 1. Volúmenes

## 1. Crea un volumen docker que se llame miweb.

```bash
$docker volume create miweb
$docker volume ls
```

![image-20240131103921731](E:\Typora compartido\Almacenamiento en Docker\image-20240131103921731.png)

## 2. Crea un contenedor desde la imagen php:7.4-apache donde montes en el directorio /var/www/html (que sabemos que es el DocumentRoot del servidor que nos ofrece esa imagen) el volumen docker que has creado.
```bash
$docker run -d -p 80:80 -v miweb:/var/www/html php:7.4-apache
```

![image-20240131104503872](E:\Typora compartido\Almacenamiento en Docker\image-20240131104503872.png)

## 3. Utiliza el comando docker cp para copiar un fichero index.html en el directorio /var/www/html.

```bash
$nano index.html
```

![image-20240131105125394](E:\Typora compartido\Almacenamiento en Docker\image-20240131105125394.png)

Le damos contenido:

![image-20240131105756824](E:\Typora compartido\Almacenamiento en Docker\image-20240131105756824.png)

Comprobamos el id del contenedor anterior, y copiamos el index.html:

```bash
$docker ps
$docker cp index.html 453101639ac7:/var/www/html/index.html
```

![image-20240131105306445](E:\Typora compartido\Almacenamiento en Docker\image-20240131105306445.png)

## 4.  Accede al contenedor desde el navegador para ver la información ofrecida por el fichero index.html .
![image-20240131105836154](E:\Typora compartido\Almacenamiento en Docker\image-20240131105836154.png)



## 5. Borra el contenedor.

Comprobamos el id y el nombre, y lo paramos primero para poder borrarlo:

```bash
$docker ps
$docker stop 453101639ac7
$docker rm quirky_bardeen
```

![image-20240131110133952](E:\Typora compartido\Almacenamiento en Docker\image-20240131110133952.png)

![image-20240131105956413](E:\Typora compartido\Almacenamiento en Docker\image-20240131105956413.png)

## 6. Crea un nuevo contenedor y monta el mismo volumen como en el ejercicio anterior.

```bash
$docker run -d -p 80:80 -v miweb:/var/www/html php:7.4-apache
```

![image-20240131110347939](E:\Typora compartido\Almacenamiento en Docker\image-20240131110347939.png)

## 7. Accede al contenedor desde el navegador para ver la información ofrecida por el fichero index.html . ¿Seguía existiendo ese fichero?
![image-20240131110725801](E:\Typora compartido\Almacenamiento en Docker\image-20240131110725801.png)

Sí, el index.html sigue existiendo en el contenedor, porque se montó el mismo volumen en el directorio /var/www/html del nuevo contenedor. Cuando montas un volumen en un contenedor, cualquier cambio que se  realice en ese volumen se reflejará en todas las instancias de  contenedores que lo utilicen.

# 2. Bind mount

## 1. Crea un directorio en tu host y dentro crea un fichero index.html .

```bash
$mkdir directorioBind
$cd directorioBind
$nano index.html
```

![image-20240202085106081](E:\Typora compartido\Almacenamiento en Docker\image-20240202085106081.png)

![image-20240202085028425](E:\Typora compartido\Almacenamiento en Docker\image-20240202085028425.png)

## 2. Crea un contenedor desde la imagen php:7.4-apache donde montes en el directorio /var/www/html el directorio que has creado por medio de bind mount. 
```bash
$docker run -d -p 80:80 -v /directorioBind:/var/www/html php:7.4-apache
```

![image-20240202091043283](E:\Typora compartido\Almacenamiento en Docker\image-20240202091043283.png)

## 3. Accede al contenedor desde el navegador para ver la información ofrecida por el fichero index.html.
![image-20240202093439917](E:\Typora compartido\Almacenamiento en Docker\image-20240202093439917.png)



## 4. Modifica el contenido del fichero index.html en tu host y comprueba que al refrescar la página ofrecida por el contenedor, el contenido ha cambiado.
```bash
$docker exec -it relaxed_mcnulty bash
```

![image-20240202093916845](E:\Typora compartido\Almacenamiento en Docker\image-20240202093916845.png)

![image-20240202093949572](E:\Typora compartido\Almacenamiento en Docker\image-20240202093949572-1706863191278-1.png)

![image-20240202094001629](E:\Typora compartido\Almacenamiento en Docker\image-20240202094001629.png)

## 5. Borra el contenedor.

```bash
$docker stop relaxed_mcnulty
$docker rm relaxed_mcnulty
```

![image-20240202094211404](E:\Typora compartido\Almacenamiento en Docker\image-20240202094211404.png)



## 6. Crea un nuevo contenedor y monta el mismo directorio como en el ejercicio anterior.

```bash
$docker run -d -p 80:80 -v directorioBind:/var/www/html php:7.4-apache
```

![image-20240202094709068](E:\Typora compartido\Almacenamiento en Docker\image-20240202094709068.png)



## 7. Accede al contenedor desde el navegador para ver la información ofrecida por el fichero index.html . ¿Se sigue viendo el mismo contenido?
![image-20240202094001629](E:\Typora compartido\Almacenamiento en Docker\image-20240202094001629.png)