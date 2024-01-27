# PRACTICA-10-ENCENDER-LED-CON-NODE-RED

Dentro de este repositorio se muestra la manera de programar una ESP32 con una luz LED para que sea encendida y apagada a través del programa Node-RED con la ayuda de una conexión WIFI.

## Introducción

### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un indicador (```LED```) para encenderlo y apagarlo a traves de un *switch* en el programa ```Node-red``` donde se mandará una señal de encendido y apagado al ```LED```; cabe aclarar que en esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/) y un programa llamado [Node-RED](http://localhost:1880/).

## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Programa [Node-RED](http://localhost:1880/)
- Tarjeta ESP 32
- LED
- Resistor

## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/) y tener instalado correctamente el programa [Node-RED](http://localhost:1880/).

### Instrucciones de preparación del entorno en wokwi

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "18.193.219.109";
const int mqttPort = 1883;
const char* mqttUser = "diegojm";
const char* mqttPassword = "1234";
const char* mqttTopic = "CRISLED";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}
```

2. Instalar la libreria de **PubSubClient** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N10/blob/main/1.jpg?raw=true)

3. Hacer la conexion del **LED** y el **Resistor** con la **ESP32** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N10/blob/main/2.jpg?raw=true)

### Instrucciones de preparación del entorno en NodeRed

Para ejecutar este flow, es necesario lo siguiente
1. Arrancar el contenedor de NodeRed en **cmd** con el comando
        
        node-red

2. Dirigirse a [localhost:1880](localhost:1880)

3. En la parte izquierda de la pantalla seleccionaremos
   - mqtt out
   - switch

 ![](https://github.com/DanielX834/PRACTICA-N10/blob/main/3.jpg?raw=true)

 4. Para la configuracion del mqtt out necesitaremos saber nuestra ip, que se saca de la siguiente manera:
   cmd --> nslookup broker.hivemq.com --> copiamos los numeros de la parte que dice addresses --> nos dirigimos a nodered --> seleccionamos mqtt out --> damos click en el icono del lapiz --> en la parte de server se pegara la direccion ip que copiamos --> update --> done

![](https://github.com/DanielX834/PRACTICA-N10/blob/main/4.jpg?raw=true)


-Tambien requeriremos de colocar un nombre en la parte donde dice Topic

![](https://github.com/DanielX834/PRACTICA-N10/blob/main/5.jpg?raw=true)


5. Configurar el bloque con el puerto ```switch``` con el grupo de **INTERRUPTORES** como se muestra en la imagen.

![](https://github.com/DanielX834/PRACTICA-N10/blob/main/6.jpg?raw=true)

6. Por ultimo en la pestaña de de *Layout* crearemos otro tabulador llamado **LED**, dentro de el añadiremos un grupo llamado **INTERRUPTORES**.

![](

### Instrucciónes de operación

1. Iniciar simulador en [WOKWI](https://https://wokwi.com/).
2. Visualizar los datos en el monitor serial.
3. Iniciar el simulador en [Node-RED](http://localhost:1880/) dando *click izquierdo* en el botón **Deploy** y despues abrir la interfaz dando *click izquierdo* en el boton de exportar.
4. Visualizar la interfaz y accionar el ```LED```.
5. Observar en el simulador que el **LED** haya encendido.

## Resultados

Cuando haya funcionado, verás la luz del **LED** encendida y los valores dentro del monitor serial, a continuación se puede observar una vista previa del resultado del flow 3 y la interaccion con el wokwi 

![](

# Créditos
Desarrollado por Ing. Daniel Armenta


