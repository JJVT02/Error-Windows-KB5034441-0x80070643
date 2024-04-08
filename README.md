# Error-Windows-KB5034441-0x80070643
- Abre una ventana del símbolo del sistema (CMD) como administrador.


- Para comprobar el estado de WinRE, 
Ejecuta:
```
reagentc /info
```
- Si WinRE está instalado, debería haber una "ubicación de Windows RE" con una ruta de acceso al directorio de WinRE. Un ejemplo es "Windows RE ubicación:

  **[file://%3f/GLOBALROOT/device/harddisk0/partition4/Recovery/WindowsRE]\\?\GLOBALROOT\device\harddisk0\partition4\Recovery\WindowsRE"**
 
 Aquí, el número después de "harddisk" (disco duro) y "partition" (partición) es el índice del disco y la partición en la que está activado WinRE.


## Para deshabilitar WinRE, ejecuta:
```
reagentc /disable
```
## Reducir particion
Reduzca la partición del sistema operativo y prepare el disco para una nueva partición de recuperación.


Para reducirlo, ejecuta:

```
diskpart
```

Luego haz este comando:
```bash
list disk
```

Para seleccionar el disco del SO, Ejecuta:
```
 sel disk<OS disk index>
```

 Este debe ser el mismo índice de disco que WinRE.


Para comprobar la partición en el disco del so y encontrar la partición del so, ejecuta list part


Para seleccionar la partición del sistema operativo, ejecuta:

```
sel part<OS partition index>
```

Ejecutar 

```
shrink desired=512 minimum=512
```



(espacio en MB que ocupará WinRE)


Para seleccionar la partición de WinRE, ejecuta 

```
sel part<WinRE partition index>
```

Para eliminar la partición de WinRE, ejecuta: 

```
delete partition override
```

Cree una nueva partición de recuperación.

## IMPORTANTE
En primer lugar, compruebe si el estilo de partición de disco es **GPT** o **MBR.**  Para ello, ejecute list disk y comprueba si hay un carácter de asterisco (*) en la columna "Gpt".  Si hay un carácter de asterisco (*), la unidad es GPT. De lo contrario, la unidad es MBR.


Si el disco es **GPT**, ejecuta 
```
create partition primary id=de94bba4-06d1-4d40-a16a-bfd50179d6ac
```
seguido del comando 
```
gpt attributes =0x8000000000000001
```

Si el disco es **MBR**, ejecuta 

```
create partition primary id=27
```

Para formatear la partición, ejecuta:

```
format quick fs=ntfs label=”Windows RE tools”
```

Para confirmar que se ha creado la partición de WinRE, ejecuta:

```
list vol
```

Para salir de diskpart, ejecuta:

```
exit
```

Para volver a habilitar WinRE, ejecuta:
```
 reagentc /enable
```

Para confirmar dónde está instalado WinRE, ejecuta:
```
 reagentc /info
```
