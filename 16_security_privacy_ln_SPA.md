[[seguridad_y_privacidad]]
== Seguridad y Privacidad de pass:[<span class="keep-together">Lightning Network</span>]

((("seguridad y privacidad", id="ix_16_security_privacy_ln-asciidoc0", range="startofrange")))En este capítulo, analizamos algunos de los problemas más importantes relacionados con la seguridad y la privacidad de Lightning Network. Primero, consideraremos la privacidad, lo que significa, cómo evaluarla y algunas cosas que puede hacer para proteger su propia privacidad mientras usa Lightning Network. Luego exploraremos algunos ataques comunes y técnicas de mitigación.

=== ¿Por qué la Privacidad es importante?

((("seguridad y privacidad","importancia de la privacidad"))) La propuesta de valor clave de la criptomoneda es que estamos ante dinero resistente a la censura. Bitcoin ofrece a los participantes la posibilidad de almacenar y transferir su riqueza sin interferencias de los gobiernos, bancos o corporaciones. Lightning Network continúa con esta misión.
//The key value proposition of cryptocurrency is censorship resistant money. Bitcoin offers participants the possibility of storing and transferring their wealth without interference by governments, banks, or corporations. The Lightning Network continues this mission.

//Unlike trivial scaling solutions like custodial Bitcoin banks, the Lightning Network aims to scale Bitcoin without compromising on self custody, which should lead to greater censorship resistance in the Bitcoin ecosystem. However, the Lightning Network operates under a different security model, which introduces novel security and privacy challenges.
A diferencia de las soluciones de escalado triviales como los bancos de custodia de Bitcoin, Lightning Network tiene como objetivo escalar Bitcoin sin comprometer la autocustodia, lo que debería conducir a una mayor resistencia a la censura en el ecosistema de Bitcoin. Sin embargo, Lightning Network opera bajo un modelo de seguridad diferente, que presenta nuevos desafíos de seguridad y privacidad.

=== Definiciones de privacidad

((("seguridad y privacidad","definiciones de privacidad", id="ix_16_security_privacy_ln-asciidoc1", range="startofrange"))) A la pregunta, "¿Es Lightning privado?", no hay una respuesta directa. La privacidad es un tema complejo; a menudo es difícil definir con precisión lo que entendemos por privacidad, especialmente si no eres un investigador de privacidad. Afortunadamente, los investigadores de la privacidad utilizan proceesos para analizar y evaluar las características de un sistema en cuanto a privacidad, ¡y nosotros podemos utilizarlas también! Veamos cómo un investigador de seguridad podría tratar de responder a la pregunta "¿Es Lightning privado?" en dos pasos generales.
//The question, "Is Lightning private?" has no direct answer. Privacy is a complex topic; it is often difficult to precisely define what we mean by privacy, particularly if you are not a privacy researcher. Fortunately, privacy researchers use processes to analyze and evaluate the privacy characteristics of systems, and we can use them too! Let's look at how a security researcher might seek to answer the question, "Is Lightning private?" in two general steps.

Primero, un investigador de privacidad definiría un _modelo de seguridad_ que especifica lo que un adversario es capaz de hacer y pretende lograr.
Luego, describirían las propiedades relevantes del sistema y verificarían si cumple con los requisitos.

=== Proceso para evaluar la privacidad

((("seguridad y privacidad","proceso para evaluar la privacidad")))((("security assumptions")))
Un modelo de seguridad se basa en un conjunto de _supuestos de seguridad_ subyacentes.
En los sistemas criptográficos, estas suposiciones a menudo se centran en las propiedades matemáticas de las primitivas criptográficas, como cifrados, firmas y funciones hash.
Las suposiciones de seguridad de Lightning Network son que las firmas ECDSA, la función hash SHA-256 y otras funciones criptográficas utilizadas en el protocolo se comportan dentro de sus definiciones de seguridad.
Por ejemplo, asumimos que es prácticamente imposible encontrar una preimagen (y una segunda preimagen) de una función hash.
Esto permite que Lightning Network confíe en el mecanismo HTLC (que usa la preimagen de una función hash) para la atomicidad de los pagos multisalto: nadie excepto el destinatario final puede revelar el secreto del pago y resolver el HTLC.
También asumimos un grado de conectividad en la red, es decir, que los canales Lightning forman un gráfico conectado. Por lo tanto, es posible encontrar un camino desde cualquier emisor a cualquier receptor. Finalmente, asumimos que los mensajes de red se propagan dentro de ciertos tiempos de espera.

Ahora que hemos identificado algunas de nuestras suposiciones subyacentes, consideremos algunos posibles adversarios.

Estos son algunos modelos posibles de adversarios en Lightning Network.
Un nodo de reenvío "honesto pero curioso" puede observar los montos de los pagos, los nodos inmediatamente anteriores y posteriores, y el gráfico de los canales anunciados con sus capacidades.
Un nodo muy bien conectado puede hacer lo mismo pero en mayor medida.
Por ejemplo, considere a los desarrolladores de una billetera popular que mantienen un nodo al que sus usuarios se conectan de forma predeterminada.
Este nodo sería responsable de enrutar una gran parte de los pagos hacia y desde los usuarios de esa billetera.
¿Qué pasa si varios nodos están bajo control adversario?
Si dos nodos en connivencia se encuentran en la misma ruta de pago, entenderán que están reenviando HTLC que pertenecen al mismo pago porque los HTLC tienen el mismo hash de pago.

[NOTA]
====
Los Pagos Multiparte (ver <<mpp>>) permiten a los usuarios ofuscar sus montos de pago debido a sus tamaños de división no uniformes.
====

¿Cuáles pueden ser los objetivos de un atacante Lightning?
La seguridad de la información a menudo se describe en términos de tres propiedades principales: confidencialidad, integridad y disponibilidad.

Confidencialidad:: La información solo llega a los destinatarios previstos.
Integridad:: La información no se altera en tránsito.
Disponibilidad:: El sistema funciona la mayor parte del tiempo.

Las propiedades importantes de Lightning Network se centran principalmente en la confidencialidad y la disponibilidad. Algunas de las propiedades más importantes para la protección incluyen:

* Solo el remitente y el destinatario conocen el monto del pago.
* Nadie puede vincular emisores y receptores.
* Un usuario honesto no puede ser bloqueado para enviar y recibir pagos.

Para cada objetivo de privacidad y modelo de seguridad, existe una cierta probabilidad de que un atacante tenga éxito.
Esta probabilidad depende de varios factores, como el tamaño y la estructura de la red.
En igualdad de condiciones, generalmente es más fácil atacar con éxito una red pequeña que una grande.
Del mismo modo, cuanto más centralizada es la red, más capaz puede ser un atacante si los nodos "centrales" están bajo su control.
Por supuesto, el término centralización debe definirse con precisión para construir modelos de seguridad a su alrededor, y hay muchas definiciones posibles de qué tan centralizada es una red.
Finalmente, como red de pago, Lightning Network depende de estímulos económicos.
El tamaño y la estructura de las tarifas afectan el algoritmo de enrutamiento y, por lo tanto, pueden ayudar al atacante reenviando la mayoría de los pagos a través de sus nodos o evitar que esto suceda.(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc1")))


=== Conjunto de anonimato 
//Anonymity Set 

((("conjunto_de_anonimato")))((("desanonimizar")))((("seguridad y privacidad","conjunto de anonimato")))
¿Qué significa desanonimizar a alguien?
En términos simples, la desanonimización implica vincular alguna acción con la identidad del mundo real de una persona, como su nombre o dirección física.

En la investigación de la privacidad, la noción de desanonimización tiene más matices.
Primero, no estamos necesariamente hablando de nombres y direcciones.
Descubrir la dirección IP o el número de teléfono de alguien también puede considerarse anonimización.
Una pieza de información que permite vincular la acción de un usuario con sus acciones anteriores se denomina _identidad_.

En segundo lugar, la desanonimización no es binaria; un usuario no es completamente anónimo ni completamente desanonimizado.
En cambio, la investigación de privacidad analiza el anonimato en comparación con el conjunto de anonimato.

El _conjunto de anonimato_ es una noción central en la investigación de la privacidad.
Se refiere al conjunto de identidades tales que, desde el punto de vista de un atacante, una acción dada podría corresponder a cualquiera en el conjunto.
Considere un ejemplo de la vida real.
Imagina que conoces a una persona en una calle de la ciudad.
¿Cuál es su anonimato establecido desde tú punto de vista?
Si no lo conoces personalmente y sin ninguna información adicional, su conjunto de anonimato equivale aproximadamente a la población de la ciudad, incluidos los viajeros.
Si además consideras su apariencia, es posible que pueda estimar aproximadamente su edad y excluir a los residentes de la ciudad que obviamente son mayores o menores que la persona en cuestión del conjunto de anonimato.
Además, si observas que la persona ingresa a la oficina de la empresa X con una credencial electrónica,
//the anonymity set shrinks to the number pass:[<span class="keep-together">of Company</span>] X's employees and visitors.
el conjunto de anonimato se reduce al pase de número: de empleados y visitantes [<span class="keep-together">de la empresa</span>] X.
Finalmente, puedes ver el número de matrícula del coche que utilizó para llegar al lugar.
Si eres un observador casual, esto no te da mucho.
Sin embargo, si tú eres un funcionario de la ciudad y tienes acceso a la base de datos que relaciona los números de matrícula con los nombres, puede reducir el anonimato establecido a solo unas pocas personas: el propietario del coche y cualquier amigo cercano y pariente que pueda haber tomado prestado dicho coche.

Este ejemplo ilustra algunos puntos importantes.
Primero, cada bit de información puede acercar al adversario a su objetivo.
Puede que no sea necesario reducir el conjunto de anonimato al tamaño de uno.
Por ejemplo, si el adversario planea un ataque de denegación de servicio (DoS) dirigido y puede derribar 100 servidores, el conjunto de anonimato de 100 es suficiente.
En segundo lugar, el adversario puede correlacionar información de diferentes fuentes.
Incluso si una fuga de privacidad parece relativamente benigna, nunca sabemos lo que puede lograr en combinación con otras fuentes de datos.
Finalmente, especialmente en configuraciones criptográficas, el atacante siempre tiene el "último recurso" de una búsqueda de fuerza bruta.
Las primitivas criptográficas están diseñadas para que sea prácticamente imposible adivinar un secreto como una clave privada.
Sin embargo, cada bit de información acerca al adversario a este objetivo y, en algún momento, se vuelve alcanzable.

En términos de Lightning, eliminar el anonimato generalmente significa derivar una correspondencia entre pagos y usuarios identificados por ID de nodo.
A cada pago se le puede asignar un conjunto de anonimato de remitente y un conjunto de anonimato de receptor.
Idealmente, el conjunto de anonimato consiste en todos los usuarios de la red.
Esto asegura que el atacante no tiene información alguna.
Sin embargo, la red real filtra información que permite a un atacante restringir la búsqueda.
Cuanto más pequeño sea el conjunto de anonimato, mayor será la posibilidad de una desanonimización exitosa.

[role="pagebreak-before less_space"]
=== Diferencias entre Lightning Network y Bitcoin en términos de privacidad

((("seguridad y privacidad","diferencias entre Lightning Network y Bitcoin en términos de privacidad", id="ix_16_security_privacy_ln-asciidoc2", range="startofrange")))Si bien es cierto que las transacciones en la red de Bitcoin no asocian identidades del mundo real con direcciones de Bitcoin, todas las transacciones se transmiten en texto no cifrado y se pueden analizar.
Se han creado varias empresas que buscan la forma de eliminar el anonimato de los usuarios de Bitcoin y otras criptomonedas.

A primera vista, Lightning brinda una mejor privacidad que Bitcoin porque los pagos de Lightning no se transmiten a toda la red.
Si bien esto mejora la línea base de privacidad, otras propiedades del protocolo Lightning pueden hacer que los pagos anónimos sean más desafiantes.
Por ejemplo, los pagos más grandes pueden tener menos opciones de enrutamiento.
Esto puede permitir que un adversario que controle nodos bien capitalizados enrute la mayoría de los pagos grandes y, que descubra los cantidades y probablemente otros detalles. Con el tiempo, a medida que crece Lightning Network, esto puede convertirse en un problema menor.

Otra diferencia relevante entre Lightning y Bitcoin es que los nodos Lightning mantienen una identidad permanente, mientras que los nodos Bitcoin no.
Un usuario sofisticado de Bitcoin puede cambiar fácilmente los nodos utilizados para recibir datos de la blockchain y transmitir transacciones.
Un usuario Lightning, por el contrario, envía y recibe pagos a través de los nodos que ha utilizado para abrir sus canales de pago.
Además, el protocolo Lightning asume que los nodos de enrutamiento anuncian su dirección IP además de su ID de nodo.
Esto crea un vínculo permanente entre los ID de nodo y las direcciones IP, lo que puede ser peligroso si se tiene en cuenta que una dirección IP suele ser un paso intermedio en los ataques de anonimato vinculados a la ubicación física del usuario y, en la mayoría de los casos, a la identidad del mundo real.
Es posible usar Lightning sobre Tor, pero muchos nodos no usan esta funcionalidad, como se puede ver en https://1ml.com/statistics[estadísticas recopiladas de los nodos anunciados].

Un usuario Lightning, al enviar un pago, tiene a sus vecinos en su conjunto de anonimato.
Específicamente, un nodo de enrutamiento solo conoce los nodos inmediatamente anteriores y posteriores.
El nodo de enrutamiento no sabe si sus vecinos inmediatos en la ruta de pago son el remitente o el receptor final.
Por lo tanto, el conjunto de anonimato de un nodo en Lightning es aproximadamente igual al de sus vecinos (ver <<conjunto_de_anonimato>>).

[[conjunto_de_anonimato]]
.El conjunto de anonimato de Alice y Bob constituye sus vecinos
image::images/mtln_1601.png["El conjunto de anonimato de Alice y Bob constituye sus vecinos"]

Se aplica una lógica similar a los receptores de pago. Muchos usuarios abren solo un puñado de canales de pago, lo que limita sus conjuntos de anonimato. Además, en Lightning, el conjunto de anonimato es estático o al menos cambia lentamente.

Por el contrario, uno puede lograr conjuntos de anonimato significativamente más grandes en transacciones CoinJoin en cadena. Las transacciones CoinJoin con conjuntos de anonimato mayores de 50 son bastante frecuentes.
Por lo general, los conjuntos de anonimato en una transacción CoinJoin corresponden a un conjunto de usuarios que cambia dinámicamente. Finalmente, a los usuarios de Lightning también se les puede negar el servicio, y un atacante puede bloquear o agotar sus canales.

El reenvío de pagos requiere que el capital (¡un recurso escaso!) se bloquee temporalmente en los HTLC a lo largo de la ruta. Un atacante puede enviar muchos pagos pero no finalizarlos, ocupando el capital de los usuarios honestos durante largos períodos.

Este vector de ataque no está presente (o al menos no es tan obvio) en Bitcoin. En resumen, aunque algunos aspectos de la arquitectura de Lightning Network sugieren que es un paso adelante en términos de privacidad en comparación con Bitcoin, otras propiedades del protocolo pueden facilitar los ataques a la privacidad. Se necesita una investigación exhaustiva para evaluar qué garantías de privacidad proporciona Lightning Network y mejorar la situación.

Los temas discutidos en esta parte del capítulo resumen la investigación disponible a mediados de 2021. Sin embargo, esta área de investigación y desarrollo está creciendo rápidamente. Nos complace informar que los autores conocen varios equipos de investigación que trabajan actualmente en la privacidad de Lightning. Ahora revisemos algunos de los ataques a la privacidad de LN que se han descrito en la literatura académica. (((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc2")))


=== Ataques en Lightning

((("seguridad y privacidad","ataques en Lightning", seealso="violación de la privacidad", id="ix_16_security_privacy_ln-asciidoc3", range="startofrange")))Investigaciones recientes describen varias formas en las que la seguridad y la privacidad de Lightning Network pueden verse comprometidas.

==== Observando los montos de pago

((("violación de la privacidad","Observando los montos de pago")))Uno de los objetivos de un sistema de pago que preserva la privacidad es ocultar el monto del pago a las partes no involucradas.
Lightning Network es una mejora sobre la Capa 1 en este sentido.
Si bien las transacciones de Bitcoin se transmiten en texto sin cifrar y cualquier persona puede observarlas, los pagos Lightning solo viajan a través de unos pocos nodos a lo largo de la ruta de pago.
Sin embargo, los nodos intermediarios ven el monto del pago, aunque este monto del pago puede no corresponder al monto del pago total real (ver <<mpp>>).
Esto es necesario para crear un nuevo HTLC en cada salto.
La disponibilidad de montos de pago para los nodos intermediarios no presenta una amenaza inmediata.
Sin embargo, un nodo intermediario _honesto pero curioso_ puede usarlo como parte de un ataque mayor.


==== Vinculando remitentes y receptores

((("violación de la privacidad","vinculando remitentes y receptores", id="ix_16_security_privacy_ln-asciidoc4", range="startofrange")))Un atacante podría estar interesado en conocer el remitente y/o el receptor de un pago para revelar ciertas relaciones económicas.
Esta violación de la privacidad podría dañar la resistencia a la censura, ya que un nodo intermediario podría censurar los pagos hacia o desde ciertos destinatarios o remitentes.
Idealmente, la vinculación de remitentes con receptores no debería ser posible para nadie más que el remitente y el receptor.

En las siguientes secciones, consideraremos dos tipos de adversarios: el adversario fuera del camino y el adversario en el camino.
Un adversario fuera de la ruta intenta evaluar al remitente y al receptor de un pago sin participar en el proceso de enrutamiento del pago.
Un adversario en camino puede aprovechar cualquier información que pueda obtener enrutando el pago de intereses.

((("adversario fuera de la ruta")))Primero, considere al _adversario fuera de la ruta. En el primer paso de este escenario de ataque, un potente adversario fuera de ruta deduce los saldos individuales en cada canal de pago a través de un sondeo (descrito en una sección posterior) y forma una instantánea de la red en el momento __t~1~__. Para simplificar, hagamos que __t~1~__ sea igual a 12:05. Luego sondea la red nuevamente en algún momento posterior en el tiempo __t~2~__, que haremos 12:10. Luego, el atacante compararía las instantáneas a las 12:10 y las 12:05 y usaría las diferencias entre las dos instantáneas para inferir información sobre los pagos que se realizaron al observar las rutas que han cambiado. En el caso más simple, si solo se produjera un pago entre las 12:10 y las 12:05, el adversario observaría un único camino donde los saldos han cambiado en las mismas cantidades. Así, el adversario aprende casi todo sobre este pago: el remitente, el destinatario y el monto. Si varias rutas de pago se superponen, el adversario debe aplicar heurísticas para identificar dicha superposición y separar los pagos. 

((("adversario en la ruta"))) Ahora, dirigimos nuestra atención a un _adversario en la ruta.
Tal adversario puede parecer complicado.
Sin embargo, en junio de 2020, los investigadores notaron que el único nodo más central https://arxiv.org/pdf/2006.12143.pdf[observó cerca del 50% de todos los pagos de LN], mientras que los cuatro nodos más centrales. https://arxiv.org/pdf/1909.06890.pdf[observó un promedio de 72% de pagos].

Estos hallazgos enfatizan la relevancia del modelo de atacante en ruta.
Aunque los intermediarios en una ruta de pago solo conocen a su sucesor y predecesor, existen varias filtraciones que un intermediario malicioso u honesto pero curioso, podría usar para inferir quién es el remitente y el destinatario.

El adversario en ruta puede observar el monto de cualquier pago enrutado, así como los deltas de bloqueo de tiempo (consulte <<onion_routing>>).
Por lo tanto, el adversario puede excluir cualquier nodo del conjunto de anonimato del remitente o del receptor con capacidades inferiores a la cantidad enrutada.
Por lo tanto, observamos una compensación entre privacidad y montos de pago.
Por lo general, cuanto mayor es el monto del pago, más pequeños son los conjuntos de anonimato.
Observamos que esta fuga podría minimizarse con pagos multiparte o con canales de pago de gran capacidad.
De manera similar, los canales de pago con pequeños deltas de bloqueo de tiempo podrían excluirse de una ruta de pago.
Más precisamente, un canal de pago no puede pertenecer a un pago si el tiempo restante durante el cual el pago podría estar bloqueado es mayor que el que el nodo de reenvío estaría dispuesto a aceptar.
Esta fuga podría ser desalojada adhiriéndose a las llamadas rutas sombra.

Una de las filtraciones más sutiles y poderosas que un adversario en ruta puede fomentar es el análisis de tiempo.
Un adversario en ruta puede mantener un registro de cada pago enrutado, junto con la cantidad de tiempo que tarda un nodo en responder a una solicitud HTLC.
Antes de comenzar el ataque, el atacante aprende las características de latencia de cada nodo en Lightning Network enviándoles solicitudes.
Naturalmente, esto puede ayudar a establecer la posición precisa del adversario en la ruta de pago.
Más aún, como se demostró recientemente, un atacante puede determinar con éxito el remitente y el destinatario de un pago a partir de un conjunto de posibles remitentes y destinatarios utilizando estimadores basados ​​en el tiempo.

Finally, it's important to recognize that unknown or unstudied leakages probably exist that could aid de-anonymizing attempts. For instance, because different Lightning wallets apply different routing algorithms, even knowing the applied routing algorithm could help exclude certain nodes from being a sender and/or receiver of a payment.(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc4")))

Finalmente, es importante reconocer que probablemente existan filtraciones desconocidas o no estudiadas que podrían ayudar a los intentos de anonimización. Por ejemplo, debido a que diferentes carteras Lightning aplican diferentes algoritmos de enrutamiento, incluso sabiendo que el algoritmo de enrutamiento aplicado podría ayudar a excluir ciertos nodos de ser un remitente y/o receptor de un pago.(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc4 ")))

==== Revelación de saldos de canales (Sondeo o "Probing")
//TO DO Esto hay que revisarlo
((("violación de la privacidad","revelación de saldos de canales", id="ix_16_security_privacy_ln-asciidoc5", range="startofrange")))((("channel balances, revealing", id="ix_16_security_privacy_ln-asciidoc6", range="startofrange")))((("channel probing", id="ix_16_security_privacy_ln-asciidoc7", range="startofrange")))((("probing attack", id="ix_16_security_privacy_ln-asciidoc8", range="startofrange")))Se supone que los saldos de los canales Lightning están ocultos por razones de privacidad y eficiencia.
Un nodo Lightning solo conoce los saldos de sus canales adyacentes.
El protocolo no proporciona una forma estándar de consultar el saldo de un canal remoto.

Sin embargo, un atacante puede revelar el saldo de un canal remoto en un _ataque de sondeo o "probing attack"_.
En seguridad de la información, el sondeo se refiere a la técnica de enviar solicitudes a un sistema objetivo y sacar conclusiones sobre su estado privado en función de las respuestas recibidas.

Los canales de rayos son propensos a sondear. 
Recuerde que un pago Lightning estándar comienza cuando el receptor crea un secreto de pago aleatorio y envía su hash al remitente. 
Tenga en cuenta que para los nodos intermediarios, todos los hashes parecen aleatorios. 
No hay forma de saber si un hash corresponde a un secreto real o si se generó aleatoriamente.

El ataque de sondeo procede de la siguiente manera.
Digamos que el atacante Mallory quiere revelar el saldo de Alice de un canal público entre Alice y Bob. 
Supongamos que la capacidad total de ese canal es de 1 millón de satoshis. 
El saldo de Alice puede oscilar entre cero y 1 millón de satoshis (para ser precisos, la estimación es un poco más ajustada debido a la reserva de canales, pero no la tomamos en cuenta aquí por simplicidad).
Mallory abre un canal con Alice con 1 millón de satoshis y envía 500 000 satoshis a Bob a través de Alice usando un _número aleatorio_ como hash de pago. 
Por supuesto, este número no corresponde a ningún secreto de pago conocido. Por lo tanto, el pago fallará. 
La pregunta es: ¿cómo fallará exactamente? 

Existen dos escenarios.
Si Alice posee mas de 500.000 satoshis en su lado del canal con Bob, ella envia el pago.

Bob descifra la cebolla de pago y se da cuenta de que el pago está destinado a él.
Busca en su tienda local de secretos de pago y busca la preimagen que corresponde al hash de pago, pero no la encuentra.
Siguiendo el protocolo, Bob devuelve el error de "hash de pago desconocido" a Alice, quien se lo transmite a Mallory.
Como resultado, Mallory sabe que el pago _podría haber tenido éxito_ si el hash del pago fuera real.
Por lo tanto, Mallory puede actualizar su estimación del saldo de Alice de "entre cero y 1 millón" a "entre 500.000 y 1 millón".
Otro escenario ocurre si el saldo de Alice es inferior a 500.000 satoshis.
En ese caso, Alice no puede envíar el pago y devuelve el error de "saldo insuficiente" a Mallory.
Mallory actualiza su estimación de "entre cero y 1 millón" a "entre cero y 500.000".

Tenga en cuenta que, en cualquier caso, la estimación de Mallory se vuelve el doble de precisa después de un solo sondeo.
Puede continuar sondeando, eligiendo la siguiente cantidad de sondeo de modo que divida el intervalo de estimación actual por la mitad.
((("búsqueda binaria"))) Esta conocida técnica de búsqueda se llama _búsqueda binaria_.
Con la búsqueda binaria, el número de sondas es _logarítmico_ con la precisión deseada.
Por ejemplo, para obtener el saldo de Alice en un canal de 1 millón de satoshis hasta un solo satoshi, Mallory solo tendría que realizar log~2~ (1.000.000) ≈ 20 sondeos.
Si un sondeo tarda 3 segundos, ¡un canal se puede sondear con precisión en solo un minuto!

El sondeo de canales se puede hacer aún más eficiente.
En su variante más simple, Mallory se conecta directamente al canal que quiere sondear.
¿Es posible sondear un canal sin abrir un canal a uno de sus puntos finales?
Imagine que Mallory ahora quiere probar un canal entre Bob y Charlie, pero no quiere abrir otro canal, lo que requiere pagar tarifas en cadena y esperar confirmaciones de las transacciones de financiación.
En cambio, Mallory reutiliza su canal existente a Alice y envía una sonda a lo largo de la ruta Mallory -> Alice -> Bob -> Charlie.
Mallory puede interpretar el error "hash de pago desconocido" de la misma manera que antes: la sonda ha llegado al destino; por lo tanto, todos los canales a lo largo de la ruta tienen saldos suficientes para reenviarlo.
Pero, ¿y si Mallory recibe el error de "saldo insuficiente"?
¿Significa que el equilibrio es insuficiente entre Alice y Bob o entre Bob y Charlie?

En el protocolo Lightning actual, los mensajes de error informan no solo _cuál_ error ocurrió sino también _dónde_ sucedió.
Entonces, con un manejo de errores más cuidadoso, Mallory ahora sabe qué canal falló.
Si este es el canal objetivo, actualiza sus estimaciones; si no, elige otra ruta hacia el canal de destino.
Incluso obtiene información _adicional_ sobre los saldos de los canales intermediarios, además de la del canal de destino.

El ataque de sondeo se puede utilizar además para vincular remitentes y receptores, como se describe en la sección anterior.

En este punto, puede preguntarse: ¿por qué Lightning Network hace un trabajo tan pobre en la protección de los datos privados de sus usuarios?
¿No sería mejor no revelar al remitente por qué y dónde ha fallado el pago?
De hecho, esto podría ser una contramedida potencial, pero tiene importantes inconvenientes.
Lightning tiene que lograr un cuidadoso equilibrio entre privacidad y eficiencia.
Recuerde que los nodos regulares no conocen las distribuciones de saldos en los canales remotos.
Por lo tanto, los pagos pueden fallar (y a menudo lo hacen) debido a un saldo insuficiente en un salto intermediario.
Los mensajes de error permiten al remitente excluir el canal que falla al construir otra ruta.
Una billetera Lightning popular incluso realiza un sondeo interno para verificar si una ruta construida realmente puede manejar un pago.

Existen otras contramedidas potenciales contra el sondeo de canales.
Primero, es difícil para un atacante apuntar a canales no anunciados.
En segundo lugar, los nodos que implementan enrutamiento justo a tiempo (JIT) pueden ser menos propensos al ataque.
Finalmente, dado que los pagos de varias partes hacen que el problema de la capacidad insuficiente sea menos grave, los desarrolladores del protocolo pueden considerar ocultar algunos de los detalles del error sin dañar la eficiencia.
(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc8")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc7")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc6")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc5")))

[[denegacion_de_servicio]]
==== Denegación de Servicio

((("violación de la privacidad","ataques de denegación de servicio", id="ix_16_security_privacy_ln-asciidoc9", range="startofrange")))((("ataques denegación-de-servicio (DoS)", id="ix_16_security_privacy_ln-asciidoc10", range="startofrange")))Cuando los recursos se ponen a disposición del público, existe el riesgo de que los atacantes intenten hacer que ese recurso no esté disponible mediante la ejecución de un ataque de denegación de servicio o "denial of service" (DoS).
Generalmente, esto se logra cuando el atacante bombardea un recurso con solicitudes, que son indistinguibles de las consultas legítimas.
Los ataques rara vez dan como resultado que el objetivo sufra pérdidas financieras, aparte del costo de oportunidad de la caída de su servicio, y simplemente tienen la intención de agraviar al objetivo.

Las mitigaciones típicas de los ataques DoS requieren la autenticación de las solicitudes para separar a los usuarios legítimos de los malintencionados. Estas mitigaciones incurren en un costo trivial para los usuarios regulares, pero actuarán como un impedimento suficiente para que un atacante inicie solicitudes a gran escala.
Las medidas contra la denegación de servicio se pueden ver en todas partes en Internet: los sitios web aplican límites de velocidad para garantizar que ningún usuario pueda consumir toda la atención de su servidor, los sitios de reseñas de películas requieren autenticación de inicio de sesión para mantenerse enojado r/prequelmemes (grupo Reddit) miembros a raya, y los servicios de datos venden claves API para limitar el número de consultas.

===== DoS en Bitcoin

((("Bitcoin (sistema)","Ataques DoS")))((("ataques denegación-de-servicio (DoS)","DoS en Bitcoin")))En Bitcoin, el ancho de banda que utilizan los nodos para transmitir transacciones y el espacio que aprovechan para la red en forma de su mempool son recursos disponibles públicamente.
Cualquier nodo de la red puede consumir ancho de banda y espacio de mempool enviando una transacción válida.
Si esta transacción se extrae en un bloque válido, pagarán tarifas de transacción, lo que agrega un costo al uso de estos recursos de red compartidos.

En el pasado, la red Bitcoin se enfrentó a un intento de ataque DoS en el que los atacantes enviaron spam a la red con transacciones de bajo costo.
Muchas de estas transacciones no fueron seleccionadas por los mineros debido a sus bajas tarifas de transacción, por lo que los atacantes podían consumir recursos de la red sin pagar las tarifas.
Para abordar este problema, se estableció una tarifa mínima de retransmisión de transacciones que establece una tarifa de umbral que los nodos requieren para propagar transacciones.
Esta medida aseguró en gran medida que las transacciones que consumen recursos de la red finalmente pagarán sus tarifas de cadena.
La tarifa mínima de retransmisión es aceptable para los usuarios habituales, pero perjudicaría financieramente a los atacantes si intentaran enviar spam a la red.
Si bien es posible que algunas transacciones no se conviertan en bloques válidos en entornos de tarifas altas, estas medidas han sido en gran medida efectivas para disuadir este tipo de spam.

===== DoS en Lightning

((("ataques denegación-de-servicio (DoS)","DoS en Lightning")))De manera similar a Bitcoin, Lightning Network cobra tarifas por el uso de sus recursos públicos, pero en este caso, los recursos son canales públicos y las tarifas vienen en forma de tarifas de enrutamiento. La capacidad de enrutar pagos a través de nodos a cambio de tarifas brinda a la red un gran beneficio de escalabilidad (los nodos que no están conectados directamente aún pueden realizar transacciones), pero tiene el costo de exponer un recurso público que debe protegerse contra ataques DoS. 
Cuando un nodo Lightning reenvía un pago en su nombre, utiliza datos y ancho de banda de pago para actualizar su transacción de compromiso, y el monto del pago se reserva en el saldo de su canal hasta que se liquide o falle. En pagos exitosos, esto es aceptable porque el nodo finalmente paga sus tarifas. Los pagos fallidos no incurren en cargos en el protocolo actual. Esto permite que los nodos enruten sin costo los pagos fallidos a través de cualquier canal. Esto es excelente para usuarios legítimos, a quienes no les gustaría pagar por intentos fallidos, pero también permite a los atacantes consumir los recursos de los nodos sin costo, al igual que las transacciones de bajo costo en Bitcoin que nunca terminan pagando las tarifas de los mineros.

En el momento de escribir este artículo, hay un debate https://lists.linuxfoundation.org/pipermail/lightning-dev/2020-June/002734.html[en curso] en la lista de correo de lightning-dev sobre la mejor manera de abordar este problema.

===== Ataque conocidos de DoS

((("ataques denegación-de-servicio (DoS)","ataque conocidos de DoS")))Hay dos ataques DoS conocidos en canales LN públicos que inutilizan un canal de destino, o un conjunto de canales de destino.
Ambos ataques implican el enrutamiento de pagos a través de un canal público y luego retenerlos hasta su tiempo de espera, lo que maximiza la duración del ataque.
El requisito de fallar en los pagos para no pagar las tarifas es bastante simple de cumplir porque los nodos maliciosos pueden simplemente redirigir los pagos hacia ellos mismos.
En ausencia de tarifas por pagos fallidos, el único costo para el atacante es el costo en cadena de abrir un canal para enviar estos pagos, lo que puede ser trivial en entornos de tarifas bajas.(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc10")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc9")))

==== Commitment Jamming o Interferencia de compromiso

((("violación de la privacidad","commitment jamming")))((("commitment jamming")))Los nodos Lightning actualizan su estado compartido mediante transacciones de compromiso asimétricas, en las que se agregan y eliminan HTLC para facilitar los pagos.
Cada parte está limitada a un total de https://github.com/lightningnetwork/lightning-rfc/blob/c053ce7afb4cbf88615877a0d5fc7b8dbe2b9ba0/02-peer-protocol.md#the-open_channel-message[483] HTLC en la transacción de compromiso a la vez.
Un ataque de interferencia de canal permite que un atacante inutilice un canal enrutando 483 pagos a través del canal de destino y reteniéndolos hasta que se agote el tiempo de espera.

It should be noted that this limit was chosen in the specification to ensure that all the HTLCs can be swept in a https://github.com/lightningnetwork/lightning-rfc/blob/master/05-onchain.md#penalty-transaction-weight-calculation[single justice transaction].
While this limit _may_ be increased, transactions are still limited by the block size, so the number of slots available is likely to remain limited.

Cabe señalar que este límite se eligió en la especificación para garantizar que todos los HTLC se puedan barrer en una https://github.com/lightningnetwork/lightning-rfc/blob/master/05-onchain.md#penalty-transaction-peso-cálculo[transacción única de justicia].
Si bien este límite _puede_ aumentarse, las transacciones aún están limitadas por el tamaño del bloque, por lo que es probable que la cantidad de espacios disponibles siga siendo limitada.

==== Channel Liquidity Lockup o Bloqueo de liquidez del canal

((("violación de la privacidad","channel liquidity lockup")))((("channel liquidity lockup")))Un ataque de bloqueo de liquidez del canal es comparable a un ataque de bloqueo del canal en el sentido de que enruta los pagos a través de un canal y los retiene para que el canal quede inutilizable.
En lugar de bloquear espacios en el compromiso del canal, este ataque enruta grandes HTLC a través de un canal de destino, consumiendo todo el ancho de banda disponible del canal.
El compromiso de capital de este ataque es más alto que el ataque de interferencia de compromiso porque el nodo atacante necesita más fondos para enrutar los pagos fallidos a través del objetivo.(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc3")))

=== Cross-Layer De-Anonymization o Desanonimización de capas cruzadas

((("violación de la privacidad","cross-layer de-anonymization", id="ix_16_security_privacy_ln-asciidoc11", range="startofrange")))((("cross-layer de-anonymization", id="ix_16_security_privacy_ln-asciidoc12", range="startofrange")))((("seguridad y privacidad","cross-layer de-anonymization", id="ix_16_security_privacy_ln-asciidoc13", range="startofrange")))Las redes informáticas suelen estar en capas.
La estratificación permite la separación de preocupaciones y hace que todo el sistema sea manejable.
Nadie podría diseñar un sitio web si requiriera comprender toda la pila de TCP/IP hasta la codificación física de bits en un cable óptico.
Se supone que cada capa proporciona la funcionalidad a la capa superior de una manera limpia.
Idealmente, la capa superior debería percibir una capa inferior como una caja negra.
En realidad, sin embargo, las implementaciones no son ideales y los detalles se filtran a la capa superior.
Este es el problema de las abstracciones con fugas.

En el contexto de Lightning, el protocolo LN se basa en el protocolo Bitcoin y la red LN P2P.
Hasta este punto, solo consideramos las garantías de privacidad que ofrece Lightning Network de forma aislada.
Sin embargo, la creación y el cierre de canales de pago se realizan inherentemente en la cadena de bloques de Bitcoin.
En consecuencia, para un análisis completo de las disposiciones de privacidad de Lightning Network, es necesario considerar cada capa de la pila tecnológica con la que los usuarios podrían interactuar.
Específicamente, un adversario anonimizado puede y usará datos dentro y fuera de la cadena para agrupar o vincular nodos LN a las direcciones de Bitcoin correspondientes.

Los atacantes que intentan eliminar el anonimato de los usuarios de LN pueden tener varios objetivos, en un contexto de capas cruzadas:

  * Clúster de direcciones Bitcoin propiedad del mismo usuario (Capa 1). Llamamos a estas entidades Bitcoin.
  * Nodos de LN de clúster que es propiedad del mismo usuario (Capa 2).
  * Vincular sin ambigüedades los conjuntos de nodos LN a los conjuntos de entidades Bitcoin que los controlan.

Hay varias heurísticas y patrones de uso que permiten a un adversario agrupar direcciones de Bitcoin y nodos de LN propiedad de los mismos usuarios de LN.
Además, estos clústeres se pueden vincular a través de capas utilizando otras potentes heurísticas de vinculación entre capas.
El último tipo de heurística, las técnicas de enlace entre capas, enfatiza la necesidad de una visión holística de la privacidad. Específicamente, debemos considerar la privacidad en el contexto de ambas capas juntas.


==== Agrupación de entidades de Bitcoin On-chain 
((("Entidades Bitcoin","entity clustering")))((("cross-layer de-anonymization","on-chain Bitcoin entity clustering")))((("on-chain Bitcoin entity clustering")))Las interacciones de la cadena de bloques Lightning Network se reflejan permanentemente en el gráfico de entidades de Bitcoin.
Incluso si un canal está cerrado, un atacante puede observar qué dirección fondeó el canal y dónde se gastaron las monedas después de cerrarlo.
Para este análisis, consideremos cuatro entidades separadas.
La apertura de un canal provoca un flujo monetario de una _entidad origen ("source")_ a una _entidad financiadora ("funding")_; el cierre de un canal provoca un flujo desde una _entidad de liquidación ("settlement")_ a una _entidad de destino ("destination")_.

A principios de 2021, https://arxiv.org/pdf/2007.00764.pdf[Romiti et al.] identificó cuatro heurísticas que permiten la agrupación de estas entidades.
Dos de ellos capturan cierto comportamiento de financiación con fugas y dos describen comportamientos de liquidación con fugas.

Heurística de estrella (financiación):: Si un componente contiene una entidad de origen que reenvía fondos a una o más entidades de financiación, es probable que estas entidades de financiación estén controladas por el mismo usuario.
Heurística de serpiente (financiación):: si un componente contiene una entidad de origen que reenvía fondos a una o más entidades, que a su vez se utilizan como entidades de origen y de financiación, es probable que todas estas entidades estén controladas por el mismo usuario.
Heurística del recopilador (liquidación):: si un componente contiene una entidad de destino que recibe fondos de una o más entidades de liquidación, es probable que estas entidades de liquidación estén controladas por el mismo usuario.
Proxy heurístico (liquidación):: Si un componente contiene una entidad de destino que recibe fondos de una o más entidades, que a su vez se utilizan como entidades de liquidación y destino, es probable que estas entidades estén controladas por el mismo usuario.

Vale la pena señalar que estas heurísticas pueden producir falsos positivos.
Por ejemplo, si las transacciones de varios usuarios no relacionados se combinan en una transacción CoinJoin, entonces la estrella o la heurística de proxy pueden producir falsos positivos.
Esto podría suceder si los usuarios están financiando un canal de pago a partir de una transacción CoinJoin.
Otra fuente potencial de falsos positivos podría ser que una entidad pudiera representar a varios usuarios si las direcciones agrupadas están controladas por un servicio (por ejemplo, intercambio) o en nombre de sus usuarios (cartera de custodia).
Sin embargo, estos falsos positivos se pueden filtrar de manera efectiva.

===== Contramedidas
Si los resultados de las transacciones de financiación no se reutilizan para abrir otros canales, la heurística de la serpiente no funciona.
Si los usuarios se abstienen de utilizar canales de financiación de una única fuente externa y evitan recaudar fondos en una única entidad de destino externa, las otras heurísticas no arrojarían ningún resultado significativo.

==== Agrupación Off-Chain de nodos Lightning
((("cross-layer de-anonymization","off-chain Lightning node clustering")))((("Lightning node clustering")))((("off-chain Lightning node clustering")))Los nodos de LN anuncian alias, por ejemplo, _LNBig.com_.
Los alias pueden mejorar la usabilidad del sistema.
Sin embargo, los usuarios tienden a usar alias similares para sus propios nodos diferentes.
Por ejemplo, es probable que _LNBig.com Billing_ sea propiedad del mismo usuario que el nodo con el alias _LNBig.com_.
Dada esta observación, uno puede agrupar nodos LN aplicando sus alias de nodo.
Específicamente, uno agrupa los nodos LN en una sola dirección si sus alias son similares con respecto a alguna métrica de similitud de cadenas.
Otro método para agrupar nodos LN es aplicar sus direcciones IP o Tor.
Si las mismas direcciones IP o Tor corresponden a diferentes nodos LN, es probable que estos nodos estén controlados por el mismo usuario.

===== Countermeasures
For more privacy, aliases should be sufficiently different from one another.
While the public announcement of IP addresses may be unavoidable for those nodes that wish to have incoming channels in the Lightning Network, linkability across nodes of the same user can be mitigated if the clients for each node are hosted with different service providers and thus IP addresses.

==== Enlace de capa cruzada o Cross-Layer Linking: Nodos Lightning y Entidades Bitcoin
((("Bitcoin entities","cross-layer linking to Lightning nodes")))((("violación de la privacidad","cross-layer linking: Lightning nodes and Bitcoin entities")))((("cross-layer de-anonymization","cross-layer linking: Lightning nodes and Bitcoin entities")))((("Lightning node operation","cross-layer linking to Bitcoin entities")))Asociar nodos LN a entidades Bitcoin es una violación grave de la privacidad que se ve agravada por el hecho de que la mayoría de los nodos LN exponen públicamente sus direcciones IP.
Por lo general, una dirección IP se puede considerar como un identificador único de un usuario.
Dos patrones de comportamiento ampliamente observados revelan vínculos entre los nodos LN y las entidades de Bitcoin:

Reutilización de monedas:: Cada vez que los usuarios cierran los canales de pago, recuperan sus monedas correspondientes. Sin embargo, muchos usuarios reutilizan esas monedas para abrir un nuevo canal.
Esas monedas se pueden vincular efectivamente a un nodo LN común.

Reutilización de entidades:: por lo general, los usuarios financian sus canales de pago desde direcciones de Bitcoin correspondientes a la misma entidad de Bitcoin.

Estos algoritmos de vinculación de capas cruzadas podrían frustrarse si los usuarios poseen múltiples direcciones no agrupadas o usan múltiples billeteras para interactuar con Lightning Network.

La posible anonimización de las entidades de Bitcoin ilustra lo importante que es considerar la privacidad de ambas capas simultáneamente en lugar de una a la vez.(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc13")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc12")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc11")))

//TODO from author:  maybe here we should/could include the corresponding figures from the Romiti et al. paper. it would greatly improve and help the understanding of the section

=== Lightning Graph

((("Lightning graph", id="ix_16_security_privacy_ln-asciidoc14", range="startofrange")))((("seguridad y privacidad","Lightning graph", id="ix_16_security_privacy_ln-asciidoc15", range="startofrange")))The Lightning Network, as the name suggests, is a peer-to-peer network of payment channels.
Therefore, many of its properties (privacy, robustness, connectivity, routing efficiency) are influenced and characterized by its network nature.

In this section, we discuss and analyze the Lightning Network from the point of view of network science.
We are particularly interested in understanding the LN channel graph, its robustness, connectivity, and other important characteristics.

==== How Does the Lightning Graph Look in Reality?
((("Lightning graph","reality versus theoretical appearance of", id="ix_16_security_privacy_ln-asciidoc16", range="startofrange")))One could have expected that the Lightning Network is a random graph, where edges are randomly formed between nodes.
If this was the case, then the Lightning Network's degree distribution would follow a Gaussian normal distribution.
In particular, most of the nodes would have approximately the same degree, and we would not expect nodes with extraordinarily large degrees.
This is because the normal distribution exponentially decreases for values outside of the interval around the average value of the distribution.
The depiction of a random graph (as we saw in <<lngraph>>) looks like a mesh network topology.
It looks decentralized and nonhierarchical: every node seems to have equal importance.
Additionally, random graphs have a large diameter.
In particular, routing in such graphs is challenging because the shortest path between any two nodes is moderately long.

However, in stark contrast, the LN graph is completely different.

===== Lightning graph today
Lightning is a financial network.
Thus, the growth and formation of the network are also influenced by economic incentives.
Whenever a node joins the Lightning Network, it may want to maximize its connectivity to other nodes in order to increase its routing efficiency. This phenomenon is called preferential attachment.
These economic incentives result in a fundamentally different network than a random graph.

Based on snapshots of publicly announced channels, the degree distribution of the Lightning Network follows a power-law function.
In such a graph, the vast majority of nodes have very few connections to other nodes, while only a handful of nodes have numerous connections.
At a high level, this graph topology resembles a star: the network has a well-connected core and a loosely connected periphery.
Networks with power-law degree distribution are also called scale-free networks.
This topology is advantageous for routing payments efficiently but prone to certain topology-based attacks.

===== Topology-based attacks

((("Lightning graph","topology-based attacks")))((("topology-based attacks")))An adversary might want to disrupt the Lightning Network and may decide its goal is to dismantle the whole network into many smaller components, making payment routing practically impossible in the whole network.
A less ambitious, but still malicious and severe goal might be to only take down certain network nodes.
Such a disruption might occur on the node level or on the edge level.

Let's suppose an adversary can take down any node in the Lightning Network.
For instance, it can attack them with a distributed denial of service (DDoS) attack or make them nonoperational by any means.
It turns out that if the adversary chooses nodes randomly, then scale-free networks like the Lightning Network are robust against node-removal attacks.
This is because a random node lies on the periphery with a small number of connections, therefore playing a negligible role in the network's connectivity.
However, if the adversary is more prudent, it can target the most well-connected nodes.
Not surprisingly, the Lightning Network and other scale-free networks are _not_ robust against targeted node-removal attacks.

On the other hand, the adversary could be more stealthy.
Several topology-based attacks target a single node or a single payment channel.
For example, an adversary might be interested in exhausting a certain payment channel's capacity on purpose.
More generally, an adversary can deplete all the outgoing capacity of a node to knock it down from the routing market.
This could be easily obtained by routing payments through the victim node with amounts equalling the outgoing capacity of each payment channel.
After completing this so-called node isolation attack, the victim cannot send or route payments anymore unless it receives a payment or rebalances its channels.

To conclude, even by design, it is possible to remove edges and nodes from the routable Lightning Network.
However, depending on the utilized attack vector, the adversary may have to provide more or fewer resources to carry out the attack.


===== Temporality of the Lightning Network

((("Lightning graph","temporality of Lightning Network and")))((("temporality of Lightning Network")))The Lightning Network is a dynamically changing, permissionless network.
Nodes can freely join or leave the network, they can open and create payment channels anytime they want.
Therefore, a single static snapshot of the LN graph is misleading. We need to consider the temporality and ever-changing nature of the network. For now, the LN graph is growing in terms of the number of nodes and payment channels.
Its effective diameter is also shrinking; that is, nodes become closer to each other, as we can see in <<temporal_ln>>.

[[temporal_ln]]
.The steady growth of the Lightning Network in nodes, channels, and locked capacity (as of September 2021)
image::images/mtln_1602.png["The steady growth of the Lightning Network in terms of nodes, channels, and locked capacity (as of September 2021)"]

In social networks, triangle closing behavior is common.
Specifically, in a graph where nodes represent people and friendships are represented as edges, it is somewhat expected that triangles will emerge in the graph.
A triangle, in this case, represents pairwise friendships between three people.
For instance, if Alice knows Bob and Bob knows Charlie, then it is likely that at some point Bob will introduce Alice to Charlie.
However, this behavior would be strange in the Lightning Network.
Nodes are simply not incentivized to close triangles because they could have just routed payments instead of opening a new payment channel.
Surprisingly, triangle closing is a common practice in the Lightning Network.
The number of triangles was steadily growing before the implementation of multipart payments.
This is counterintuitive and surprising given that nodes could have just routed payments through the two sides of the triangle instead of opening the third channel.
This may mean that routing inefficiencies incentivized users to close triangles and not fall back on routing.
Hopefully, multipart payments will help increase the effectiveness of payment routing(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc16"))).(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc15")))(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc14")))

=== Centralization in the Lightning Network

((("betweenness centrality")))((("central point dominance")))((("centralization, Lightning Network and")))((("seguridad y privacidad","centralization in Lightning Network")))A common metric to assess the centrality of a node in a graph is its _betweenness centrality_. Central point dominance is a metric derived from betweenness centrality, used to assess the centrality of a network.
For a precise definition of central point dominance, the reader is referred to https://doi.org/10.2307/3033543[Freeman's work].

The larger the central point dominance of a network is, the more centralized the network is.
We can observe that the Lightning Network has a greater central point dominance (i.e., it is more centralized) than a random graph (Erdős–Rényi graph) or a scale-free graph (Barabási–Albert graph) of equal size.

In general, our understanding of the dynamic nature of the LN channel graph is rather limited.
It is fruitful to analyze how protocol changes like multipart payments can affect the dynamics of the Lightning Network.
It would be beneficial to explore the temporal nature of the LN graph in more depth.

=== Economic Incentives and Graph Structure

((("Lightning graph","economic incentives and graph structure")))((("seguridad y privacidad","economic incentives and graph structure")))The LN graph forms spontaneously, and nodes connect to each other based on mutual interest.
As a result, incentives drive graph development.
Let's look at some of the relevant incentives:

  * Rational incentives:
    - Nodes establish channels to send, receive, and route payments (earn fees).
    - What makes a channel more likely to be established between two nodes that act rationally?
  * Altruistic incentives:
    - Nodes establish channels "for the good of the network."
    - While we should not base our security assumptions on altruism, to a certain extent, altruistic behavior drives Bitcoin (accepting incoming connections, serving blocks).
    - What role does it play in Lightning?

In the early stages of the Lightning Network, many node operators have claimed that the earned routing fees do not compensate for the opportunity costs stemming from liquidity lock-up. This would indicate that operating a node may be driven mostly by altruistic incentives "for the good of the network."
This might change in the future if the Lightning Network has significantly larger traffic or if a market for routing fees emerges.
On the other hand, if a node wishes to optimize its routing fees, it would minimize the average shortest path lengths to every other node.
Put differently, a profit-seeker node will try to locate itself in the _center_ of the channel graph or close pass:[<span class="keep-together">to it</span>].

=== Practical Advice for Users to Protect Their Privacy

((("seguridad y privacidad","practical advice for users to protect privacy")))We're still in the early stages of the Lightning Network.
Many of the concerns listed in this chapter are likely to be addressed as it matures and grows.
In the meantime, there are some measures that you can take to guard your node against malicious users; something as simple as updating the default parameters that your node runs with can go a long way in hardening your node.

=== Unannounced Channels

((("payment channel","unannounced channels")))((("seguridad y privacidad","unannounced channels")))((("unannounced channels")))If you intend to use the Lightning Network to send and receive funds between nodes and wallets you control, and have no interest in routing other users' payments, there is little need to announce your channels to the rest of the network.
You could open a channel between, say, your desktop PC running a full node and your mobile phone running a Lightning wallet, and simply forgo the channel announcement discussed in <<ch03_How_Lightning_Works>>.
These are sometimes called "private" channels; however, it is more correct to refer to them as "unannounced" channels because they are not strictly private.

Unannounced channels will not be known to the rest of the network and won't normally be used to route other users' payments.
They can still be used to route payments if other nodes are made aware of them; for example, an invoice could contain routing hints which suggests a path with an unannounced channel.
However, assuming that you've only opened an unannounced channel with yourself, you do gain some measure of privacy.
Since you are not exposing your channel to the network, you lower the risk of a denial-of-service attack on your node.
You can also more easily manage the capacity of this channel, since it will only be used to receive or send directly to your node.

There are also advantages to opening an unannounced channel with a known party that you transact with frequently.
For example, if Alice and Bob frequently play poker for bitcoin, they could open a channel to send their winnings back and forth.
Under normal conditions, this channel will not be used to route payments from other users or collect fees.
And since the channel will not be known to the rest of the network, any payments between Alice and Bob cannot be inferred by tracking changes in the channel's routing capacity.
This confers some privacy to Alice and Bob; however, if one of them decides to make other users aware of the channel, such as by including it in the routing hints of an invoice, then this privacy is lost.

It should also be noted that to open an unannounced channel, a public transaction must be made on the Bitcoin blockchain.
Hence it is possible to infer the existence and size of the channel if a malicious party is monitoring the blockchain for channel opening transactions and attempting to match them to channels on the network.
Furthermore, when the channel is closed, the final balance of the channel will be made public once it's committed to the Bitcoin blockchain.
However, since the opening and commitment transactions are pseudonymous, it will not be a simple matter to connect it back to Alice or Bob.
In addition, the Taproot update of 2021 makes it difficult to distinguish between channel opening and closing transactions and other specific kinds of Bitcoin transactions.
Hence, while unannouned channels are not completely private, they do provide some privacy benefits when used carefully.

[[routing_considerations]]
=== Routing Considerations

((("denial-of-service (DoS) attacks","protecting against")))((("routing","security/privacy considerations")))((("seguridad y privacidad","routing considerations")))As covered in <<denial_of_service>>, nodes that open public channels expose themselves to the risk of a series of attacks on their channels.
While mitigations are being developed on the protocol level, there are many steps that a node can take to protect against denial of service attacks on their public channels:

Minimum HTLC size:: On channel open, your node can set the minimum HTLC size that it will accept.
Setting a higher value ensures that each of your available channel slots cannot be occupied by a very small payment.
Rate limiting:: Many node implementations allow nodes to dynamically accept or reject HTLCs that are forwarded through your node.
Some useful guidelines for a custom rate limiter are as follows:
+
** Limit the number of commitment slots a single peer may consume
** Monitor failure rates from a single peer, and rate limit if their failures spike suddenly
Shadow channels:: Nodes that wish to open large channels to a single target can instead open a single public channel to the target and support it with further private channels called pass:[<a href='https://anchor.fm/tales-from-the-crypt/episodes/197-Joost-Jager-ekghn6'>shadow channels</a>]. These channels can still be used for routing but are not announced to potential attackers.

==== Accepting Channels
((("routing","accepting channels")))At present, Lightning nodes struggle with bootstrapping inbound liquidity. While there are some paid
solutions to acquiring inbound liquidity, like swap services, channel markets, and paid channel opening services from known hubs, many nodes will gladly accept any legitimate looking channel opening request to increase their inbound liquidity.

Stepping back to the context of Bitcoin, this can be compared to the way that Bitcoin Core treats its incoming and outgoing connections differently out of concern that the node may be eclipsed.
If a node opens an incoming connection to your Bitcoin node, you have no way of knowing whether the initiator randomly selected you or is specifically targeting your node with malicious intent.
Your outgoing connections do not need to be treated with such suspicion because either the node was selected randomly from a pool of many potential peers or you intentionally connected to the peer manually.

The same can be said in Lightning.
When you open a channel, it is done with intention, but when a remote party opens a channel to your node, you have no way of knowing whether this channel will be used to attack your node or not.
As several papers note, the relatively low cost of spinning up a node and opening channels to targets is one of the significant factors that make attacks easy.
If you accept incoming channels, it is prudent to place some restrictions on the nodes you accept incoming channels from.
Many implementations expose channel acceptance hooks that allow you to tailor your channel acceptance policies to your preferences.

The question of accepting and rejecting channels is a philosophical one.
What if we end up with a Lightning Network where new nodes cannot participate because they cannot open any channels?
Our suggestion is not to set an exclusive list of "mega-hubs" from which you will accept channels, but rather to accept channels in a manner that suits your risk preference.

Some potential strategies are:

No risk:: Do not accept any incoming channels.
Low risk:: Accept channels from a known set of nodes that you have previously had successful channels open with.
Medium risk:: Only accept channels from nodes that have been present in the graph for a longer period and have some long-lived channels.
Higher risk:: Accept any incoming channels, and implement the mitigations described in <<routing_considerations>>.

=== Conclusion
In summary, privacy and security are nuanced, complex topics, and while many researchers and developers are looking for network-wide improvements, it's important for everyone participating in the network to understand what they can do to protect their own privacy and increase security on an individual node level.

=== References and Further Reading

In this chapter, we used many references from ongoing research on Lightning security. You may find these useful articles and papers listed by topic in the following lists.

==== Privacy and probing attacks

* Jordi Herrera-Joancomartí et al. https://eprint.iacr.org/2019/328["On the Difficulty of Hiding the Balance of Lightning Network Channels"]. _Asia CCS '19: Proceedings of the 2019 ACM Asia Conference on Computer and Communications Security_ (July 2019): 602–612.
* Utz Nisslmueller et al. "Toward Active and Passive Confidentiality Attacks on Cryptocurrency Off-Chain Networks." arXiv preprint, https://arxiv.org/abs/2003.00003[] (2020).
* Sergei Tikhomirov et al. "Probing Channel Balances in the Lightning Network." arXiv preprint, https://arxiv.org/abs/2004.00333[] (2020).
* George Kappos et al. "An Empirical Analysis of Privacy in the Lightning Network." arXiv preprint, https://arxiv.org/abs/2003.12470[] (2021).
* https://github.com/LN-Zap/zap-desktop/blob/v0.7.2-beta/services/grpc/router.methods.js[Zap source code with the probing function].

===== Congestion attacks

* Ayelet Mizrahi and Aviv Zohar. "Congestion Attacks in Payment Channel Networks." arXiv preprint, https://arxiv.org/abs/2002.06564[] (2020).

===== Routing considerations

* Marty Bent, interview with Joost Jager, _Tales from the Crypt_, podcast audio, October 2, 2020, https://anchor.fm/tales-from-the-crypt/episodes/197-Joost-Jager-ekghn6[].(((range="endofrange", startref="ix_16_security_privacy_ln-asciidoc0")))

