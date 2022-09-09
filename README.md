# CH32V307-TinyMaix

## Port TinyMaix to CH32V307

https://github.com/sipeed/TinyMaix

## Chip

|Item |Parameter|
|--   |--|
|Chip |CH32V307|
|Arch |RISCV32 IMAC|
|Freq |144M |
|Flash|480KB|
|RAM  |128KB|

## Board
CH32V307V-R0-1v0

## Development Environment
riscv-none-embed-gcc

## Step/Project
Just simple edit `tm_port.h`

## Result
|config  |mnist|cifar|mbnet96|mbnet128|Note|
|---     |---  |---  |---    |---     |---|
|O0 CPU  |1.23    |75.77  |*    | *        ||
|O1 CPU  |1.14    |63.78  |*    |*     ||

