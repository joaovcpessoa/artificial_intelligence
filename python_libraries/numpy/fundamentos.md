# Numpy

## Motivação

NumPy é o pacote fundamental para computação científica em Python. É uma biblioteca Python que fornece um objeto de array multidimensional, vários objetos derivados (como arrays e matrizes mascaradas) e uma variedade de rotinas para operações rápidas em arrays, incluindo operações matemáticas, lógicas, manipulação de formas, ordenação, seleção, entrada/saída, transformadas discretas de Fourier, álgebra linear básica, operações estatísticas básicas, simulação aleatória e muito mais.

No núcleo do pacote NumPy está o objeto `ndarray`. Este encapsula arrays n-dimensionais de tipos de dados homogêneos, com muitas operações sendo realizadas em código compilado para otimizar o desempenho. Existem diversas diferenças importantes entre os arrays do NumPy e as sequências padrão do Python:

- Os arrays NumPy têm um tamanho fixo na sua criação, ao contrário das listas em Python (que podem crescer dinamicamente). Alterar o tamanho de um array `ndarray` criará um novo array e excluirá o original
- Os elementos em um array NumPy devem ser todos do mesmo tipo de dados e, portanto, terão o mesmo tamanho na memória. Exceção: é possível ter arrays de objetos (Python, incluindo NumPy), permitindo assim arrays com elementos de tamanhos diferentes
- Os arrays NumPy facilitam operações matemáticas avançadas e de outros tipos em grandes quantidades de dados. Normalmente, essas operações são executadas de forma mais eficiente e com menos código do que seria possível usando as sequências nativas do Python
- Um número crescente de pacotes científicos e matemáticos baseados em Python utiliza arrays NumPy; embora normalmente suportem entrada de sequências Python, eles convertem essa entrada em arrays NumPy antes do processamento e, frequentemente, geram arrays NumPy como saída. Em outras palavras, para usar com eficiência grande parte (talvez até a maioria) dos softwares científicos/matemáticos atuais baseados em Python, saber usar apenas os tipos de sequência nativos do Python é insuficiente, também é necessário saber usar arrays NumPy.

Os aspectos relacionados ao tamanho da sequência e à velocidade são particularmente importantes na computação científica. Como um exemplo simples, considere o caso de multiplicar cada elemento de uma sequência unidimensional pelo elemento correspondente em outra sequência de mesmo comprimento. Se os dados estiverem armazenados em duas listas Python, `a` e `b`, poderíamos iterar sobre cada elemento:

```python
a = [1, 2, 3, 4, 5]
b = [10, 11, 12, 13, 14]
c = []
for i in range(len(a)):
    c.append(a[i]*b[i])
```
Isso produz a resposta correta, mas se `a` e `b` contiverem milhões de números, pagaremos o preço pelas ineficiências dos loops em Python. Poderíamos realizar a mesma tarefa muito mais rapidamente em C escrevendo (para maior clareza, omitimos declarações e inicializações de variáveis, alocação de memória, etc.).

```C
a = [1, 2, 3, 4, 5]
b = [10, 11, 12, 13, 14]
for (i = 0; i < rows; i++) {
  c[i] = a[i]*b[i];
}
```

Isso elimina toda a sobrecarga envolvida na interpretação do código Python e na manipulação de objetos Python, mas à custa dos benefícios obtidos com a programação em Python. Além disso, o trabalho de codificação necessário aumenta com a dimensionalidade dos nossos dados. No caso de uma matriz bidimensional, por exemplo, o código C (abreviado como antes) se expande para

```C
a = [1, 2, 3, 4, 5]
b = [10, 11, 12, 13, 14]
for (i = 0; i < rows; i++) {
  for (j = 0; j < columns; j++) {
    c[i][j] = a[i][j]*b[i][j];
  }
}
```

NumPy nos oferece o melhor dos dois mundos: operações elemento a elemento são o "modo padrão" quando um array `ndarray` está envolvido, mas essas operações são executadas rapidamente por código C pré-compilado.

```python
a = np.array([1, 2, 3, 4, 5])
b = np.array([10, 11, 12, 13, 14])
c = a * b
```

Faz o mesmo que os exemplos anteriores, com velocidades próximas à de C, mas com a simplicidade de código que esperamos de algo baseado em Python. Na verdade, o idioma NumPy é ainda mais simples! Este último exemplo ilustra duas das características do NumPy que são a base de grande parte de seu poder: vetorização e broadcasting.

A vetorização descreve a ausência de qualquer repetição explícita, indexação, etc., no código — essas operações ocorrem, naturalmente, "nos bastidores" do código C otimizado e pré-compilado. O código vetorizado possui muitas vantagens, entre as quais:

- É mais conciso e fácil de ler
- Exige menos linhas de código e isso geralmente significa menos bugs
- O código se assemelha mais à notação matemática padrão
- A vetorização resulta em um código mais "pythonico"

Broadcasting é o termo usado para descrever o comportamento implícito elemento a elemento das operações; de modo geral, no NumPy, todas as operações, não apenas as aritméticas, mas também as lógicas, bit a bit, funcionais etc., se comportam dessa maneira implícita elemento a elemento, ou seja, elas são transmitidas (broadcasting). Além disso, no exemplo acima, `a` ae ` bb` poderiam ser arrays multidimensionais do mesmo formato, ou um escalar e um array, ou até mesmo dois arrays com formatos diferentes, desde que o array menor seja "expansível" para o formato do maior de forma que a transmissão resultante seja inequívoca. Para obter as "regras" detalhadas de broadcasting, consulte Broadcasting .

O NumPy oferece suporte completo a uma abordagem orientada a objetos, começando, mais uma vez, com o `ndarray`. Por exemplo, `ndarray` é uma classe que possui inúmeros métodos e atributos. Muitos de seus métodos são espelhados por funções no namespace mais externo do NumPy, permitindo que o programador codifique no paradigma que preferir. Essa flexibilidade permitiu que o dialeto de arrays do NumPy e  a classe ndarray se tornassem a linguagem padrão para a troca de dados multidimensionais em Python.

## Instalação

```python
# Gerenciador padrão do python
pip install numpy
# Gerenciador de pacotes Python moderno, projetado para velocidade e simplicidade.
uv pip install numpy
# Gerenciador de pacotes multiplataforma para Python e outras linguagens.
pixi add numpy
```

Em seguida você importa a biblioteca e verifica a versão

```python
import numpy as np
print(np.__version__)
```

## Criação da matriz

Existem 6 mecanismos gerais para criar arrays:

1. Conversão de outras estruturas Python (ou seja, listas e tuplas)
2. Funções intrínsecas de criação de arrays NumPy (ex: arange, ones, zeros, etc.)
3. Replicar, unir ou modificar matrizes existentes
4. Leitura de arrays do disco, seja em formatos padrão ou personalizados
5. Criação de arrays a partir de bytes brutos através do uso de strings ou buffers
6. Utilização de funções especiais da biblioteca (ex: random)

Você pode usar esses métodos para criar `ndarrays` ou `arrays estruturados` (irei abordar sobre mais a frente). Aqui iremos nos focar nos métodos gerais para a criação de `ndarrays`.

### 1. Convertendo sequências Python em arrays NumPy

Os arrays NumPy podem ser definidos usando sequências Python, como listas e tuplas. Listas e tuplas são definidas usando `list` [...] e `tuple` (...), respectivamente. Listas e tuplas podem definir a criação de ndarrays:

- Uma lista de números criará uma matriz unidimensional.
- Uma lista de listas criará uma matriz bidimensional.
- Listas aninhadas adicionais criarão arrays de dimensões superiores. Em geral, qualquer objeto de array é chamado de `ndarray` no NumPy.

```python
import numpy as np
a1D = np.array([1, 2, 3, 4])
a2D = np.array([[1, 2], [3, 4]])
a3D = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])
```

Ao definir um novo `numpy.array`, você deve considerar o tipo de dados (dtype) dos elementos, que pode ser especificado explicitamente. Esse recurso oferece maior controle sobre as estruturas de dados subjacentes e como os elementos são tratados em funções C/C++. Quando os valores não se encaixam e você está usando um array dtype, o NumPy pode gerar um erro.

```python
>>>import numpy as np
>>>np.array([127, 128, 129], dtype=np.int8)
Traceback (most recent call last):
...
OverflowError: Python integer 128 out of bounds for int8
```

Um inteiro com sinal de 8 bits representa números inteiros de -128 a 127. Atribuir valores inteiros `int8` fora desse intervalo ao array resulta em estouro de capacidade (overflow). Essa característica pode ser frequentemente mal compreendida. Se você realizar cálculos com valores incompatíveis dtypes, poderá obter resultados indesejados, por exemplo:

```python
>>>import numpy as np
>>>a = np.array([2, 3, 4], dtype=np.uint32)
>>>b = np.array([5, 6, 7], dtype=np.uint32)
>>>c_unsigned32 = a - b
>>>print('unsigned c:', c_unsigned32, c_unsigned32.dtype)
unsigned c: [4294967293 4294967293 4294967293] uint32
>>>c_signed32 = a - b.astype(np.int32)
>>>print('signed c:', c_signed32, c_signed32.dtype)
signed c: [-3 -3 -3] int64
```

Observe que, ao realizar operações com dois arrays do mesmo tipo dtype(`uint32`), o array resultante será do mesmo tipo. Ao realizar operações com arrays de tipos diferentes, o NumPy atribuirá um novo tipo que satisfaça todos os elementos do array envolvidos no cálculo; aqui, `uint32` e `int32` podem ser representados como `int64`.
O comportamento padrão do NumPy é criar arrays em inteiros com sinal de 32 ou 64 bits (dependendo da plataforma e correspondendo ao tamanho `long` da linguagem C) ou em números de ponto flutuante de dupla precisão. Se você espera que seus arrays de inteiros sejam de um tipo específico, então você precisa especificar o tipo de dados (dtype) ao criar o array.

### 2. Funções intrínsecas de criação de arrays NumPy 

O NumPy possui mais de 40 funções integradas para a criação de arrays, conforme descrito nas rotinas de criação de arrays . Essas funções podem ser divididas em três categorias principais, com base na dimensão do array que criam:

- matrizes 1D
- matrizes 2D
- ndarrays



### 3 Replicar, unir ou modificar arrays existentes

Depois de criar arrays, você pode replicá-los, uni-los ou modificá-los para criar novos arrays. Ao atribuir um array ou seus elementos a uma nova variável, você precisa especificar explicitamente `numpy.copy` o array, caso contrário, a variável será uma representação do array original.

```python
>>>import numpy as np
>>>a = np.array([1, 2, 3, 4, 5, 6])
>>>b = a[:2]
>>>b += 1
>>>print('a =', a, '; b =', b)
a = [2 3 3 4 5 6] ; b = [2 3]
```

Neste exemplo, você não criou um novo array. Você criou uma variável, `b` que armazenava os dois primeiros elementos de `a`. Ao adicionar 1 a `b` você obteria o mesmo resultado adicionando 1 em `a[:2]`. Se você quiser criar um novo array, use a rotina `numpy.copy` de criação de arrays da seguinte forma:

```python
>>>import numpy as np
>>>a = np.array([1, 2, 3, 4])
>>>b = a[:2].copy()
>>>b += 1
>>>print('a = ', a, 'b = ', b)
a =  [1 2 3 4] b =  [2 3]
```

Existem diversas rotinas para unir arrays existentes, como `numpy.vstack`, `numpy.hstack` e `numpy.block`. Aqui está um exemplo de como unir quatro arrays 2x2 em um array 4x4 usando `block`:

```python
>>>import numpy as np
>>>A = np.ones((2, 2))
>>>B = np.eye(2, 2)
>>>C = np.zeros((2, 2))
>>>D = np.diag((-3, -4))
>>>np.block([[A, B], [C, D]])
array([[ 1.,  1.,  1.,  0.],
       [ 1.,  1.,  0.,  1.],
       [ 0.,  0., -3.,  0.],
       [ 0.,  0.,  0., -4.]])
```

Outras rotinas usam sintaxe semelhante para concatenar ndarrays. Consulte a documentação da rotina para obter mais exemplos e informações sobre a sintaxe.

### 4 Leitura de arrays do disco, seja em formatos padrão ou personalizados

Este é o caso mais comum de criação de grandes arrays. Os detalhes dependem muito do formato dos dados no disco. Esta seção fornece orientações gerais sobre como lidar com vários formatos.

<b>Formatos binários padrão</b>

Diversos campos possuem formatos padrão para dados de matriz. A seguir, listamos aqueles que possuem bibliotecas Python conhecidas para lê-los e retornar matrizes NumPy.

```sh
HDF5: h5py
FITS: Astropy
```

Exemplos de formatos que não podem ser lidos diretamente, mas para os quais não é difícil converter, são aqueles suportados por bibliotecas como a PIL (capaz de ler e gravar muitos formatos de imagem, como jpg, png, etc).

<b>Formatos ASCII comuns</b>

Arquivos delimitados, como arquivos de valores separados por vírgula (CSV) e de valores separados por tabulação (TSV), são usados ​​em programas como Excel e LabVIEW. Funções em Python podem ler e analisar esses arquivos linha por linha. O NumPy possui duas rotinas padrão para importar um arquivo com dados delimitados `numpy.loadtxt` e `numpy.genfromtxt`. Essas funções têm casos de uso mais complexos em leitura e gravação de arquivos.

```txt
$ cat simple.csv
x, y
0, 0
1, 1
2, 4
3, 9
```

```python
>>>import numpy as np
>>>np.loadtxt('simple.csv', delimiter = ',', skiprows = 1) 
array([[0., 0.],
       [1., 1.],
       [2., 4.],
       [3., 9.]])
```

### 5 Criando arrays a partir de bytes brutos através do uso de strings ou buffers

Existem diversas abordagens que podem ser utilizadas. Se o arquivo tiver um formato relativamente simples, pode-se escrever uma biblioteca de E/S simples e usar as funções `fromfile()` e métodos `.tofile()` do NumPy para ler e escrever arrays NumPy diretamente (mas atenção à ordem dos bytes!). Se existir uma boa biblioteca em C ou C++ que leia os dados, pode-se encapsular essa biblioteca com diversas técnicas, embora isso certamente dê muito mais trabalho e exija um conhecimento significativamente mais avançado para interagir com C ou C++.

### 6. Utilização de funções de biblioteca especiais

NumPy é a biblioteca fundamental para contêineres de arrays no conjunto de ferramentas de computação científica em Python. Muitas bibliotecas Python, incluindo SciPy, Pandas e OpenCV, usam ndarrays do NumPy como formato comum para troca de dados. Essas bibliotecas podem criar, manipular e trabalhar com arrays do NumPy.