# Modos de Red en VirtualBox

| Modo             | Vm -> Host         | Vm <- Host         | Vm1 <-> Vm2        | Vm -> Intenet      | VM <- Internet      |
| :--------------- | :----------------: | :----------------: | :----------------: | :----------------: | ------------------: |
| Host-only        | :white_check_mark: | :white_check_mark: | :white_check_mark: | :x:                | :x:                 |
| Internal         | :x:                | :x:                | :white_check_mark: | :x:                | :x:                 |
| Adaptador puente | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark:  |
| NAT              | :white_check_mark: | Reenvío de puertos | :x:                | :white_check_mark: | Reenvío de puertos  |
| Red Nat          | :white_check_mark: | Reenvío de puertos | :white_check_mark: | :white_check_mark: | Reenvío de puertos  |

## Nat


## Red Nat (Nat Network)



## Adaptador Puente (Bridge adapter)