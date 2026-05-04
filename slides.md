<!-- .slide: class="center" -->

# Switch 2. La movida

Estado del arte de la Nintendo Switch 2.

---

# Disclaimer

<!-- .slide: class="center disclaimer" -->

Esta charla únicamente tiene **fines educativos**.

El autor en ningún momento promueve o alienta a la piratería.

---

# YO

<div class="two-column">
    <p class="fragment">
        <img src="images/yo.png">
    </p>
    <p class="fragment">
        <br><br>
        Ángel Kemp
        <br><br>
        Evaluador en Jtsec
    </p>
</div>

Note:
Pero que estado del arte ni mierdas, si todo el mundo sabe que la hackearon a los dias de salir.

---

## Pero si ya se ha hackeado, pelele

<p class="fragment">
    <video controls width="100%">
      <source src="videos/retroid_userland_rop.mp4" type="video/mp4">
    </video>
</p>

---

# Motivación

¿Qué significa realmente "hackear" una consola?
<p class="fragment">Sobre todo, una consola moderna</p>

Note:
Quería entender a fondo los pasos necesarios que hacen falta para que una
consola pueda ser "hackeada", esto, es poder ejecutar cualquier tipo de código
arbitrario (sin firmar) en la consola.

--

## Definición

Poder ejecutar tu propio código usando todo el potencial de la consola que te pertenece.

Note:
te has comprado una consola y quieres desarrollar software para ella.

---

# Índice

- Entrypoints típicos
- Historia de la Switch 1
    - Primer Exploit
    - Exploit más relevante
- Actualidad
- Switch 2

---

# Entrypoints


**Chipeacion**

Note:
Chipeacion: directamente bypasseas las medidas de seguridad. Muy enfocado en fault injection.
Saltarse instrucciones rayando la placa

--

## PS2

<img src="images/ps2_modchip.jpg">

Note:
Enfocado directamente en la piratería.
Que vas a hacer un check de firma, nonono

---

# Entrypoints


Chipeacion<br>
**Archivos de guardado**

Note:
Archivos de guardado: Si un juego tiene un parser del archivo de guardado vulnerable,
se puede hacer buffer overflow y ganar ejecución de código.

--

<img src="images/twilight_princess_caratula.jpg">

--

<img src="images/epona.webp">

Note:
Poniendo el nombre in-game si que verificaba tamaño, desde el save no.
No había ASLR, como ya corría en un juego firmado, confiaba en el código, en resumen, poca mitigación moderna.

---


# Entrypoints


Chipeacion<br>
Archivos de guardado<br>
**Navegadores**

--

## Webkit

<img src="images/webkit.jpg">

Note:
Navegadores: sobre todo webkit, plagado de vulnerabilidades. Ejecutar javascript es una movida.
Motor de navegador hecho por Apple, open-source, modular, licencia permisiva.

--

## Ejemplos

<p class="fragment">Nintendo Wii</p>
<p class="fragment">Nintendo Wii <b>U</b></p>
<p class="fragment">Nintendo 3DS</p>
<p class="fragment">PS Vita</p>
<p class="fragment">PS3</p>
<p class="fragment">PS4</p>
<p class="fragment">PS5</p>
<p class="fragment">...</p>


---

# Historia de la Switch 1

Note:
Es muy importante conocer como funciona la Switch 1 y sus debilidades/fortalezas
para conocer bien como afecta eso a la Switch 2. Son primas hermanas

---

## Que hay dentro

<p class="fragment"><img src="images/tegra_x1.png"></p>

Note:
Que se os quede en la cabeza este chip, es el que usa la nintendo Switch y será importante más adelante.
Por ahora sabed que existen placas de desarrollo y documentación pública del chip, quitando aspectos de seguridad.

--

<img src="images/switch_arch.png">

---

## SO

<div class="two-column">
    <div>
        <img src="images/sw_os.jpeg">
    </div>
    <div>
        <p class="fragment">Nombre en clave "Horizon"</p>
        <p class="fragment">SO propietario</p>
        <p class="fragment">Arquitectura microkernel</p>
        <p class="fragment">Evolución de la 3DS</p>
    </div>
</div>

---

## Modelo de seguridad

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row fragment">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row fragment">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells fragment">
    <span>fs</span>
    <span>ncm</span>
    <span>sm</span>
    <span>pm</span>
    <span>ldr</span>
    <span>spl</span>
  </div>

  <div class="security-row fragment">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row fragment">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Cada fila tiene acceso a llamadas al sistema limitadas.

---

## Primer entrypoint

Teniendo en cuenta que la switch, en sus primeras versiones no tenía la aplicación de navegador, ¿cuál creeis que fue su primer entrypoint?
<p class="fragment">Efectivamente, <b>el navegador</b></p>

---

<img src="images/puyopuyo.png">

---

<img src="images/puyopuyo_browser.png">

Note:
puedes clickar en el enlace de SEGA.

--

<img src="images/sw1_proxy.jpg">

Note:
Si lo juntas con las opciones de proxy de la propia consola, ya estaría, controlas la página.

--

### CVE-2016-4657

Note:
Hay un video mu weno de live overflow que explica técnicamente la base del exploit.

---

En la versión 2.0 del firmware introdujeron soporte para los portales captivos.

Ejemplo:

<p class="fragment">Te conectas al wifi público del aeropuerto y te pide pelas</b></p>

---

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells">
    <span>fs</span>
    <span>ncm</span>
    <span>sm</span>
    <span>pm</span>
    <span>ldr</span>
    <span>spl</span>
  </div>

  <div class="security-row">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row security-row-game">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Todavía no se puede dumpear nada de los microservicios.

---

Mediante black-box testing, se pudo detectar un array out-of-bound read.
<p class="fragment">Si especificabas índices negativos, te dumpeaba el código (.text)</p>

Note:
quedaos con esta idea de dumpear código.

---

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells">
    <span>fs</span>
    <span>ncm</span>
    <span>sm</span>
    <span>pm</span>
    <span>ldr</span>
    <span>spl</span>
  </div>

  <div class="security-row security-row-microservices">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row security-row-game">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Focus ahora en SM, es el portero que dicta si te puedes comunicar o no con procesos.

---

### Protocolo IPC

Note:
Yo como proceso tengo que pedirle permiso al kernel para comunicarme con otro proceso.

--

### SM

<div class="two-column">
    <div>
        <img src="images/sm_communication.png">
    </div>
    <div>
        <p class="fragment">Proceso pide handle de sm</p>
        <p class="fragment">Inicialización (0)</p>
        <p class="fragment">Proceso pide handle del otro proceso</p>
        <p class="fragment">Comunicación con el otro proceso</p>
    </div>
</div>

--

Pero... <p class="fragment">¿y si no dices de inicializar?</p>

<p class="fragment">El campo PID de SM se queda a 0.</p>
<p class="fragment"><b>Puedes comunicarte con cualquier proceso</b></p>

---

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells security-row-services">
    <span>fs</span>
    <span>ncm</span>
    <span>sm</span>
    <span>pm</span>
    <span>ldr</span>
    <span>spl</span>
  </div>

  <div class="security-row security-row-microservices">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row security-row-game">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Puedes comunicarte con los procesos, pero "a ciegas".
Hay que encontrar una manera de dumpear código.

No me queda muy claro, pero sacaron como se lanzaba un proceso y la función que se encargaba de montar el los archivos del sistema

---



### fsp-ldr

Este servicio, presente dentro de FS, permite, entre otras cosas, dumpear todos los módulos de código.

<p class="fragment">Resulta que solo se puede tener una sesión activa con este servicio</p>
<p class="fragment">Si crasheas el servicio LDR, <b>se libera la sesión...</b></p>
<p class="fragment">Resulta que se puede crashear de muchas maneras...</p>
<p class="fragment">Ejemplo: ldr:ro cmd 0</p>
<p class="fragment"><b>Profit</b></p>

---

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row security-row-kernel">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells security-row-services">
    <span>fs</span>
    <span>ncm</span>
    <span>sm</span>
    <span>pm</span>
    <span>ldr</span>
    <span>spl</span>
  </div>

  <div class="security-row security-row-microservices">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row security-row-game">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Ojos al kernel, ¿como dumpeas su código?

---

### Kernel

<p class="fragment">Muy complicado sacar primitivas de lectura mediante black-box.</p>
<p class="fragment">Sobre todo mediante software</p>

<p class="fragment">¿Por qué no se prueba desde hardware?</p>
<p class="fragment">Voltage glitching</p>
<p class="fragment"><b>Profit</b></p>

Note:
Como dije al principio, el procesador que usa la Switch está documentado, su proceso de arranque también.
La idea es glitchear, justo después de que se ejecute el bootROM, para poder sobreescribir la clave con la que va firmada el código del bootloader

--

<img src="images/glitching_setup.png">

Note:
Recuerdo que la idea de esto es sacar el código del kernel para analizarlo y explotarlo.

--

Accidentalmente, mapearon el ejecutable del kernel en userspace lol.

Note:
Aunque no se permitía leer, al no implementar kASLR, se podían ejecutar funciones del kernel. "ASLR Bypass"

--

Existía una última protección: SMMU (IOMMU)

<p class="fragment">Era bastante bypasseable</p>

Note:
Entrada y salida mapeada a memoria, el kernel se encargaba de mantener una tabla que restringía el acceso a memoria de los dispositivos.

---

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row security-row-services">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells security-row-services">
    <span>fs</span>
    <span>ncm</span>
    <span>sm</span>
    <span>pm</span>
    <span>ldr</span>
    <span>spl</span>
  </div>

  <div class="security-row security-row-microservices">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row security-row-game">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Faltaría por explicar como se cargaron la TrustZone, pero realmente una vez tienes el kernel comprometido ya no hace falta más.
Se abusaba de la funcionalidad de "deep sleep". Parámetros críticos de crypto se guardaban en la RAM. Se lo pides al kernel y ya.

---

## Exploit más relevante

### Fusée-gelée

---

<img src="images/tegra_x1.png">

Note:
ESPERO QUE OS ACORDÉIS DEL CHIP

--

<img src="images/tegra_x1_arch.png">

--

<div class="two-column">
    <div>
        <img src="images/tegra_x1_devboard.jpg">
    </div>
    <div>
        <p class="fragment">Placa de desarrollo</p>
        <p class="fragment">Cualquiera puede comprarla</p>
        <p class="fragment">Documento técnico muy extenso, público</p>
        <p class="fragment">Casualmente no hay nada relacionado con la seguridad en esa documentación</p>
    </div>
</div>

--

<img src="images/tegra_x1_boot.png">

--

A pesar que había placas de desarrollo y mucha documentación sobre el procesador, la chicha estaba "oculta".

<p class="fragment">No se podía obtener el código de la secuencia de boot</p>
<p class="fragment">Estaba protegido contra lectura </p>
<p class="fragment">Voltage glitching</p>

Note:
Usando la documentación pública, se pudo discernir que se escribía un byte que luego hacía que se protegiese la ROM
Glitcheando la instrucción que escribía ese byte, no se protegía la ROM y se podía leer.

---

### Vale pero ¿y que?

--

<img src="images/tegra_x1_boot.png">

--

La funcionalidad de recovery permite ejecutar código <b>firmado</b>.

Note:
Útil en la fábrica o cuando se brickea una Switch.

--

Realmente habla USB.

<p class="fragment">Había un buffer overflow en como gestionaba el control de USB</p>
<p class="fragment"><b>Profit</b></p>

Note:
Por como implementaban los dispositivos USB, podías especificar código, y antes
que revisase si estaba firmado o no, hacer el overflow y que la dirección de retorno
apunte al payload.

---

El bug no se reportaría a Nintendo, sino a Nvidia directamente.
<p class="fragment"><b>Hasta que no salieron nuevas Switches con una nueva remesa de procesadores parcheados, no se solucionó</b></p>

---

### Entrando en modo recovery

Tres posibilidades:

<p class="fragment">Ejecución de código en el kernel</p>
<p class="fragment">Quitas la placa eMMC</p>
<p class="fragment">Presionas una combinación mágica mientras enciendes</p>
<p class="fragment">"Volumen Arriba" + "Home"</p>

Note:
No existe el botón HOME, existe un pin GPIO, localizado en el joycon derecho

--

<img src="images/fusee_gelee.png">

---

# Actualidad

A dia de hoy, el firmware de la Switch, a menos que sea con modchip, no se puede hacer nada.
Se parchea todo lo que va saliendo.

<div class="two-column">
    <div>
        <p class="fragment">- ASLR</p>
        <p class="fragment">- kASLR</p>
        <p class="fragment">- kASLR</p>
    </div>
    <div>
        <p class="fragment">- PASLR</p>
        <p class="fragment">- Software PAC</p>
        <p class="fragment">- RelRO</p>
    </div>
</div>

Note:
- PAC: Pointer Authentication, los punteros van firmados
- RelRO: No permite sobreescribir punteros del "got".

---

# SWITCH 2

---

<!-- .slide: class="security-slide" -->

<div class="security-model">

  <div class="security-row ">
    <span class="trustzone">TrustZone</span>
    <span class="detail">crypto</span>
  </div>


  <div class="security-row ">
    <span class="kernel">Kernel</span>
    <span class="detail">process isolation, IOMMU</span>
  </div>

  <div class="security-row security-cells ">
    <span>"Kernel"Services</span>
  </div>

  <div class="security-row ">
    <span class="microservices">Microservices</span>
    <span class="detail">least privilege</span>
  </div>

  <div class="security-row ">
    <span class="game">Game/Application</span>
    <span class="detail">untrusted</span>
  </div>

</div>

Note:
Es lo mismo que la SW1, ¿no?

---

## eXecute Only Memory (XOM) (--x)

<p class="fragment">Las secciones .text no se pueden leer, solo ejecutar x.x</p>
<p class="fragment">Funciona a nivel de procesador</p>

Note:
Como proceso, no necesitas leer el código, solo que puedas saltar a él para que el procesador se encargue de ejecutarlo.
En la SW1 se soporta a partir de cierta versión del firmware, pero no se usa.

--

Los últimos cartuchos que están saliendo implementan esto.
<p class="fragment">En el firmware que traía la SW2 de salida, los sysmodules también</p>

---

## PAC a nivel de hardware

Las direcciones de retorno del stack están firmadas.
<p class="fragment">Funciona a nivel de procesador</p>

Note:
En la SW1, funcionaba a nivel de software

---

## Procesador

Se sabe cual es el modelo (Tegra T239).

<p class="fragment">Única y exclusivamente lo usa Nintendo para la Switch 2</p>
<p class="fragment">Ni documentación ni devkit ni pollas</p>
<p class="fragment">Soporta encriptación de memoria</p>

Note:
Aunque se sniffe el bus de la memoria, no se vería nada.

---

## ¿Qué ha pasado de momento?

<p class="fragment">El navegador...</p>
<p class="fragment">Juegos de switch 1 con saves "maliciosos"</p>
<p class="fragment">Bastante "sandboxeados", con pocos privilegios interesantes</p>
<p class="fragment">El "sysmodule" "ldn" ha sido "comprometido"</p>
<p class="fragment"><b>Intentos de glitch electromagnéticos en el bus de la DRAM</b></p>

Note:
De lo del sysmodule se sacó que tienen XOM
De los intentos de glitch se sacó que la memoria estaba encriptada, los esfuerzos
de investigación se están centrando aquí.

---

<img src="images/switch2hacks-shitpost.png">

---


<!-- .slide: class="center" -->

# Gracias

¿Preguntas?

<span class="small muted">yo@akemp.es</span>
