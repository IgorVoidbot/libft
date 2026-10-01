# Projeto 00 — libft: Recriando a Biblioteca Padrão C

## Visão Geral do Projeto & Objetivos

A linguagem C não possui estruturas de dados, algoritmos ou utilitários
prontos para uso. Todo programador que deseja dominar C precisa, em algum
momento, construir suas próprias ferramentas. É exatamente isso que este
projeto propõe: reescrever, do zero, um conjunto de funções essenciais da
biblioteca padrão (libc).

Os objetivos do projeto são:

- Compreender **como** as funções clássicas da libc funcionam internamente,
  e não apenas *o que* elas fazem.
- Dominar a manipulação de **ponteiros**, **arrays**, **strings** e
  **blocos de memória** em C.
- Aprender a alocar e liberar memória dinamicamente (`malloc`/`free`) com
  responsabilidade.
- Produzir uma biblioteca estática (`libft.a`) reutilizável, que será
  expandida e usada em todos os projetos futuros do currículo 42.

Ao final, você terá sua própria implementação das funções que usará pelo
resto da formação — e uma compreensão profunda do funcionamento da libc.

## Regras Gerais e Restrições de Compilação

### Makefile obrigatório

O projeto deve ser entregue com um `Makefile` que implemente, no mínimo, as
seguintes regras:

| Regra    | Comportamento esperado                                            |
|----------|-------------------------------------------------------------------|
| `all`    | Compila a biblioteca `libft.a` (regra padrão).                    |
| `clean`  | Remove os arquivos-objeto (`.o`) gerados.                         |
| `fclean` | Executa `clean` e remove o binário/arquivo `libft.a`.             |
| `re`     | Executa `fclean` seguido de `all` (recompilação completa).        |

O nome do arquivo de saída deve ser **`libft.a`** (biblioteca estática,
gerada com `ar`).

### Flags de compilação

A compilação de **todos** os arquivos-fonte deve utilizar, obrigatoriamente,
as seguintes flags:

```
-Wall -Wextra -Werror
```

com o compilador `gcc` ou `clang`. Nenhum *warning* é tolerado: qualquer
aviso interrompe a compilação e é considerado erro.

### Proibições e permissões

- **Permitido**: usar somente `<unistd.h>` e `<stdlib.h>` (e as funções que
  eles declaram), além de declarações próprias em um header do projeto
  (ex.: `libft.h`).
- **Proibido**: qualquer biblioteca externa (libc funcional de alto nível,
  funções utilitárias de terceiros, etc.) que implemente o comportamento
  pedido. O objetivo é escrever a lógica do zero.
- **Proibido**: declarar funções implicitamente; todo protótipo usado deve
  estar declarado no header do projeto.
- Todas as funções devem respeitar as convenções de nome `ft_*` e assinaturas
  exatas descritas na Parte 1.

## Parte 1: Protótipos das Funções Obrigatórias

Todas as funções abaixo devem ser implementadas com o **mesmo protótipo** e o
**mesmo comportamento** das originais da libc. Qualquer desvio de
comportamento (valor de retorno, efeitos colaterais, tratamento de erros)
será considerado falha.

### Manipulação de caracteres

| Função      | Protótipo                          | Comportamento esperado                                      |
|-------------|------------------------------------|-------------------------------------------------------------|
| `ft_isalpha`| `int ft_isalpha(int c);`           | Retorna diferente de zero se `c` é letra (A–Z, a–z).        |
| `ft_isdigit`| `int ft_isdigit(int c);`           | Retorna diferente de zero se `c` é dígito (0–9).            |
| `ft_isalnum`| `int ft_isalnum(int c);`           | Retorna diferente de zero se `c` é alfanumérico.            |
| `ft_isascii`| `int ft_isascii(int c);`           | Retorna diferente de zero se `c` pertence à tabela ASCII (0–127). |
| `ft_isprint`| `int ft_isprint(int c);`           | Retorna diferente de zero se `c` é imprimível (32–126).     |
| `ft_toupper`| `int ft_toupper(int c);`           | Converte minúscula em maiúscula; retorna `c` inalterado se não aplicável. |
| `ft_tolower`| `int ft_tolower(int c);`           | Converte maiúscula em minúscula; retorna `c` inalterado se não aplicável. |

### Strings e memória

| Função       | Protótipo                                  | Comportamento esperado                                              |
|--------------|--------------------------------------------|---------------------------------------------------------------------|
| `ft_strlen`  | `size_t ft_strlen(const char *s);`         | Retorna o número de caracteres de `s`, **sem** contar o `'\0'`.     |
| `ft_strcpy`  | `char *ft_strcpy(char *dst, const char *src);` | Copia `src` para `dst` (incluindo o `'\0'`) e retorna `dst`.    |
| `ft_strncpy` | `char *ft_strncpy(char *dst, const char *src, size_t n);` | Copia no máximo `n` caracteres; preenche com `'\0'` se `src` for menor que `n`. Retorna `dst`. |
| `ft_strlcpy` | `size_t ft_strlcpy(char *dst, const char *src, size_t size);` | Copia com garantia de terminação em `'\0'`; retorna o comprimento total de `src`. |
| `ft_strcmp`  | `int ft_strcmp(const char *s1, const char *s2);` | Compara lexicograficamente; retorna 0, negativo ou positivo. |
| `ft_strncmp` | `int ft_strncmp(const char *s1, const char *s2, size_t n);` | Compara no máximo `n` caracteres. |
| `ft_strchr`  | `char *ft_strchr(const char *s, int c);`   | Retorna ponteiro para a primeira ocorrência de `c` em `s`, ou `NULL`. |
| `ft_strrchr` | `char *ft_strrchr(const char *s, int c);`  | Retorna ponteiro para a **última** ocorrência de `c` em `s`, ou `NULL`. |
| `ft_strstr`  | `char *ft_strstr(const char *haystack, const char *needle);` | Localiza a primeira ocorrência da substring `needle` em `haystack`; retorna ponteiro ou `NULL`. |
| `ft_strdup`  | `char *ft_strdup(const char *s);`          | Aloca memória suficiente para uma cópia de `s` e a retorna.        |
| `ft_memset`  | `void *ft_memset(void *b, int c, size_t n);` | Preenche os primeiros `n` bytes de `b` com o byte `c`; retorna `b`. |
| `ft_bzero`   | `void ft_bzero(void *s, size_t n);`        | Zera os primeiros `n` bytes de `s` (equivalente a `memset(s, 0, n)`). |
| `ft_memcpy`  | `void *ft_memcpy(void *dst, const void *src, size_t n);` | Copia `n` bytes de `src` para `dst`; retorna `dst`. Não garante comportamento com regiões sobrepostas. |
| `ft_memmove` | `void *ft_memmove(void *dst, const void *src, size_t n);` | Copia `n` bytes mesmo com regiões sobrepostas; retorna `dst`. |
| `ft_memcmp`  | `int ft_memcmp(const void *s1, const void *s2, size_t n);` | Compara os primeiros `n` bytes; retorna 0, negativo ou positivo. |
| `ft_memchr`  | `void *ft_memchr(const void *s, int c, size_t n);` | Busca o byte `c` nos primeiros `n` bytes; retorna ponteiro ou `NULL`. |

### Alocação dinâmica

| Função       | Protótipo                                    | Comportamento esperado                                     |
|--------------|----------------------------------------------|------------------------------------------------------------|
| `ft_calloc`  | `void *ft_calloc(size_t count, size_t size);`| Aloca memória para `count` elementos de `size` bytes, **zerando** toda a região; retorna `NULL` se a alocação falhar. |
| `ft_atoi`    | `int ft_atoi(const char *str);`              | Converte a string inicial de `str` em inteiro, ignorando espaços iniciais e tratando sinal opcional. |

### Observações importantes

- O header do projeto (ex.: `libft.h`) deve conter os protótipos de **todas**
  as funções acima.
- Nenhuma das funções obrigatórias pode depender de chamadas às funções
  originais da libc que ela substitui.

## Parte 2: Casos de Borda e Gestão de Memória

A correção (Moulinette) testará ativamente os cenários abaixo. Sua
implementação deve lidar com todos eles sem *crash*, *undefined behavior* ou
vazamentos de memória.

### Ponteiros NULL

- **Comportamento indefinido no padrão, mas testado na prática**: muitas
  funções (ex.: `ft_strlen(NULL)`, `ft_strcpy(dst, NULL)`) não são
  especificadas para `NULL`. O esperado no projeto é que as funções sejam
  testadas com entradas válidas; contudo, sua implementação deve ser
  defensiva quando razoável — e, principalmente, **nunca** deve escrever em
  ponteiros `NULL` passados como destino.
- Funções de busca (`ft_strchr`, `ft_strstr`, `ft_memchr`) devem retornar
  `NULL` quando o elemento não for encontrado.

### Tamanhos zerados (`size_t n = 0`)

- `ft_memset`, `ft_bzero`, `ft_memcpy`, `ft_memmove`, `ft_memcmp` e
  `ft_memchr` com `n == 0` devem retornar de forma correta **sem tocar**
  nos ponteiros envolvidos.
- `ft_strncmp` com `n == 0` deve retornar `0`.
- `ft_strncpy` com `n == 0` não deve escrever nada em `dst`.
- `ft_calloc(0, size)` e `ft_calloc(count, 0)`: comportamento deve ser
  consistente — alocar um bloco válido (ou `NULL`) e, principalmente, nunca
  vazar memória.

### Falha no `malloc`

- `ft_strdup` e `ft_calloc` devem **verificar** o retorno de `malloc` e
  retornar `NULL` em caso de falha de alocação.
- Nenhum código deve tentar escrever em um ponteiro `NULL` retornado por
  `malloc`.

### Prevenção de *memory leaks*

- Toda memória alocada por `ft_strdup` e `ft_calloc` deve ser liberada com
  `free` pelo chamador nos testes.
- A biblioteca em si **não** deve alocar memória invisível, global ou
  persistente além da solicitada pelas funções de alocação.
- Testes com ferramentas como `valgrind` ou `-fsanitize=address` não devem
  reportar *leaks*, *invalid reads* nem *invalid writes*.

### Strings e buffers

- `ft_strlcpy` deve sempre terminar `dst` com `'\0'` quando `size > 0`.
- `ft_strncpy` deve preencher os bytes restantes com `'\0'` quando `src` é
  mais curta que `n`.
- Buffers de destino com tamanho insuficiente não podem causar escrita fora
  dos limites.
- `ft_memmove` deve funcionar corretamente quando `dst` e `src` se sobrepõem
  (inclusive com `dst > src`).

## Critérios de Avaliação da Moulinette

### Checklist de testes automatizados

- [ ] O `Makefile` compila `libft.a` com as flags `-Wall -Wextra -Werror`
      sem nenhum *warning*.
- [ ] As regras `all`, `clean`, `fclean` e `re` funcionam exatamente como
      especificado.
- [ ] Todos os protótipos batem exatamente com os da libc (tipos de retorno,
      ordem e tipos dos parâmetros).
- [ ] Cada função obrigatória passa nos testes de comportamento idêntico à
      libc (comparação direta com a função original para entradas variadas,
      incluindo caracteres fora da tabela ASCII e valores negativos).
- [ ] Casos de borda: strings vazias (`""`), tamanho `0`, ponteiros para
      buffers de 1 byte, valores de `char` com sinal, `INT_MIN`/`INT_MAX`
      em `ft_atoi`.
- [ ] `ft_strdup` e `ft_calloc` retornam `NULL` (ou comportamento
      consistente) em cenários de falha de alocação.
- [ ] Nenhum vazamento de memória detectado por `valgrind`/ASan.
- [ ] A biblioteca é estática e pode ser vinculada com um programa externo:
      `gcc main.c -L. -lft` deve funcionar.

### Critérios para nota máxima

1. **Conformidade estrita**: comportamento byte a byte idêntico ao da libc
   para todas as funções obrigatórias.
2. **Zero warnings** com `-Wall -Wextra -Werror`.
3. **Código limpo e idiomático**: funções curtas, sem variáveis globais,
   sem números mágicos e com nomes descritivos.
4. **Gestão de memória impecável**: sem *leaks*, sem acessos inválidos,
   tratamento correto de falha de alocação.
5. **Makefile robusto**: relink evitado quando desnecessário, regras
   funcionando em qualquer ordem e em recompilações parciais.
6. **Norma respeitada**: estilo e organização de código de acordo com a
   norma vigente da escola (indentação, tamanho de função, limites de
   linhas, etc.).

> **Nota**: este documento contém apenas a especificação do projeto. A
> implementação das funções é de responsabilidade do cadete e não deve
> constar aqui.
