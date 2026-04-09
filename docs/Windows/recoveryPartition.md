# Создание recovery partition в начале диска
```
select disk 0
clean
convert gpt

create partition efi size=200

format quick fs=fat32 label="System"
assign letter="S"

create partition msr size=16

create partition primary size=2000
format quick fs=ntfs label="Recovery"
assign letter="R"
set id="de94bba4-06d1-4d40-a16a-bfd50179d6ac"
gpt attributes=0x8000000000000001


create partition primary

format quick fs=ntfs label="OS"
assign letter="W"
list volume
exit

```
https://thedxt.ca/2023/06/moving-windows-recovery-partition-correctly/

https://www.precedence.co.uk/wiki/Support-KB-Windows/RecoveryPartition

https://blog.loftinnc.com/manually-create-windows-recovery-partition-before-os-partition
