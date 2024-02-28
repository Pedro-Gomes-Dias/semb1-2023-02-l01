# Questionário Sistemas Embarcados I

## 1. Explique brevemente o que é compilação cruzada (***cross-compiling***) e para que ela serve.
O compilador cruzado é um compilador que consegue gerar um código executável para uma plataforma diferente da qual o compilador está sendo executado. Esse tipo de compilador é usado para compilar dentro de uma plataforma cuja qual não é capaz de sustentar o processo de compilação, como sistemas embarcados, microcontroladores, microprocessadores, entre outros, os quais não possuem um sistema operacional.

## 2. O que é um código de inicialização ou ***startup*** e qual sua finalidade?
Um código de incialização diz respeito a um conjunto de instruções de baixo nível que são executadas quando um sistema computacional é ligado ou quando um programa é iniciado. Este tipo de código tem como função principal realizar tarefas essenciais para a inicialização do sistema, sendo elas a configuração de hardware, inicilização de periféricos, definição de valores iniciais de registradores e memória, entre outras. Em sistemas embarcados, a existência do código de inicialização é indispensável, pois é ele que garante a configuração correta do hardware e também que o dispositivo esteja pronto para executar o software específico.

## 3. Sobre o utilitário **make** e o arquivo **Makefile responda**:

#### (a) Explique com suas palavras o que é e para que serve o **Makefile**.
O "Makefile" é um arquivo de configuração, utilizado em projetos de desenvolvimento de software, que automatiza o processo de compição e construção de programas. Ele é composto por um conjunto de regras e comandos que ditam como os diferentes componentes de software devem ser compilados e linkados para finalmente gerar o executável.
Dentre suas principais finalidades temos, a automatização de compilação, a gestão de dependências, a organização do projeto, a portabilidade do código-fonte e execução de algumas tarefas adicionais (limpeza de arquivos temporários, execução de testes automatizados, entre outros).

#### (b) Descreva brevemente o processo realizado pelo utilitário **make** para compilar um programa.
O processo de compilação de um programa geralmente envolve a criação de um arquivo "Makefile", o qual contém regras e instruções para a compilação do código principal.
Em resumo:
- O desenvolvedor cria um arquivo "Makefile" na mesma pasta do projeto que está desenvolvendo. 
- As regras contidas no arquivo "Makefile" são formadas por alvos (resultado desejado) e dependências (arquivos necessários para gerar o alvo). 
- Para cada regra, o arquivo "Makefile" inclui comandos que ditam como as deapendências devem ser compiladas e como o alvo final deve ser gerado. 
- Então, o desenvolvedor executa o comando "make" no terminal. Tal comando é responsável por ler o "Makefile", verificando as dependências e decidindo o que vai ser compilado.
- O "make" aciona os compiladores necessários para transformar o código-fonte em arquivos objeto (.o)
- Após a compilação, vêm a etapa da vinculação, onde o "make" vincula os arquivos objeto para, enfim, gerar o executável.
OBS : o comando "make" mantém o controle das datas de modificação dos arquivos fonte e objetos possibilitando recompilar, caso seja necessário, apenas as partes do código que foram alteradas desde a última compilação realizada.
#### (c) Qual é a sintaxe utilizada para criar um novo **target**?
- Exemplo de arquivo "Makefile" com um novo target

all: programa_test (target padrão)

programa_test: test1.c test2.c (novo target 'programa_test' com suas dependências e comandos)
    gcc -o programa_test test1.c test2.c

#### (d) Como são definidas as dependências de um **target**, para que elas são utilizadas?
As dependências de um target no "Makefile" são definidas para indicar ao "make" quais arquivos ou outros "targets" precisam estar presentes ou serem atualizados antes que o "target" atual seja construído. Essa definição de dependências serve para garantir que o target seja construído somente se suas dependências forem mais recentes do que o próprio target ou caso as dependências não existirem.

#### (e) O que são as regras do **Makefile**, qual a diferença entre regras implícitas e explícitas?
As regras são instruções que descrevem como transformar um ou mais arquivos-fonte em um target específico. As regras explícitas são diretamente definidas pelo programador dentro do "Makefile" e são responsáveis pelo controle sobre como os targets são construídos e quais comandos devem ser executados. Já as regras implícitas são regras genéricas que o "make" utiliza para deduzir como transformar certos tipos de arquivos em outros.

## 4. Sobre a arquitetura **ARM Cortex-M** responda:

### (a) Explique o conjunto de instruções ***Thumb*** e suas principais vantagens na arquitetura ARM. Como o conjunto de instruções ***Thumb*** opera em conjunto com o conjunto de instruções ARM?
O conjunto de instruções "Thumb" é uma extensão do conjunto de instruções ARM, projetado para proporcionar uma maior eficiência em termos de tamanho de código. Dentre suas principais vantagens temos:
- Instruções de 16 bits;
- Menor consumo de energia;
- Utilização otimizada de cache.
O conjunto de instruções "Thumb" opera em conjunto com o cojunto de instruções ARM através do "interworking", onde os processadores ARM são capazes de alternarem dinamicamente entre os modos "Thumb" e ARM durante o funcionamento do código pois ambas instruções compartilham o mesmo conjunto de registradores.

### (b) Explique as diferenças entre as arquiteturas ***ARM Load/Store*** e ***Register/Register***.
- Acesso à memória: a arquitetura ARM Load/Store contém instruções limitadas a operar apenas entre registradores e memória, utilizando os registradores como intermediários enquanto que a Register/Memory contém instruções que podem operar diretamente entre registradores e memória, sem a necessidade de registradores intermediários.
- Instruções típicas : na ARM Load/Store exemplos típicos incluem Load Register e Store Register. Já na Register/Memory exemplos típicos incluem  instruções de "LOAD" e "STORE" que podem operar diretamente com a memória.
- Complexidade do Código: a ARM Load/Store pode exigir mais instruções para realizar operações de carga e armazenamento, pois envolve o uso explícito de registradores intermediários. A Register/Memory pode simplificar o código em determinadas situações, sendo umas das principais quando instruções diretas de carga e armazenamento são suficientes.

### (c) Os processadores **ARM Cortex-M** oferecem diversos recursos que podem ser explorados por sistemas baseados em **RTOS** (***Real Time Operating Systems***). Por exemplo, a separação da execução do código em níveis de acesso e diferentes modos de operação. Explique detalhadamente como funciona os níveis de acesso de execução de código e os modos de operação nos processadores **ARM Cortex-M**.
Níveis de acesso de execução de código:
1.Thread (Nível de aplicação) : é o nível mais alto, onde o código da aplicação é executado. Cada aplicação, geralmente representada por uma thread, tem seu próprio espaço de endereçamento;
2.Handler (Nível de intervenção) : são rotinas de serviço de interrupçãp que respondem a eventos como interrupções e exceções. Existem handlers específicos para diferentes tipos de interrupções;
3.Kernel (Nível do sistemas operacional): é onde o kernel do sistema operacional é executado. O kernel é responsável por gerenciar o acesso concorrente aos recursos do sistema e fornece serviços essenciais para as threads, como escalonamento, gerenciamento de memória e de tarefas.

Modo de operação:
1.Thread Mode: é o modo padrão de execução de código de aplicação. As threads de aplicação normalmente operam neste modo.
2.Handler Mode: é um modo especial ativado em resposta a uma interrupção ou exceção. Handlers de interrupção/exceção executam nesse modo.
3.Priviliged Mode: esse modo geralmente é associado à execução do kernel do sistema operacional. O código que executa nesse modo tem acesso privilegiado a recursos do sistema.

### (d) Explique como os processadores ARM tratam as exceções e as interrupções. Quais são os diferentes tipos de exceção e como elas são priorizadas? Descreva a estratégia de **group priority** e **sub-priority** presente nesse processo.
Os processadores ARM Cortex-M possuem vários tipos de exceções, incluindo:
- HardFault: exceção causada por falhas graves, como violações de acesso à memória.
- Memory Management Fault: erros relacionados ao gerenciamento de memória.
- Bus Fault: erros relacionados ao barramento, como violações de alinhamento.
- Usage Fault: exceções relacionadas ao uso inadequado de instruções e registradores.

Esses processadores usam um sistema de priorização para lidar com exceções e interrupções, onde cada exceção e interrupção é associada a um número de prioridade, sendo 0 a prioridade mais alta. Além disso, existem os conceitos de "Group Priority" e "Sub-Priority".
- Group Priority : as exceções e interrupções são agrupadas em grupos, e cada grupo tem sua própria prioridade. O número de grupos e o número de exceções em cada grupo podem variar.
- Sub-Priority : dentro de cada grupo, as exceções e interrupções têm sub-prioridades. Essas sub-prioridades são usadas para priorizar exceções dentro do mesmo grupo.
Quanto a estratégia de resposta dos conceitos mencionados acima, temos que quando ocorre uma exceção ou interrupção, o processador Cortex-M primeiramente preserva o estado atual, depois carrega o vetor de interrupção e por fim executa a rotina de tratamento.

### (e) Qual a diferença entre os registradores **CPSR** (***Current Program Status Register***) e **SPSR** (***Saved Program Status Register***)?
O "CPSR" armazena o estado atual do processador, incluindo informações sobre flags (sinalizadores) condicionais, modo de execução, estado de interrupções e status de condição. Já o "SPSR" é usado para armazenar temporariamente o estado do "CPSR" quando ocorre uma exceção, permitindo que o processador retome seu estado anterior após o tratamento da exceção.

### (f) Qual a finalidade do **LR** (***Link Register***)?
Sua finalidade principal é armazenar o endereço de retorno para uma instrução de chamada de sub-rotina (branch and link). Quando uma sub-rotina é chamada, o endereço de retorno é armazenado no registrador "LR". Este endereço de retorno permite que o programa possa retornar à instrução que efetuou a chamada de sub-rotina.

### (g) Qual o propósito do Program Status Register (PSR) nos processadores ARM?
O PSR é frequentemente utilizado em conjunto com instruções condicionais e também é manipulado em situações como chamadas de sub-rotinas (branch and link) e interrupções. Além disso, durante exceções, o estado atual do PSR é geralmente salvo para que possa ser restaurado quando a exceção for tratada e a execução do programa normal for retomada.

### (h) O que é a tabela de vetores de interrupção?
É uma estrutura de dados em sistemas embarcados que mapeia endereços de memória para as rotinas de tratamento de interrupções e exceções. Ela é utilizada para direcionar o fluxo de controle do programa para as funções 
apropriadas quando ocorrem eventos como interrupções, exceções ou traps.
    
### (i) Qual a finalidade do NVIC (**Nested Vectored Interrupt Controller**) nos microcontroladores ARM e como ele pode ser utilizado em aplicações de tempo real?
Dentre as principais finalidade do NVIC temos a gestão de interrupções, a priorização de interrupções, o atendimento de interrupções aninhadas e o vetor de interrupção.
Quanto a sua utilização em aplicações de tempo real podemos dizer que ele possui uma rápida resposta a eventos, priorização para tarefas críticas, gestão eficiente de interrupções e controle de exceções, tornando-se uma poderosa ferramente para a programação de sistemas embarcados.

### (j) Em modo de execução normal, o Cortex-M pode fazer uma chamada de função usando a instrução **BL**, que muda o **PC** para o endereço de destino e salva o ponto de execução atual no registador **LR**. Ao final da função, é possível recuperar esse contexto usando uma instrução **BX LR**, por exemplo, que atualiza o **PC** para o ponto anterior. No entanto, quando acontece uma interrupção, o **LR** é preenchido com um valor completamente  diferente,  chamado  de  **EXC_RETURN**.  Explique  o  funcionamento  desse  mecanismo  e especifique como o **Cortex-M** consegue fazer o retorno da interrupção. 
O mecanismo de retorno de interrupção no Cortex-M envolve o uso do registrador "LR" (Link Register) durante a entrada e saída do tratamento de exceções (interrupções) e exceções relacionadas. O valor especial chamado "EXC_RETURN" é utilizado para indicar ao processador o contexto que deve ser restaurado durante o retorno de uma exceção.
Essa abordagem fornece uma maneira eficiente e flexível de gerenciar o retorno de exceções em microcontroladores Cortex-M, garantindo a restauração adequada do contexto de execução.

### (k) Qual  a  diferença  no  salvamento  de  contexto,  durante  a  chegada  de  uma  interrupção,  entre  os processadores Cortex-M3 e Cortex M4F (com ponto flutuante)? Descreva em termos de tempo e também de uso da pilha. Explique também o que é ***lazy stack*** e como ele é configurado. 
Enquanto o Cortex-M3 e o Cortex-M4F compartilham muitos conceitos e estruturas para tratamento de interrupções, as diferenças nos registradores disponíveis, especialmente a presença de registradores de ponto flutuante no Cortex-M4F, impactam o tempo e o uso da pilha durante o salvamento de contexto.
O conceito de "lazy stack" refere-se à estratégia de atrasar o salvamento dos registradores de ponto flutuante até que o processador saiba se a interrupção precisa ou não ser tratada. Se a interrupção for aceita, os registradores de ponto flutuante são salvos; caso contrário, são restaurados para o estado original sem a necessidade de terem sido salvos e restaurados desnecessariamente. Ele é configurado por meio de configurações específicas do sistema e do uso de flags no Program Status Register (PSR). O processador Cortex-M4F oferece suporte a essa estratégia, permitindo otimizar o desempenho durante o tratamento de interrupções que não envolvem cálculos de ponto flutuante.

## Referências

### Básicas

[1] [STM32F411xC Datasheet](https://www.st.com/resource/en/datasheet/stm32f411ce.pdf)

[2] [RM0383 Reference Manual](https://www.st.com/resource/en/reference_manual/rm0383-stm32f411xce-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

[3] [Using the GNU Compiler Collection (GCC)](https://gcc.gnu.org/onlinedocs/gcc/index.html)

[4] [GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

### Vídeos Microsoft Teams

[5] [05 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[6] [06 - Arquitetura Cortex-M Parte 1/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[7] [07 - Arquitetura Cortex-M Parte 2/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[8] [08 - Microcontroladores STM32](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[9] [10 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

### Material Suplementar

[5] [A General Overview of What Happens Before main()](https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/)
 
[6] [Bare metal embedded lecture-1: Build process](https://youtu.be/qWqlkCLmZoE?si=mn5yDnJYudQ1PpZH)
 
[7] [Bare metal embedded lecture-2: Makefile and analyzing relocatable obj file](https://youtu.be/Bsq6P1B8JqI?si=yuNLPj3JQ-2IT1yo)
 
[8] [Bare metal embedded lecture-3: Writing MCU startup file from scratch](https://youtu.be/2Hm8eEHsgls?si=c27MpZ47ApiMSwHR)
 
[9] [Lecture 15: Booting Process](https://youtu.be/3brOzLJmeek?si=MsHRUEJP8zofjwJQ)
