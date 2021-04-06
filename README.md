### Comandos do Linux - Remover todo o texto após X

# 1. Visão Geral
Existem várias ocasiões em que podemos querer remover o texto após um caractere específico ou conjunto de caracteres. Por exemplo, um cenário típico é quando queremos remover a extensão de um nome de arquivo específico.

Neste tutorial rápido, vamos explorar várias abordagens para ver como podemos manipular strings para remover texto após um determinado padrão. Usaremos o shell Bash em nossos exemplos, mas esses comandos também podem funcionar em outros shells POSIX.

# 2. Manipulação de String Nativa
Vamos começar dando uma olhada em como podemos remover texto usando algumas das operações de manipulação de string integradas oferecidas pelo Bash. Para isso, vamos usar um recurso do shell chamado expansão de parâmetro.

Para recapitular rapidamente, a expansão do parâmetro é o processo em que o Bash expande uma variável com um determinado valor. Para conseguir isso, simplesmente usamos um cifrão seguido por nosso nome de variável, entre colchetes:

```
my_var="Hola Mundo"
echo ${my_var}
```

Como esperado, o exemplo acima resulta na saída:

```
Hola Mundo
```

Mas, como veremos durante o processo de expansão, também podemos modificar o valor da variável ou substituí-lo por outros valores.

Agora que entendemos os fundamentos da expansão de parâmetro, nas próximas subseções, explicaremos várias maneiras diferentes de como excluir partes de nossa variável.

Em todos os nossos exemplos, vamos nos concentrar em um caso de uso bastante simples para remover a extensão de um nome de arquivo.

### 2.1. Extração de caracteres usando uma determinada posição e comprimento
Começaremos vendo como extrair uma substring de um comprimento específico usando uma determinada posição inicial:

```
my_filename="interesting-text-file.txt"
echo ${my_filename:0:21}
```

Isso nos dá a saída:

```
interesting-text-file
```

Neste exemplo, estamos extraindo uma string da variável my_filename. Começando na posição 0 e com comprimento de 21 caracteres. Na verdade, estamos dizendo para remover todo o texto após a posição 21 que, neste caso, é a extensão .txt.

Embora essa solução funcione, existem algumas desvantagens óbvias:

Nem todos os nomes de arquivos terão o mesmo comprimento
Precisamos calcular onde começa a extensão do arquivo para tornar esta uma solução mais dinâmica
A olho nu, não é muito intuitivo o que o código está realmente fazendo

No próximo exemplo, veremos uma solução mais elegante.

### 2.2. Excluindo a correspondência mais curta
Agora veremos como podemos excluir a correspondência de substring mais curta do verso de nossa variável:

```
echo ${my_filename%.*}
```

Vamos explicar com mais detalhes o que estamos fazendo no exemplo acima:

- Usamos o caractere ‘%’, que tem um significado especial e é removido da parte de trás da string;
- Então usamos ‘. *’ Para combinar a substring que começa com um ponto;
- Em seguida, executamos o comando echo para gerar o resultado dessa manipulação de substring.

Novamente, excluímos a substring ‘.txt’ resultando na saída:

```
interesting-text-file
```

### 2.3. Excluindo a correspondência mais longa
Da mesma forma, também podemos excluir a correspondência de substring mais longa de nosso nome de arquivo. Agora vamos imaginar que temos um cenário um pouco mais complicado, onde o nome do arquivo tem mais de uma extensão:

``` 
complicated_filename="hello-world.tar.gz"
echo ${complicated_filename%%.*}
```

Nesta variação, ‘%%. *’ Remove a correspondência mais longa para ‘. *’ Da parte de trás de nossa variável confused_filename. Isso simplesmente corresponde a “.tar.gz” resultando em:

```
hello-world
```

### 2.4. Usando Localizar e Substituir
Neste exemplo final de manipulação de string, veremos como usar os recursos internos de localização e substituição do Bash:

```
echo ${my_filename/.*/}
```

Para entender este exemplo, vamos primeiro entender a sintaxe da substituição de substring:

```
${string/substring/replacement}
```

Agora, para colocar isso em contexto, estamos substituindo a primeira correspondência de ‘. *’ Na variável my_filename e substituindo-a por uma string vazia. Nesse caso, removemos novamente a extensão.

# 3. Usando o Comando sed
Nesta penúltima seção, veremos como podemos usar o comando sed. O comando sed é um editor de fluxo poderoso que podemos usar para realizar transformações de texto básicas e complexas.

Usando este comando, podemos encontrar um padrão e substituí-lo por outro padrão. Quando o marcador de posição de substituição é deixado vazio, o padrão é excluído.

De acordo com nosso outro exemplo, vamos simplesmente repetir o nome do nosso arquivo depois de remover a extensão:

```
echo 'interesting-text-file.txt' | sed 's/.txt*//'
```

Neste exemplo, em vez de atribuir nosso nome de arquivo a uma variável, começamos canalizando-o para o comando sed.

O padrão que procuramos é ‘.txt *’ e como a parte de substituição do comando é deixada em branco, ela é removida do nome do arquivo. Novamente, o resultado é simplesmente ecoar o valor do nome do arquivo sem a extensão.

# 4. Usando o comando cut
Neste exemplo final, vamos explorar o comando cut. Como o nome sugere, podemos usar o comando cut para cortar seções do texto:

```
echo 'interesting-text-file.txt' | cut -f1 -d"."
```

Vamos dar uma olhada no comando em mais detalhes para entendê-lo corretamente:

- Primeiro usamos a opção -f para especificar o número do campo que indica o campo a extrair;
- A opção -d é usada para especificar o separador ou delimitador de campo, neste exemplo um ‘.’
Os campos de saída são separados por uma única ocorrência do caractere delimitador de campo. Isso significa que em nosso exemplo terminamos com dois campos divididos por um ponto. Consequentemente, selecionamos o primeiro e, no processo, descartamos a extensão ‘.txt’.

# 5. Conclusão
Neste tutorial rápido, descrevemos algumas maneiras que nos ajudam a remover texto de uma string.

Primeiro, exploramos a manipulação de string nativa usando expansão de parâmetro. Mais tarde, vimos um exemplo com o comando de edição de fluxo de energia sed. Em seguida, mostramos como poderíamos obter resultados semelhantes usando o comando cut.

