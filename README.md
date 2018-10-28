### Cambia a la interfaz con internet desde RouterOS
##### Solo tienes que especificar tu IP a la variable local.


```
:global a {""}
:for i from=0 to=([/interface wireless print count-only]-2) do={
set ($a->$i) [/interface wireless get value-name=name number=$i]
}
:local IP "172.16.20.248";
:local PA ([/ip firewall address-list get value-name=list [find address=$IP]]);
:local continue true;
:if ( [/ping 8.8.8.8 routing-table="$PA" count=2 ] >0 ) do={
} else={
:while ($continue) do={
:foreach w in=$a do={
:if ( [/ping 8.8.8.8 routing-table=$w count=2 ] >0 ) do={
ip firewall address-list set [find where address=$IP] list=$w
:log info message="0==||====> INTERNET encontrado en $w";
:set continue false;
}
}
}
```
