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


### Perlin Noise Suavizado

    public static void PerlinNoise_Suavizado( int[,] _mapa, float _semilla, int _intervalo)
    {
        if (_intervalo > 1)
        {
            // Utilizados en el proceso de suavizado
            Vector2Int posActual, posAnterior;

            // los puntos correspondientes para el suavizado. Una lista para cada eje.
            List<int> ruidoX = new List<int>();
            List<int> ruidoY = new List<int>();

            int nuevoPunto, puntos;

            // Genera el ruido
            // dimension de la x --> _mapa.GetUpperBound(0)
            for (int x = 0; x <= _mapa.GetUpperBound(0) + _intervalo; x += _intervalo)
            {
                // Obtener la altura
                nuevoPunto = Mathf.FloorToInt(( Mathf.PerlinNoise(x, _semilla)) * _mapa.GetUpperBound(1));

                ruidoY.Add( nuevoPunto);
                ruidoX.Add( x);
            }

            puntos = ruidoY.Count;

            // Empieza en la primera posición  
            for (int i = 1; i < puntos; i++)
            {
                posActual = new Vector2Int( ruidoX[i], ruidoY[i]);
                posAnterior = new Vector2Int (ruidoX[i - 1], ruidoY[i - 1]);

                // calcular la recta que une a los 2 puntos
                Vector2 diferencia = posActual - posAnterior;

                // cuanto habría que bajar en cada paso, en cada x
                float cambioEnAltura = diferencia.y / _intervalo;

                // Guarda la altura actual
                float alturaActual = posAnterior.y;

                // no nos podemos pasar de la posición actual y de la dismensiones del mapa
                // Generar los bloques del intervalo de la x anterior a la x actual
                for (int x = posAnterior.x; x < posActual.x && x <= _mapa.GetUpperBound(0); x++)
                {
                    // empezamos desde la altura actual
                    for (int y = Mathf.FloorToInt(alturaActual); y >= 0; y--)
                    {
                        _mapa[x, y] = 1;
                    }
                    alturaActual += cambioEnAltura;
                }
            }
        }
        else
        {
            // no podemos suavisar, se realiza el método anterior --> PerlinNoise
             PerlinNoise( _mapa, _semilla);
        }     
    }
