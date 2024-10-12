### Perlin Noise
    public static void PerlinNoise( int[,] _mapa, float _semilla)
    {
       // Altura del suelo
       int nuevoPunto ;

       for (int x = 0; x <= _mapa.GetUpperBound(0); x++) // Recorre el ancho del mapa
       {
           // obtener la altura para cada X del mapa
           nuevoPunto = Mathf.FloorToInt(Mathf.PerlinNoise(x, _semilla) * _mapa.GetUpperBound(1));

           for (int y = nuevoPunto; y >= 0; y--)
           {
               _mapa[x, y] = 1;
           }
       }
       
    }


Con el método **Mathf.PerlinNoise** se consigue un número entre 0 y 1. El valor resultante se multiplica por la altura del mapa (_mapa.GetUpperBound(1)) para conseguir la altura que va a tener la posición X del mapa.

Ejemplo de resultado:

![](https://github.com/ruperana/ruperana.github.io/blob/main/algoritmos/img/PerlinNoise_basico.png)
