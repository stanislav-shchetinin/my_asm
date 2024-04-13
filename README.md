# Translator + processor model for Assembler

- Щетинин Станислав Владимирович. Группа: P3208
- ` asm | stack | harv | mc | instr | binary | stream | port | cstr | prob2 | cache `
- Усложнение - кэш
- instr -> tick - т.к. взял вариант с усложнением
  
## Язык программирования
``` ebnf
<program> ::= <section_data> <section_text> | <section_text> <section_data> | <section_text>

<section_data> ::= "section .data\n" <declaration>*
<section_text> ::= "section .text\n" (<label> | <instruction>)*

<declaration> ::= <label> (<array> | <reserve>)
<instruction> ::= <label_arg_command> | <address_arg_command> | <number_arg_command> | <without_arg_command>

<label> ::= (<name> ":" (" ")*) | (<name> ":" (" ")* "\n")

<array> ::= (<array_element> "," (" ")*)* <array_element>+
<reserve> ::= "res" (" ")+ <number>
<array_element> ::= ("\"" <any_ascii> "\"" | <number>)

<label_arg_command> ::= ("push" | "jmp" 
| "jz" | "jnz" | "js" | "jns" | "call") (" ")+ <label>

<address_arg_command> ::= ("load" | "store") (" ")+ <address>

<number_arg_command> ::= ("push" | "cmp") (" ")+ <number>
<without_arg_command> ::= ("ldvatt" | "input" | "output" | "inc" | "hlt" | "pop" | "ldvtab" | "swap" | "add" | "sub" | "mul" | "div" | "ret")

<address> ::= "#" (1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)
<number> ::= [-2^64; 2^64 - 1]
<name> ::= ("_")* (<letter>)+
<letter> ::= [a-z] | [A-Z]
``` 

### Hello, world
```asm
section .data
hello_str: "Hello, world!"

section .text
_main:
    push hello_str
    cycle:
        load      ;загружаю на вершину стэка значение по адресу вершины стэка
        cmp 0     ;вычитаю из вершины стэка 0 - результат на вершине стэка
        jz ext    ;если на вершине 0, то переход
        pop       ;убрал ноль
        output 1  ;вывод по порту 1 (порт для вывода), 0 - порт для ввода
        pop       ;убрал букву
        inc       ;увеличил на один адрес
        jmp cycle ;безусловный переход
    
    ext:
        pop       ;убрал 0
        pop       ;убрал адрес нуля в памяти
        hlt       ;завершение

```