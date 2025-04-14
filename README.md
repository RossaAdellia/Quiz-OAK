# Quiz-OAK

Materi: basic/09_instruksi_logika/instruksi_and.asm
![Cuplikan layar 2025-04-14 145317](https://github.com/user-attachments/assets/1757cf0d-1c19-4c52-9a13-f745d6c0cd97)

# Penjelasan Kode

section .text
global _start

1. section .text: Bagian ini berisi kode program (instruksi CPU).

2. global _start: Menandai label _start agar dikenali sebagai entry point oleh linker. _start adalah titik awal eksekusi program di Linux.

3. # Bagian Utama Program
_start:

    mov ax, 8h
    and ax, 1
    jz  evnn

1. mov ax, 8h
Memasukkan nilai 8 ke register AX (16-bit). Ini angka yang akan dicek apakah ganjil atau genap.

2. and ax, 1
   
Melakukan operasi AND biner antara 8 dan 1. Hasilnya:

8 = 1000
1 = 0001
----------
    0000 (hasil = 0)
Kalau hasilnya 0, berarti angka genap (bit terakhir = 0). Kalau hasilnya 1, berarti ganjil

3. jz evnn
   
Jika hasil AND tadi adalah nol (Z = zero flag), lompat ke label evnn, artinya angka genap

# Jika Ganjil

    mov eax, 4
    mov ebx, 1
    mov ecx, odd_msg
    mov edx, len2
    int 0x80
    jmp outprog

Jika angka ganjil, maka:

1. eax = 4: syscall number 4 → sys_write

2. ebx = 1: file descriptor 1 → stdout (layar)

3. ecx = odd_msg: alamat pesan "bilangan ganjil"

4. edx = len2: panjang pesan

int 0x80: memanggil interrupt Linux untuk menjalankan syscall.

jmp outprog: lompat ke bagian akhir program (supaya tidak lanjut ke evnn).

# Jika Genap

evnn:

    mov ah, 09h
    mov eax, 4
    mov ebx, 1
    mov ecx, even_msg
    mov edx, len1
    int 0x80

Ini bagian yang dijalankan kalau angka genap:

1. Mirip dengan bagian sebelumnya, hanya saja:

2. Menampilkan pesan "bilangan genap"

3. Ada instruksi mov ah, 09h yang tidak berguna dalam konteks Linux syscall (ini biasanya dipakai di DOS, jadi bisa dihapus)

4. # Keluar dari Program

 outprog:

    mov eax, 1
    int 0x80

1. eax = 1: syscall number 1 → sys_exit

2. int 0x80: eksekusi keluar dari program

   # Bagia Data

section .data
even_msg db 'bilangan genap'
len1 equ $ - even_msg

odd_msg db 'bilangan ganjil'
len2 equ $ - odd_msg


1. section .data: Tempat menyimpan data statis (konstanta string).

2. even_msg, odd_msg: Pesan yang ditampilkan.

3. len1, len2: Panjang masing-masing string, dihitung otomatis ($ adalah alamat saat ini, jadi selisihnya adalah panjang string)
 
