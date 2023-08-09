section .data
    mensagem db "Digite o CPF a ser verificado: ", 0
    buffer db 13      ; Buffer para armazenar a entrada (tamanho máximo de 12 + 1 para o newline)
    buffer_size equ $ - buffer

section .bss
    cpf resb 12       ; Variável para armazenar o CPF (12 bytes)

section .text
    global _start

_start:
    ; Escreve a mensagem para solicitar o CPF
    mov eax, 4        ; sys_write
    mov ebx, 1        ; file descriptor 1 (stdout)
    mov ecx, mensagem
    mov edx, buffer_size
    int 0x80          ; Chamada de sistema

    ; Lê a entrada do usuário para o buffer
    mov eax, 3        ; sys_read
    mov ebx, 0        ; file descriptor 0 (stdin)
    mov ecx, buffer
    mov edx, buffer_size
    int 0x80          ; Chamada de sistema

    ; Copia o CPF do buffer para a variável cpf
    mov edi, cpf      ; Destino (edi = endereço da variável cpf)
    mov esi, buffer   ; Origem (esi = endereço do buffer)
    mov ecx, 12       ; Tamanho da cópia
    cld               ; Define a direção de cópia para frente
    rep movsb         ; Copia os bytes

    ; código para processar o CPF
calcular_digitos_verificadores:

    ; Calcula o primeiro dígito verificador
    mov edi, cpf
    mov ecx, 9        ; Apenas os 9 primeiros dígitos
    xor eax, eax      ; Zera o acumulador
.loop1:
    mov al, byte [edi + ecx - 1]   ; Carrega o dígito
    sub al, '0'        ; Converte de ASCII para número
    imul al, cl        ; Multiplica pelo peso (9 a 2)
    add [esp + 4], al  ; Acumula o resultado
    loop .loop1

    mov edx, 0         ; Limpa o edx
    mov eax, [esp + 4] ; Move o resultado para eax
    div byte 11        ; Divide por 11
    mov byte [cpf + 9], dl ; Armazena o dígito verificador

    ; Calcula o segundo dígito verificador
    mov edi, cpf
    mov ecx, 10        ; Apenas os 10 primeiros dígitos
    xor eax, eax       ; Zera o acumulador
.loop2:
    mov al, byte [edi + ecx - 1]   ; Carrega o dígito
    sub al, '0'        ; Converte de ASCII para número
    imul al, cl        ; Multiplica pelo peso (10 a 2)
    add [esp + 4], al  ; Acumula o resultado
    loop .loop2

    mov edx, 0         ; Limpa o edx
    mov eax, [esp + 4] ; Move o resultado para eax
    div byte 11        ; Divide por 11
    mov byte [cpf + 10], dl ; Armazena o dígito verificador
    ; Converte os dígitos verificadores calculados para caracteres
    movzx eax, byte [cpf + 9]  ; Primeiro dígito verificador calculado
    add al, '0'                ; Converte para ASCII
    mov [cpf + 9], al

    movzx eax, byte [cpf + 10] ; Segundo dígito verificador calculado
    add al, '0'                ; Converte para ASCII
    mov [cpf + 10], al

    ; Compara os dígitos verificadores calculados com os dígitos finais do CPF
    cmp byte [cpf + 9], byte [cpf + 9 + 2]  ; Compara o primeiro dígito
    jne .invalido  ; Pula para o rótulo .invalido se for diferente

    cmp byte [cpf + 10], byte [cpf + 10 + 2] ; Compara o segundo dígito
    jne .invalido  ; Pula para o rótulo .invalido se for diferente

    ; Imprime a mensagem de CPF válido
    mov eax, 4       ; sys_write
    mov ebx, 1       ; file descriptor 1 (stdout)
    mov ecx, mensagem_valido
    mov edx, 13      ; Tamanho da mensagem
    int 0x80         ; Chamada de sistema
    jmp .fim

.invalido:
    ; Imprime a mensagem de CPF inválido
    mov eax, 4       ; sys_write
    mov ebx, 1       ; file descriptor 1 (stdout)
    mov ecx, mensagem_invalido
    mov edx, 12      ; Tamanho da mensagem
    int 0x80         ; Chamada de sistema


    ; Terminar o programa
    mov eax, 1        ; sys_exit
    xor ebx, ebx      ; Código de saída 0
    int 0x80          ; Chamada de sistema

