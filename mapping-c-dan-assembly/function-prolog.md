# Function Proglog

Dibagian sebelumnya kita telah melihat seperti script sederhana bahasa C yang hanya berisi `return` dan program hello world dalam assembly. Dibagian sebelumnya juga banyak kita temukan instruksi berupa `push rbp`, `mov rbp, rsp`. Seperti di kode assembly hello world sebelumnya
```
$ gdb -batch -ex 'file hello' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     lea    rdi,[rip+0x92]        # 0x400584
   0x00000000004004f2 <+11>:    call   0x4003f0 <puts@plt>
   0x00000000004004f7 <+16>:    mov    eax,0x0
   0x00000000004004fc <+21>:    pop    rbp
   0x00000000004004fd <+22>:    ret    
End of assembler dump.
```

Sebenarnya untuk apa fungsi dari instruksi itu ?