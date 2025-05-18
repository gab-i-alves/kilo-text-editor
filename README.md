# Construindo um Editor de Texto

*Esse repositório inclui o código feito seguindo o tutorial disponível [aqui](https://viewsourcecode.org/snaptoken/kilo/index.html) + um README com todas as anotações e dúvidas que surgiram durante o desenvolvimento do projeto.*

## Progresso

- **D-1 [18/05/2025]**: Capítulos 1 (Setup) e 2 (Entering raw mode)


### Anotações do Capítulo 2
#### Conceitos Fundamentais de Programação de Terminal em Modo Raw

##### Modo Terminal
- **Modo Canônico (cooked mode)**: O modo padrão do terminal onde a entrada é processada linha por linha e enviada ao programa apenas após pressionar Enter.
- **Modo Raw**: Modo onde cada tecla é enviada imediatamente ao programa sem processamento intermediário do sistema operacional.
- **Bufferização**: No modo canônico, a entrada é armazenada em buffer até que uma linha completa seja enviada, enquanto no modo raw não há bufferização.

##### Estrutura termios
- **Definição**: Estrutura de dados que contém todas as configurações do terminal no Unix/Linux.
- **Componentes Principais**:
  - `c_iflag`: Flags de controle de entrada (input flags)
  - `c_oflag`: Flags de controle de saída (output flags)
  - `c_cflag`: Flags de controle (control flags)
  - `c_lflag`: Flags locais (local flags) - controlam o comportamento do terminal
  - `c_cc`: Array de caracteres de controle especiais (como EOF, EOL)

##### Flags de Terminal Importantes
- **ECHO**: Controla se os caracteres digitados são exibidos na tela.
- **ICANON**: Ativa/desativa o modo canônico de leitura (linha a linha).
- **ISIG**: Controla se sinais como Ctrl+C (SIGINT) são gerados.
- **IXON**: Controla o fluxo de controle XON/XOFF (Ctrl+S/Ctrl+Q).
- **OPOST**: Controla o processamento de saída.
- **BRKINT**: Define se o sinal BREAK causa um SIGINT.
- **ICRNL**: Controla a tradução de retorno de carro para nova linha.
- **IEXTEN**: Controla funcionalidades estendidas (como Ctrl+V).

##### Funções de Controle de Terminal
- **tcgetattr()**: Obtém os atributos atuais do terminal.
- **tcsetattr()**: Define novos atributos para o terminal.
- **TCSAFLUSH**: Flag que aplica mudanças após processar toda entrada pendente.

##### Tratamento de Erros
- **errno**: Variável global que contém o código do último erro ocorrido.
- **perror()**: Função que imprime mensagem de erro descritiva baseada em errno.
- **exit()**: Função que termina o programa com um código de retorno específico.

##### Caracteres de Controle
- **iscntrl()**: Função que verifica se um caractere é um caractere de controle.
- **Caracteres não imprimíveis**: Caracteres com códigos ASCII entre 0 e 31, como Tab, Enter, Escape.

##### Entrada e Saída
- **STDIN_FILENO**: Constante que representa o descritor de arquivo para entrada padrão (geralmente 0).
- **read()**: Função de baixo nível para ler dados de um descritor de arquivo.
- **printf()**: Função para enviar saída formatada para o terminal.

##### Configurações de Timeout
- **VMIN**: Controla o número mínimo de bytes para retornar de uma chamada read().
- **VTIME**: Define o tempo de espera em décimos de segundo para uma operação de leitura.

##### Limpeza de Recursos
- **atexit()**: Registra uma função para ser chamada automaticamente quando o programa termina.
- **Restauração de estado**: Importância de restaurar o terminal ao estado original após modificações.

##### Caracteres Especiais no Terminal
- **\r\n**: Sequência de retorno de carro e nova linha usada para compatibilidade entre sistemas.
- **Sequências de Escape**: Séries de caracteres que começam com o caractere ASCII 27 (ESC), usadas para controlar o terminal.