get_next_line:

Leitura linha-a-linha de um file descriptor — implementação em C (versão baseada no Get Next Line v13 da 42).
Este repositório contém a minha implementação de get_next_line(int fd) e um conjunto mínimo de funções utilitárias (get_next_line_utils.c) usadas por ela.

O que ele faz:

get_next_line lê o fd passado e devolve uma linha por chamada:

Retorna a linha incluindo o \n quando a linha termina com nova-linha.

Retorna a última linha (sem \n) quando o ficheiro termina sem \n.

Retorna NULL em caso de erro ou quando não há mais linhas a ler (EOF).

A função mantém estado entre chamadas usando uma variável static interna para guardar o “resto” lido.

O chamador é responsável por free() da string retornada quando não precisar dela.

Arquivos:

get_next_line.h — header com os protótipos e #ifndef / #define guard.

get_next_line.c — implementação da função get_next_line.

get_next_line_utils.c — implementação das 5 funções utilitárias usadas:

- ft_strlen(const char *str) — contar tamanho de string.

- ft_substr(char const *s, unsigned int start, size_t len) — criar substring.

- ft_strjoin(char const *s1, char const *s2) — concatenar duas strings (retorna nova string).

- ft_strdup(const char *s) — duplicar string.

- ft_strchr(const char *s, int c) — procurar caractere em string.

Protótipos do header:

char	*get_next_line(int fd);
char	*ft_strchr(const char *s, int c);
char	*ft_strdup(const char *s);
char	*ft_substr(char const *s, unsigned int start, size_t len);
size_t	ft_strlen(const char *str);
char	*ft_strjoin(char const *s1, char const *s2);

Comportamento importante e pontos de implementação:

A implementação interna utiliza helpers estáticos:

- memorize_line: junta o buffer lido ao str acumulado (com ft_strdup / ft_strjoin).

- line_with_nl: extrai a linha até o \n e atualiza o str com o resto.

- make_line: lida com retornos de read (n < 0 -> erro) e monta a linha final.

- safe_free_dp: libera ponteiro com segurança.

- BUFFER_SIZE é uma macro que determina quantos bytes são lidos por chamada ao read. Compile com -D BUFFER_SIZE=32 (ou outro valor).

Proteções:

Se fd < 0 ou BUFFER_SIZE <= 0 retorna NULL.

Em caso de falha de malloc/ft_* a função tenta libertar o que for preciso e retorna NULL sem vazamento.

ENGLISH VERSION

get_next_line:

Line-by-line reading from a file descriptor — C implementation (based on Get Next Line v13 from 42).
This repository contains my implementation of get_next_line(int fd) and a minimal set of utility functions (get_next_line_utils.c) used by it.

What it does:

get_next_line reads from the given fd and returns one line per call:

Returns the line including the \n when the line ends with a newline.

Returns the last line (without \n) when the file ends without a final newline.

Returns NULL on error or when there are no more lines to read (EOF).

The function preserves state between calls using an internal static variable to keep the leftover data.

The caller is responsible for free()-ing the returned string when it is no longer needed.

Files:

get_next_line.h — header with prototypes and include guard (#ifndef / #define).

get_next_line.c — implementation of the get_next_line function.

get_next_line_utils.c — implementation of the five utility functions used:

- ft_strlen(const char *str) — count string length.

- ft_substr(char const *s, unsigned int start, size_t len) — create a substring.

- ft_strjoin(char const *s1, char const *s2) — concatenate two strings (returns a new string).

- ft_strdup(const char *s) — duplicate a string.

- ft_strchr(const char *s, int c) — search for a character in a string.

Header prototypes:

char	*get_next_line(int fd);
char	*ft_strchr(const char *s, int c);
char	*ft_strdup(const char *s);
char	*ft_substr(char const *s, unsigned int start, size_t len);
size_t	ft_strlen(const char *str);
char	*ft_strjoin(char const *s1, char const *s2);


Important behavior and implementation notes:

The internal implementation uses static helper functions:

- memorize_line: appends the read buffer to the accumulated str (using ft_strdup / ft_strjoin).

- line_with_nl: extracts the line up to \n and updates str with the remainder.

- make_line: handles read return values (n < 0 → error) and builds the final returned line.

- safe_free_dp: safely frees a pointer.

- BUFFER_SIZE is a macro that determines how many bytes are read per read call. Compile with -D BUFFER_SIZE=32 (or another value).

Guards:

If fd < 0 or BUFFER_SIZE <= 0 the function returns NULL.

On malloc / helper failure the function attempts to free any allocated memory and returns NULL without leaking.

