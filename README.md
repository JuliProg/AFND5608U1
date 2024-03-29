![Create new chip](https://github.com/JuliProg/AFND5608U1/workflows/Create%20new%20chip/badge.svg?event=repository_dispatch)
![ChipUpdate](https://github.com/JuliProg/AFND5608U1/workflows/ChipUpdate/badge.svg)
# Join the development of the project ([list of tasks](https://github.com/users/JuliProg/projects/1))


# AFND5608U1
Implementation of the AFND5608U1 chip for the JuliProg programmer

Dependency injection, DI based on MEF framework is used to connect the chip to the programmer.

<section class = "listing">

# Chip parameters
```c#


        //--------------------Vendor Specific Pin configuration---------------------------

        //  VSP1(38pin) - NC    
        //  VSP2(35pin) - NC
        //  VSP3(20pin) - NC

        ChipAssembly()
        {
            myChip.devManuf = "ATOSolution";
            myChip.name = "AFND5608U1";
            myChip.chipID = "ffffffff";      // device ID - not found

            myChip.width = Organization.x8;    // chip width - 8 bit
            myChip.bytesPP = 512;             // page size - 512 byte 
            myChip.spareBytesPP = 16;          // size Spare Area - 16 byte
            myChip.pagesPB = 32;               // the number of pages per block - 32 
            myChip.bloksPLUN = 2048;           // number of blocks in CE - 2048
            myChip.LUNs = 1;                   // the amount of CE in the chip
            myChip.colAdrCycles = 1;           // cycles for column addressing
            myChip.rowAdrCycles = 2;           // cycles for row addressing 
            myChip.vcc = Vcc.v3_3;             // supply voltage
            (myChip as ChipPrototype_v1).EccBits = 1;                // required Ecc bits for each 512 bytes

```
# Chip operations
```c#


            //------- Add chip operations    https://github.com/JuliProg/Wiki#command-set----------------------------------------------------

            myChip.Operations("Reset_FFh").
                   Operations("Erase_60h_D0h").
                   Operations("Read_00h_30h").
                   Operations("PageProgram_80h_10h");

```
# Initial Invalid Block (s)
```c#

            
            //------- Select the Initial Invalid Block (s) algorithm    https://github.com/JuliProg/Wiki/wiki/Initiate-Invalid-Block-----------
                
            myChip.InitialInvalidBlock = "InitInvalidBlock_v1";
                
```
# Chip registers (optional)
```c#


            //------- Add chip registers (optional)----------------------------------------------------

            myChip.registers.Add(                   // https://github.com/JuliProg/Wiki/wiki/StatusRegister
                "Status Register").
                Size(1).
                Operations("ReadStatus_70h").
                Interpretation("SR_Interpreted").
                UseAsStatusRegister();



            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/ID-Register
                "Id Register").
                Size(4).
                Operations("ReadId_90h").               
                Interpretation(ID_interpreting);

```
# Interpretation of ID-register values ​​(optional)
```c#


        public string ID_interpreting(Register register)   
        
```
</section>
















