SMART calefactor
Projecte per la modificació d'un calefactor economic en smart

Amb un esp32 es converteix un calefactor de paret en un calefactor inteligent per aprofitar energia solar.

He adquirit a lidl uns calefectors de 600W de paret (endollables directament) per a un projecte de calefacció aprofitant excedents solars.
L'equip que ha costat menys de 10€/u disposa de un interrupor ON/OFF i quatre botons de funcionament. 
Amb els botons podem engegar o parar manualment l'equip, seleccionar la temperatura a la que ha d'arribar i programar el temps d'encesa.
l'equip disposa d'un sensor de temperatura ambiental amb un sensor NTC, una protecció que el para si detecta que no està en posició vertical, i un sistema de refrigeració al apagar que fa funcionar el ventilador 30segons més que les la resistencia termica.
D'aquesta manera evita un sobreescalment del equip en una aturada programada.

La intenció inicial era posar un endoll wifi o zigbee per encendre remotament l'equip però aquesta idea inicial ha hagut de ser modificada:
Al treure o tornar el fluid electric, el calefactor es situa automaticament en una temperatura de consigna de 25ºC. El fet de parar abruptament el seu funcionament pot reescalfar l'equip de plàstic. Per aquest motiu disposa d'un ventilador que funciona 30s més que la resistencia tèrmica.

Per domotitzar-lo vaig adquirir un ESP32 C3 mini i el vaig conectar a la font de 5V del propi equip i un gpio al polsador d'ON/OFF.
El problema és la font de 5V és molt justa i no aguanta l'encesa del Esp32 fallant ed forma constant.
Per solucionar-ho he comprat una minifont de 220AC a 5VDC que hi cap a l'interior del equip (amb la placa ESP32 també) conectat a la sortida del interruptor principal.

El problema és que el polsador ON/OFF requereix un pols a terra per activar/parar l'equip. Com que el ESP32 utilitza 3V en els GPIO he hagut de instal.lar un level shifter (MOS-Fet) per adaptar les tensions de treball.
Un altre problema és que malgrat aconseguiem obrir i tancar l'equip, desconeixiem si havia quedat obert o tancat el calefactor. 

La solució ha estat conectar un altre GPIO (en aquest cas entrada) al bit que obre i tanca el heater (resistencia). D'aquesta manera podem conneixer si està obert o tancat.

Les soldadures no son complicades però cal aillar bé amb termo-retractil la font de 5V, la placa ESP32 i el levelshifter per evitar cap cortcircuit.

Un cop fetes aquestes modificacions en diversos equips podem implemntar una solució per aprofitar els excedents solars per escalfar la casa.

La idea és aprofitar els excedents electrics solars per anar calefactant les habitacions que desitgem. Com que la potencia son 600W per cada aparell, podrem anar engegant/apagant aparells en funció de la quantitat d'excedents en salts de 0,6KWh.
A cada habitació hi haurà un sensor de temperatura que reportarà a Home Assistant la temperatura de cada habitació.
Home Assistant, en funció del excedent disponible s'encarregarà d'anar obrint/tancant els calefactors per consumir l'exedent sense generar consum.
Es poden escriure moltes automatitzacions:
  la més senzilla és que obri per ordre de més freda a més calenta els calefactors. Com que tots tenen una limitació de 25ªC al engegar no es sobrepassarà aquesta temperatura n cap moment.

També es podrien posar unes prioritats en funció de la sala on estigui cada calefactor (dormitori o bugaderia). O prioritats en funció del moment del dia: matí (sala estar i menjador) - tarda - vespre (dormitoris).

En el cas que m'ocupa, he instal.lat 10 plaques de 580Wp podent tenir un màxim de producció/excedent de 5,8KWh. Dividit entre 0,6KWh podria obrir fins a 9 calefactors simultaniament...

IDEES de millora:
Com que el ESP32 va sobrat per la feina que realitza, es podria implementar el projecte Bluetooth Proxy per ajudar a repetir la senyal Bluetooth per tota la casa i fer localització de dispositius (mobils, rellotges, beacons..) per activar rutines com encendre o apagar llums/calefacció..


