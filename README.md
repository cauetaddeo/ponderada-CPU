# ponderada-CPU


# Arquitetura de CPU

## Descrição do Projeto
Este projeto implementa e simula o caminho de dados (datapath) de uma Unidade Central de Processamento (CPU) simples. O circuito demonstra o ciclo clássico de execução de hardware computacional: **Busca (Fetch), Decodificação (Decode) e Execução (Execute)**, operando de forma estritamente sincronizada através de um sinal de relógio.

## O Ciclo de Execução
A CPU opera em um ciclo contínuo e ordenado:
1. **Busca:** O endereço da próxima instrução é gerado e a instrução correspondente é lida da memória.
2. **Decodificação:** A instrução é estabilizada e fatiada logicamente entre sua operação e seu alvo.
3. **Execução:** A operação matemática ou de movimentação de dados é processada e o resultado final é armazenado.

## Componentes do Sistema

### 1. Sincronização e Memória Base
* **Clock (Relógio - Clk):** É o sinal de sincronização de todo o sistema. Emite pulsos elétricos em intervalos regulares. Nenhuma operação avança para o próximo estado sem a recepção de um pulso de Clock.
* **Flip-Flop Tipo D:** Circuito lógico fundamental para armazenamento de 1 bit. O valor na entrada de dados (D) só é transferido e fixado na saída (Q) no instante exato em que ocorre o pulso de sincronização no pino (C). Componentes como este formam a base dos registradores do processador.

### 2. Fase de Busca (Fetch)
* **Program Counter (Contador de Programa - PC):** Circuito de incremento automático responsável por ditar a sequência de execução. Internamente, possui um Registrador (que guarda o endereço atual e o fornece na saída *Addr*) e um Somador lógico. O Somador recebe esse endereço, adiciona a constante 1, e devolve o novo valor à entrada do Registrador. A cada pulso do relógio, o PC avança sequencialmente (0, 1, 2, 3...) apontando sempre para a instrução seguinte.
* **ROM (Memória de Instruções):** Memória de leitura não-volátil que armazena o código-fonte binário do programa. Ela recebe o endereço de leitura apontado pelo PC e retorna a instrução completa armazenada naquela posição de memória.

### 3. Fase de Decodificação (Decode)
* **Instruction Register (Registrador de Instrução - IR):** Recebe a instrução binária bruta lida da ROM e a estabiliza durante todo o ciclo atual de processamento.
* **Separador de Barramento (Splitter):** Divide o bloco de bits da instrução em duas frentes independentes:
    * **Opcode (Código de Operação):** A parcela de bits (ex: bits 8-10) que define qual operação será executada (ex: somar, subtrair, mover).
    * **Operando:** A parcela de bits (ex: bits 0-7) que carrega o dado numérico direto ou o endereço do dado que será afetado pela operação.

### 4. Fase de Execução (Execute)
* **ALU (Unidade Lógica Aritmética):** O núcleo de processamento do circuito. Recebe o Opcode definido e o Operando numérico associado. A ALU efetua o cálculo lógico ou aritmético exigido pela instrução e fornece o valor resultante.
* **Registradores de Saída (Ac e Mq):** Armazenam os resultados processados pela ALU. O **Acumulador (Ac)** retém os resultados primários das operações rotineiras. O registrador **Mq (Multiplier-Quotient)** atua como armazenamento auxiliar em arquiteturas que processam multiplicações, divisões ou extensões de bits.

## Estrutura das Instruções
O processador processa padrões binários. O Opcode e o Operando em conjunto formam instruções lógicas, que se categorizam majoritariamente em:
* **Aritméticas e Lógicas:** Operações de cálculo direcionadas à ALU (Ex: `ADD 5` instrui o sistema a somar o valor 5 ao conteúdo atual do Acumulador).
* **Movimentação de Dados:** Operações de leitura/escrita na memória (Ex: `LOAD 10` copia o dado contido no endereço 10 da memória para dentro da CPU).
* **Controle de Fluxo (Desvios):** Instruções que alteram forçosamente o valor do Program Counter, quebrando a sequência lógica de leitura para criar loops ou condicionais (Ex: `JUMP 50` força o PC a iniciar a próxima leitura na linha 50).

## Validação via Diagrama de Tempo
A validação do processador em simulação é feita analisando as saídas lógicas em resposta aos pulsos do Clock ao longo do tempo. Os valores geralmente são exibidos no sistema **Hexadecimal** (base 16), padronizado pelo prefixo `0x`, por sua eficiência em agrupar bits de 4 em 4. Letras de A a F assumem os valores de 10 a 15.

Cálculo prático das saídas da simulação:
* **Valor `0x14`:** Convertendo para a base 10 (decimal), multiplica-se 1 por 16 e soma-se com 4. Resultado: o acumulador registrou o valor decimal 20.
* **Valor `0xFF`:** Convertendo para decimal, o 'F' vale 15. (15 * 16) + 15 resulta no valor decimal 255. Este é o limite numérico máximo que pode ser representado utilizando 8 bits de processamento.


# Vídeo explicando:

[Vídeo Ponderada - CPU de 8 bits](https://youtu.be/aX-4K79M9BQ)
