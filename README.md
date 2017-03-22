# exploracion
Análisis Exploratorio Inicial
<table style="width: 100%;" border="0" cellpadding="5">
<tbody>
<tr>
<td style="text-align: center; border-radius: 5px;" colspan="2" bgcolor="#3366CC" width="80%"><span style="font-size: x-large; font-family: arial, helvetica, sans-serif; color: #ffffff;" data-mce-mark="1">EXPLORACIÓN DE DATOS<br /></span></td>
</tr>
<tr onmouseover="this.style.backgroundColor = '#F5F5F5'" onmouseout="this.style.backgroundColor = '#fff'" style="background-color: #ffffff;">
<td style="text-align: center; width: 20%;" valign="top"><img src="images/rtwl.png" width="100%" height="" alt="rtwl" /></td>
<td style="text-align: center; width: 80%;" valign="top">
<p align="justify"></p>
<p align="justify"><span style="font-size: x-large; color: #003366;" data-mce-mark="1">Right to Work Law</span></p>
<p align="justify"></p>
<p style="color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; text-align: start;">Right to Work Law es un conjunto de normas implementadas en los Estados Unidos durante la Administración Obama que buscaba proteger al trabajador al impedir la tercerización laboral y exigir la vinculación directa del empleado.</p>
<p style="color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; text-align: start;">La base de datos proviene de un ejercicio realizado por la Universidad de California en Los Ángeles (UCLA) y contiene algunas de las variables más relevantes para el análisis del impacto de dicha política.</p>
<p style="color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; text-align: start;">El ejercicio consiste en un análisis exploratorio de lo datos que busca identificar patrones y/o parámetros que faciliten la posterior modelización. Por los alcances del curso en este punto el análisis se basará en las posibles relaciones entre las variables Tasa de Desempleo (URate) e Ingreso (Income)</p>
<p style="color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; text-align: start;">Recuerde la discusión hecha en clase</p>
</td>
</tr>
</tbody>
</table>
<p></p>
<h2 style="text-align: left;">Importación de Datos</h2>
<p>Para empezar, vamos a tomar la información del archivo <strong><span style="font-family: 'courier new', courier, monospace;" data-mce-mark="1">RightToWorkLaw.txt</span></strong> almacenado en la plataforma <a href="https://github.com/karlosmantilla/importacion" title="Enlace a GitHub" target="_blank">GitHub</a> y en <a href="https://www.dropbox.com/s/i0pnh74xmtnaopx/RightToWorkLaw.txt?dl=0" title="Enlace a Dropbox" target="_blank">dropbox</a>. Descargado el archivo se procede a importarlo. Para este caso, se va a utilizar la acción básica de copiar y pegar; para ello se abre el archivo con el bloc de notas, se copia el contenido y se ejecuta el siguiente comando:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; datos.cp &lt;- read.delim("clipboard")
</code></span></pre>
<p>Con lo anterior se han copiado los datos al programa. Para observar si la importación fue correcta se puede emplear:</p>
<pre><code class="r"><span style="color: #ff0000;">&gt; head(datos.cp)</span>
<span style="color: #003366;">            COL   PD URate     Pop Taxes Income RTWL
Atlanta     169  414  13.6 1790128  5128   2961    1
Austin      143  239  11.0  396891  4303   1711    1
Bakersfield 339   43  23.7  349874  4166   2122    0
Baltimore   173  951  21.0 2147850  5001   4654    0
Baton Rouge  99  255  16.0  411725  3965   1620    1
Boston      363 1257  24.4 3914071  4928   5634    0
</span></code></pre>
<p>Inicialmente, se puede observar el tipo de variable y algunos resúmenes numéricos básicos:</p>
<pre><code class="r"><span style="color: #ff0000;">&gt; sapply(datos.cp, class)</span>
<span style="color: #003366;">      COL        PD     URate       Pop     Taxes    Income      RTWL 
"integer" "integer" "numeric" "integer" "integer" "integer" "integer"</span> 
<span style="color: #ff0000;">&gt; summary(datos.cp)</span>
<span style="color: #003366;">          City         COL              PD             URate            Pop              Taxes          Income          RTWL       
 Atlanta    : 1   Min.   : 99.0   Min.   :  43.0   Min.   : 6.50   Min.   : 162304   Min.   :3965   Min.   : 782   Min.   :0.0000  
 Austin     : 1   1st Qu.:170.8   1st Qu.: 302.0   1st Qu.:17.82   1st Qu.: 497050   1st Qu.:4620   1st Qu.:3110   1st Qu.:0.0000  
 Bakersfield: 1   Median :205.5   Median : 400.0   Median :24.05   Median :1408054   Median :4858   Median :4865   Median :0.0000  
 Baltimore  : 1   Mean   :223.6   Mean   : 780.2   Mean   :24.22   Mean   :2040736   Mean   :4903   Mean   :4709   Mean   :0.2632  
 Baton Rouge: 1   3rd Qu.:266.5   3rd Qu.: 963.8   3rd Qu.:30.00   3rd Qu.:2355462   3rd Qu.:5166   3rd Qu.:6082   3rd Qu.:0.7500  
 Boston     : 1   Max.   :381.0   Max.   :6908.0   Max.   :39.20   Max.   :9561089   Max.   :6404   Max.   :8392   Max.   :1.0000  
 (Other)    :32                                                                                                                    
</span></code></pre>
<p>Ahora, aprovechando que se trata de datos de corte transversal, se puede asignar el nombre de cada a ciudad a la fila correspondiente usando el comando row.names() e indicando que la columna City es la que se va a emplear. Como dicha columna es la primera del conjunto de datos se emplea la discriminación datos.cp[,1] para esto:</p>
<pre><code class="r"><span style="color: #ff0000;">&gt; row.names(datos.cp)&lt;-datos.cp[,1]
&gt; datos.cp&lt;-datos.cp[,-1]
&gt; head(datos.cp)</span>
<span style="color: #003366;">            COL   PD URate     Pop Taxes Income RTWL
Atlanta     169  414  13.6 1790128  5128   2961    1
Austin      143  239  11.0  396891  4303   1711    1
Bakersfield 339   43  23.7  349874  4166   2122    0
Baltimore   173  951  21.0 2147850  5001   4654    0
Baton Rouge  99  255  16.0  411725  3965   1620    1
Boston      363 1257  24.4 3914071  4928   5634    0
</span></code></pre>
<p>Ahora, se procede a construir la matriz de correlaciones:</p>
<pre><code class="r"><span style="color: #ff0000;">&gt; cor.mat &lt;- round(cor(datos.cp),2)</span><br /><span style="color: #ff0000;">&gt; cor.mat</span>
<span style="color: #003366;">         COL    PD URate   Pop Taxes Income  RTWL
COL     1.00  0.39  0.27  0.43  0.37   0.29 -0.47
PD      0.39  1.00  0.42  0.83  0.34   0.07 -0.28
URate   0.27  0.42  1.00  0.37  0.09   0.53 -0.79
Pop     0.43  0.83  0.37  1.00  0.50  -0.04 -0.30
Taxes   0.37  0.34  0.09  0.50  1.00   0.06 -0.29
Income  0.29  0.07  0.53 -0.04  0.06   1.00 -0.51
RTWL   -0.47 -0.28 -0.79 -0.30 -0.29  -0.51  1.00</span>
<!-----></code></pre>
<p>Para el análisis exploratorio, es aconsejable realizar primero un análisis gráfico. Para obtener gráficas de más calidad y un poco más completas se pueden emplear algunas librerías. Como es probable que éstas no se encuentren instaladas, se procede a su instalación mediante las siguientes líneas:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; install.packages('ggplot2')
&gt; install.packages('reshape2')
&gt; install.packages('GGally')
&gt; install.packages('ggrepel')
</code></span></pre>
<p>Con las líneas anteriores se selecciona el CRAN (<span>Comprehensive R Archive Network); aquí usted selecciona en la opción (HTTP mirror) de la primera ventana emergente y en la segunda emergente usted selecciona el servidor de preferencia.</span></p>
<p><span>Instalados los paquetes proceda a llamar las librerías:</span></p>
<pre><span style="color: #ff0000;"><code class="r">&gt; library(ggplot2)
&gt; library(reshape2)
&gt; library(GGally)
&gt; library(ggrepel)
</code></span></pre>
<p>En primera instancia se va a construir con la matriz de correlaciones un <strong><span style="font-family: 'courier new', courier, monospace;">data.frame</span></strong> (ordenamiento de datos) para lo que se emplea lo siguiente</p>
<pre><code class="r"><span style="color: #ff0000;">&gt; melted_cormat&lt;-melt(cor.mat)
&gt; head(melted_cormat)</span>
<span style="color: #003366;">    Var1 Var2 value
1    COL  COL  1.00
2     PD  COL  0.39
3  URate  COL  0.27
4    Pop  COL  0.43
5  Taxes  COL  0.37
6 Income  COL  0.29
</span></code></pre>
<p>Ahora es posible graficar la matriz de correlaciones usando la siguiente linea de comandos:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(data = melted_cormat, aes(x=Var1, y=Var2, fill=value)) + geom_tile()
</code></span></pre>
<p><img src="images/matrizxorr1.png" width="50%" height="" alt="matriz de correlaciones" style="display: block; margin-left: auto; margin-right: auto;" /></p>
<p>Se observa en el lado derecho de la gráfica la nomenclatura para interpretar los valores del gráfico. Existe otra gráfica más simple para su interpretación y se construye como se indica a continuación:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggcorr(datos.cp, palette = "RdBu", label = TRUE)
</code></span></pre>
<p><img src="images/matrizxorr2.png" width="50%" height="" alt="Matriz de Correlaciones 2" style="display: block; margin-left: auto; margin-right: auto;" /></p>
<p>Con la información suministrada en este gráfico se puede considerar las posibles configuraciones del modelo a construir. Así que se procede a construir un diagrama de dispersión para las variables de interés en este ejercicio; para ello se emplea la siguiente línea de comandos (recuerde la explicación sobre los argumentos dada en clase):</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; win.graph(10,6,6) # Sirve para determinar las dimensiones de la imagen: 10 pts de ancho, 6 pts de alto y Tamaño de Fuente 6
&gt; ggplot(datos.cp, aes(x=Income, y=URate)) + geom_point(size=3, col="darkgreen")
</code></span></pre>
<p><img src="images/dispersion2.png" width="70%" height="" alt="Dispersión" style="display: block; margin-left: auto; margin-right: auto;" /></p>
<p></p>
<p>Es posible incluir más variables que permitan intuir el comportamiento del modelo; para el ejemplo se va a incluir el tamaño poblacional para analizar las posibles influencias de esta variable:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp, aes(x=Income, y=URate)) +  geom_point(aes(size=Pop),col="darkgreen")
</code></span></pre>
<p style="text-align: center;"><img src="images/dispersion3.png" width="75%" height="" alt="dispersión3" /></p>
<p style="text-align: left;">Son muchas las opciones gráficas que hay. Por ejemplo, es posible incluir el nombre de las ciudades para identificar las observaciones en el diagrama de dispersión:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp, aes(x=Income, y=URate)) +  geom_point(aes(size=Pop),col="darkgreen")+ geom_text_repel(label=rownames(datos.cp))
</code></span></pre>
<p style="text-align: center;"><img src="images/dispersion4.png" width="75%" height="" alt="Dispersion" /></p>
<p style="text-align: left;"></p>
<p>O se puede añadir un gráfico de densidad bidimensional para identificar los posibles centros de atracción de los datos:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp, aes(x=Income, y=URate)) +  geom_point(aes(size=Pop),col="darkgreen")+ geom_text_repel(label=rownames(datos.cp)) + geom_density_2d()
</code></span></pre>
<p style="text-align: center;"><img src="images/densidad1.png" width="75%" height="" alt="densidad" /></p>
<p>O incluir la recta de regresión y el intervalo de confianza para tener una aproximación del posible modelo:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp, aes(x=Income, y=URate)) +  geom_point(aes(size=Pop),col="darkgreen")+ geom_text_repel(label=rownames(datos.cp)) +  geom_smooth(method=lm)
</code></span></pre>
<p><img src="images/regresion.png" width="75%" height="" style="display: block; margin-left: auto; margin-right: auto;" /></p>
<p>También es posible graficar otro tipo de regresión y combinar con más elementos:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp, aes(x=Income, y=URate)) +  geom_point(aes(size=Pop),col="darkgreen")+ geom_text_repel(label=rownames(datos.cp)) +  geom_smooth()+ geom_density_2d()
</code></span></pre>
<p style="text-align: center;"><img src="images/regression2.png" width="75%" height="" alt="Regresion Loess" /></p>
<p style="text-align: left;">O se puede incluir el efecto de implementación o no del estatuto RTWL</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp,aes(x=Income, y=URate, color=as.factor(RTWL))) + geom_point(aes(size=Pop))+ scale_color_manual(values = c('#999999','#E69F00')) + theme(legend.position=c(0,1), legend.justification=c(0,1))+ geom_smooth(method=lm)+ geom_density_2d()
</code></span></pre>
<p style="text-align: center;"><img src="images/regresionanova.png" width="75%" height="" alt="anova" /></p>
<p style="text-align: left;">Finalmente, es posible incluir otros gráficos como diagramas de caja:</p>
<pre><span style="color: #ff0000;"><code class="r">&gt; ggplot(datos.cp,aes(y = URate, x = factor(RTWL)))  + geom_boxplot()+ labs(x="Application RTWL",y="Unemployed Rate")+  stat_summary(fun.y=mean, colour="red", geom="point", shape=20, size=3,show_guide = FALSE)
</code></span></pre>
<p style="text-align: left;"><img src="images/boxplot.png" width="50%" height="" alt="Boxplot" style="display: block; margin-left: auto; margin-right: auto;" /></p>
<p style="text-align: left;">Recuerde que una adecuada exploración gráfica ayuda a evitar construir modelos innecesarios</p>
