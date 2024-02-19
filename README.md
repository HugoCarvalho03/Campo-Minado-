# Minesweeper - The game
 O"Minesweeper" (campo minado em português) é um jogo de tabuleiro que desafia os
 jogadores a descobrir células sem bombas em um campo minado. Os números revelados
 indicam quantas bombas estão adjacentes a uma determinada célula. O objetivo é
 descobrir todas as células seguras sem acionar uma bomba.
 Este projeto consiste em implementar uma versão em assembly MIPs do jogo
 minesweeper.
 
![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/d3cea0fd-9d53-4946-b1fa-bcd472959119)

# Função printBoard
Este código em assembly é responsável por imprimir o tabuleiro do jogo Campo Minado, exibindo todas as células reveladas, marcadas com bandeiras ou ocultas. Ele segue uma abordagem estruturada, utilizando loops para percorrer todas as células do tabuleiro e tomando decisões de impressão com base no conteúdo dessas células.

**1-** Salva o contexto atual, move os argumentos recebidos para os registradores $s0 e $s1, respectivamente, para acessá-los mais tarde e inicializa algumas variáveis e configurações necessárias.

**2-** Utiliza dois loops, um loop para imprimir os números das colunas e espaços entre eles e outro loop para imprimir os separadores entre as linhas.

**Impressão do Conteúdo do Tabuleiro:**
Utiliza dois loops aninhados para iterar sobre todas as células do tabuleiro.
Para cada célula, verifica se ela é uma bomba oculta, uma célula revelada ou uma célula marcada com bandeira e imprime o caractere correspondente.
A decisão de impressão é baseada no valor da célula e se a opção showBombs está ativada.
Os caracteres utilizados são: '*' para células não reveladas, números para células reveladas (indicando o número de bombas ao redor) e '#' para células marcadas com bandeira.

Após a impressão de todas as células, o código restaura o contexto anterior e utiliza a instrução jr $ra para retornar ao endereço de retorno.

# Função initializeBoard
O código é uma implementação da função initializeBoard, destinada a inicializar uma matriz representando um tabuleiro de um jogo. Cada célula do tabuleiro é inicializada com o valor -2, que significa "sem bomba".

A função initializeBoard começa com a instrução save_context para salvar o contexto atual da execução, geralmente salvando os registradores que serão modificados durante a execução da função.
O registrador $s0 é usado para armazenar o endereço base do tabuleiro, passado como argumento na função.
Os registradores $s1 e $s2 são inicializados como contadores para iteração sobre as linhas e colunas da matriz, respectivamente.

Dois loops aninhados são usados para iterar sobre todas as células da matriz. O loop externo (inicializar_loop_i) itera sobre as linhas e O loop interno (inicializar_loop_j) itera sobre as colunas.

Dentro do loop interno, o código calcula o deslocamento (offset) para acessar cada célula da matriz com base nos índices i e j. Isso é feito multiplicando o índice da linha ($s1) por 32 e o índice da coluna ($s2) por 4, e então somando os resultados.

O valor -2 é armazenado em cada célula da matriz usando a instrução sw (store word) e os contadores de linha e coluna ($s1 e $s2) são incrementados dentro dos loops.

Quando as iterações dos loops são concluídas, a função restaura o contexto original com restore_context e retorna com jr $ra.

# Função countAdjacentBombs
O código implementa a função countAdjacentBombs, que conta o número de bombas adjacentes a uma célula específica em um tabuleiro de jogo representado por uma matriz.
A função countAdjacentBombs começa com a instrução save_context para salvar o contexto atual da execução.

**1-** Os registradores $s0, $s1 e $s2 são usados para armazenar respectivamente o endereço base do tabuleiro, a linha e a coluna da célula para a qual desejamos contar as bombas adjacentes.

**2-** O registrador $s7 é usado como um contador para o número de bombas adjacentes encontradas.

**3-** A função usa dois loops for, um para iterar sobre as linhas (for_list_i) e outro para iterar sobre as colunas (for_list_j) das células vizinhas à célula alvo.

**4-** Dentro dos loops for, são verificadas as condições de borda para garantir que as células vizinhas estejam dentro dos limites da matriz.

**5-** Para cada célula vizinha dentro dos limites da matriz, o código calcula o endereço da célula na matriz e carrega seu valor.

**6-** Se o valor carregado for -1 (que provavelmente indica a presença de uma bomba), o contador $s7 é incrementado.

**7-** Após a conclusão da contagem, o valor final do contador é armazenado em $v0, que é o registrador de retorno.

**8-** Finalmente, o contexto original da execução é restaurado com restore_context, e a função retorna com jr $ra.

















