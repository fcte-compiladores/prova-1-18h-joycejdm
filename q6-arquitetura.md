# Etapas de um Compilador

De modo abstrato, um compilador é um programa que converte código de uma linguagem para outra. Podemos representá-lo como uma função do tipo:

python
compilador(str) -> str | bytes


No caso de compiladores que emitem código de máquina ou bytecode, a saída normalmente é em formato binário (bytes), mas a ideia básica continua a mesma.

O processo de compilação pode ser dividido nas seguintes etapas:

python
def compilador(x1: str) -> str | bytes:
    x2 = lex(x1)        # análise léxica
    x3 = parse(x2)      # análise sintática
    x4 = analysis(x3)   # análise semântica
    x5 = optimize(x4)   # otimização
    x6 = codegen(x5)    # geração de código
    return x6


---

## lex(str) -> list[token]

*Análise léxica*

Transforma o código fonte (string) em uma lista de *tokens*, que são as unidades léxicas da linguagem:

- Palavras-chave: if, while
- Identificadores: nomes de variáveis
- Operadores: +, *, ==
- Delimitadores e símbolos: (, ), {, }

*Exemplo:*  
Código: if x > 0  
Tokens: [IF, IDENT("x"), GT, NUMBER(0)]

---

## parse(list[token]) -> AST

*Análise sintática*

Recebe a lista de tokens e constrói uma *Árvore Sintática Abstrata (AST)*, que representa a estrutura hierárquica do código.  
Verifica se a sequência de tokens está de acordo com a *gramática da linguagem*.

*Exemplo:*  
Código: x = 3 + 1  
AST: Assign(name='x', value=Add(Literal(3), Literal(1)))

---

## analysis(AST) -> AST anotada

*Análise semântica*

Garante que a AST representa um programa *coerente* em termos de lógica e significado.  
Verifica:
- Declaração de variáveis
- Tipos compatíveis
- Regras de escopo

A AST pode ser enriquecida com informações como tipo de variáveis, símbolo associado, etc.

*Exemplo:*  
Garante que x = y + 1 seja válido apenas se y estiver declarado e for numérico.

---

## optimize(AST) -> AST otimizada

*Otimização*

Melhora a AST sem alterar o comportamento do programa, gerando código mais eficiente.

Algumas técnicas comuns:
- Constante folding: 2 + 3 → 5
- Eliminação de código morto
- Simplificação de expressões

*Exemplo:*  
Transformar x = 2 + 3 diretamente em x = 5.

---

## codegen(AST) -> bytes

*Geração de código*

Converte a AST final em *código executável* ou *bytecode*.

Pode gerar:
- Código de máquina (ex: .exe, .o)
- Bytecode para máquinas virtuais (ex: .class do Java, .pyc do Python)

*Exemplo:*  
- gcc gera binários para processadores a partir de código C  
- javac gera .class para a JVM