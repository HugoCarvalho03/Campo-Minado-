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

Dois loops são usados para iterar sobre todas as células da matriz. O loop externo (inicializar_loop_i) itera sobre as linhas e O loop interno (inicializar_loop_j) itera sobre as colunas.

Dentro do loop interno, o código calcula o deslocamento (offset) para acessar cada célula da matriz com base nos índices i e j. Isso é feito multiplicando o índice da linha ($s1) por 32 e o índice da coluna ($s2) por 4, e então somando os resultados.

O valor -2 é armazenado em cada célula da matriz usando a instrução sw (store word) e os contadores de linha e coluna ($s1 e $s2) são incrementados dentro dos loops.

Quando as iterações dos loops são concluídas, a função restaura o contexto original com restore_context e retorna com jr $ra.

# Função countAdjacentBombs

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/50df3f89-820f-49d6-aebf-ffe57e03bce5)

O código implementa a função countAdjacentBombs, que conta o número de bombas adjacentes a uma célula específica em um tabuleiro de jogo representado por uma matriz.
A função countAdjacentBombs começa com a instrução save_context para salvar o contexto atual da execução.

**1-** Os registradores $s0, $s1 e $s2 são usados para armazenar respectivamente o endereço base do tabuleiro, a linha e a coluna da célula para a qual desejamos contar as bombas adjacentes.

**2-** O registrador $s7 é usado como um contador para o número de bombas adjacentes encontradas.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/e8868baa-a963-41f6-8cb1-5ca390eeda08)

**3-** A função usa dois loops for, um para iterar sobre as linhas (for_list_i) e outro para iterar sobre as colunas (for_list_j) das células vizinhas à célula alvo.

**4-** Dentro dos loops for, são verificadas as condições de borda para garantir que as células vizinhas estejam dentro dos limites da matriz.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/7b20bd9c-7ce0-4bb0-949c-0f24827a60e7)

**5-** Para cada célula vizinha dentro dos limites da matriz, o código calcula o endereço da célula na matriz e carrega seu valor.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/cb704ee7-145a-4f7b-96b8-07f6ba10b4eb)


**6-** Se o valor carregado for -1 (que provavelmente indica a presença de uma bomba), o contador $s7 é incrementado.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/f02b33bb-31c1-45cc-bbf2-8af6fb06938f)


**7-** Após a conclusão da contagem, o valor final do contador é armazenado em $v0, que é o registrador de retorno.

**8-** Finalmente, o contexto original da execução é restaurado com restore_context, e a função retorna com jr $ra.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/402dea3e-0110-4540-9b09-aece1c479c02)


# Função plantBombs
A função checkVictory é responsável por determinar se todas as células válidas do tabuleiro foram reveladas, indicando que o jogo foi vencido.

A estrutura do código começa com a instrução save_context para preservar o contexto atual da execução. Em seguida, os registradores necessários são inicializados, incluindo $s0 para armazenar o endereço base do tabuleiro e $s7 como contador para o número de células reveladas.

Dois loops aninhados são utilizados para iterar sobre todas as células do tabuleiro. O primeiro loop (for_list_i) itera sobre as linhas, enquanto o segundo loop interno (for_list_j) itera sobre as colunas.

Dentro dos loops, o código calcula o endereço de cada célula no tabuleiro e carrega seu valor. Se o valor da célula for maior ou igual a zero (indicando que não é uma bomba), o contador $s7 é incrementado para acompanhar o número de células reveladas.

Após a contagem das células reveladas, o código verifica se o número total de células reveladas é igual ao número total de células válidas no tabuleiro (tamanho do tabuleiro menos o número de bombas). Se for o caso, define o registrador de retorno $v0 como 1, indicando vitória; caso contrário, define $v0 como 0, indicando que o jogo ainda não foi vencido.

Finalmente, o contexto original da execução é restaurado com restore_context, e a função retorna com jr $ra.

# Função checkVictory
![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/95d6ebbe-e58f-4e3b-95e8-fe5ea45ec9a0)

O código implementa a função checkVictory, que verifica se todas as células válidas do tabuleiro foram reveladas, o que indica que o jogo foi vencido.

**1-** A função checkVictory começa com a instrução save_context para salvar o contexto atual da execução.

**2-** O registrador $s0 é usado para armazenar o endereço base do tabuleiro.

**3-** O registrador $s7 é inicializado como um contador para o número de células reveladas.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/b7b6fa06-bc37-4c3e-b038-43b30bd60d80)

**4-** Dois loops são usados para iterar sobre todas as células do tabuleiro.

O loop externo (for_list_i) itera sobre as linhas.
O loop interno (for_list_j) itera sobre as colunas.

**5-** Dentro dos loops, o código calcula o endereço de cada célula no tabuleiro e carrega seu valor.
![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/998ca214-01d6-467a-946c-578f3f3512cf)

**6-** Se o valor da célula for maior ou igual a zero (indicando que não é uma bomba), o contador $s7 é incrementado.

**7-** Após a contagem das células reveladas, o código verifica se o número total de células reveladas é igual ao número total de células válidas no tabuleiro (tamanho do tabuleiro menos o número de bombas). Se for, define o registrador de retorno $v0 como 1 (indicando vitória); caso contrário, define $v0 como 0 (indicando que o jogo ainda não foi vencido).

**8-** O contexto original da execução é restaurado com restore_context, e a função retorna com jr $ra.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/cba98106-7412-46ca-914b-8c93ff55229a)


# Função revealNeighboringCells0
![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/77699fdd-4fbe-4c34-b6ed-dd33d76252a4)

A função revealNeighboringCells tem como objetivo revelar as células vizinhas à célula especificada (linha, coluna) no tabuleiro do jogo.

O código começa com a instrução save_context para salvar o contexto atual da execução.

Os registradores $s0, $s1 e $s2 são utilizados para armazenar, respectivamente, o endereço base do tabuleiro, o número da linha e o número da coluna da célula alvo.

O registrador $s7 é inicializado como contador para o número de células reveladas.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/a7ff9b03-766a-4fe6-9ead-2be32f711d61)

Dois loops  são utilizados para iterar sobre as células vizinhas à célula alvo. O primeiro loop (for_list_i) itera sobre as linhas e o segundo loop interno (for_list_j) itera sobre as colunas.

Dentro dos loops, o código verifica se a célula vizinha está dentro dos limites da matriz. Se estiver, o endereço da célula é calculado e o valor da célula é carregado.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/aa28b35b-7a68-40df-9346-47893351ca15)

Se o valor da célula for -2 (indicando que a célula ainda não foi revelada), a função countAdjacentBombs é chamada para contar o número de bombas adjacentes à célula atual. O resultado é armazenado no próprio tabuleiro.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/ea0206cc-aaf2-403b-9137-b55816d27a2e)

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/12ab5462-325d-43ec-80af-dc5b0275f4a8)

Se o número de bombas adjacentes for 0, o código realiza uma chamada recursiva para revealNeighboringCells para revelar as células vizinhas a essa célula.

Após o processamento de todas as células vizinhas, o contexto original da execução é restaurado com restore_context, e a função retorna com jr $ra.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/c7a4c4e2-22aa-4a50-ba6d-8dfa462348f7)

# Função play
A função play é responsável por processar a jogada do jogador em uma célula específica (linha, coluna) no tabuleiro do jogo.

O código começa com a instrução save_context para salvar o contexto atual da execução.![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/254b0a74-7f49-4840-95b9-1cda2f91c009)


Os registradores $s1 e $s2 são utilizados para armazenar o número da linha e o número da coluna da célula alvo, respectivamente.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/fc154bcb-6e00-40f0-89be-b1f465a1f009)

Em seguida, são calculados os deslocamentos necessários para acessar a célula alvo no tabuleiro. Isso é feito multiplicando o número da linha por 32 e o número da coluna por 4, que são os tamanhos de uma linha e uma célula, respectivamente, na representação do tabuleiro em memória.

O valor da célula alvo é então carregado para o registrador $t1.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/9fc1c508-5645-4a20-a6f0-756936f3fa6f)

Se o valor da célula for -1, isso indica que a célula já foi revelada, e a função retorna 0.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/d6e0c2f2-df51-49b6-913c-f0bf692c59f7)

Se o valor da célula for -2, indicando uma bomba, a função countAdjacentBombs é chamada para contar o número de bombas adjacentes. O resultado é armazenado na célula atual.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/f35b30d1-e24c-47a4-9a02-ad4c07f79e4a)

Se o valor retornado de countAdjacentBombs for 0, indicando que não há bombas adjacentes, a função revealNeighboringCells é chamada para revelar as células vizinhas.

![image](https://github.com/HugoCarvalho03/Campo-Minado-/assets/118187319/f5ac1fe2-1c3e-4a96-8f7a-948dc5f965a5)

Se nenhuma das condições acima for atendida, a função retorna 1, indicando que a célula foi revelada e possui bombas adjacentes.

Após o processamento da jogada, o contexto original da execução é restaurado com restore_context, e a função retorna com jr $ra.

![Uploading image.png…]()


# Função main
O código é um programa em assembly MIPS que implementa um jogo de campo minado.

O procedimento main é o ponto de entrada do programa. Ele inicializa o tabuleiro, coloca as bombas e inicia um loop principal onde o jogador faz suas jogadas.

Dentro do loop principal (begin_while), o programa solicita ao jogador as coordenadas (linha e coluna) para a jogada. Verifica se a jogada é válida (ou seja, dentro dos limites do tabuleiro). Se for válida, chama a função play para processar a jogada.

Depois de processar a jogada, o programa verifica se o jogador acertou uma bomba ou se todas as células válidas foram reveladas, determinando assim o resultado da partida.

Se o jogo terminar, seja por uma derrota ou uma vitória, uma mensagem correspondente é exibida e o loop principal é encerrado.

Ao final do jogo, o tabuleiro é impresso com todas as células reveladas, incluindo as bombas. Em seguida, o programa é encerrado com a syscall exit.

O programa faz uso de strings de mensagem armazenadas na seção .data para exibir mensagens ao jogador, como "Informe a linha para a jogada", "Parabéns! Você venceu!", etc. Estas mensagens são impressas na tela usando as syscall print_string.





































