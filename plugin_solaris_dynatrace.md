# Desarrollo de un Plugin para Solaris para Monitoreo de Logs e Integración con Dynatrace

## **Objetivo**
Desarrollar un pequeño plugin para Solaris que:
1. Monitoree un archivo de log específico en tiempo real.
2. Busque palabras clave definidas en un archivo de configuración.
3. Cuando se detecte una palabra clave, realice una llamada a la API de Dynatrace para generar un evento personalizado.

---

## **1. Diseño del Plugin**

### **Arquitectura General**
- **Archivo de Configuración:** Define el log a monitorear, las palabras clave y los parámetros de conexión a Dynatrace.
- **Monitor de Logs:** Lee continuamente el archivo de log.
- **Detección de Palabras Clave:** Compara cada nueva línea con las palabras clave definidas.
- **Acción Automatizada:** Si hay coincidencia, se realiza un API Call a Dynatrace.
- **Registro de Actividad:** Guarda registros de las acciones tomadas en un archivo separado.

---

## **2. Requisitos Previos**

### **Dependencias en Solaris**
- Python 3.x (si no está disponible, instálalo).
- Bibliotecas adicionales:
  \`\`\`bash
  pip install requests watchdog
  \`\`\`

---

## **3. Estructura de Archivos**

\`\`\`plaintext
/plugin
├── config.ini      # Configuración del plugin
├── log_monitor.py  # Script principal
├── event_handler.py # Manejador de eventos para Dynatrace
└── requirements.txt # Dependencias necesarias
\`\`\`

### **Archivo de Configuración (`config.ini`)**

\`\`\`ini
[LOG]
log_file=/var/log/application.log

[KEYWORDS]
keywords=ERROR, CRITICAL, FAILURE

[DYNATRACE]
api_endpoint=https://your-dynatrace-instance/api/v2/events/ingest
api_token=your_api_token
event_type=CUSTOM_INFO
entity_id=HOST-123456
\`\`\`

---

## **4. Código Fuente**

### **Script Principal (`log_monitor.py`)**

\`\`\`python
import time
import requests
import configparser
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler

# Cargar configuración
config = configparser.ConfigParser()
config.read('config.ini')

# Parámetros del log y palabras clave
LOG_FILE = config['LOG']['log_file']
KEYWORDS = config['KEYWORDS']['keywords'].split(',')
API_ENDPOINT = config['DYNATRACE']['api_endpoint']
API_TOKEN = config['DYNATRACE']['api_token']
ENTITY_ID = config['DYNATRACE']['entity_id']
EVENT_TYPE = config['DYNATRACE']['event_type']

# Clase de manejador de eventos
class LogHandler(FileSystemEventHandler):
    def __init__(self, log_file):
        self.log_file = log_file
        self.last_position = 0

    def on_modified(self, event):
        if event.src_path == self.log_file:
            with open(self.log_file, 'r') as file:
                file.seek(self.last_position)
                lines = file.readlines()
                self.last_position = file.tell()
            
            for line in lines:
                for keyword in KEYWORDS:
                    if keyword in line:
                        print(f"Keyword encontrada: {keyword}")
                        send_event_to_dynatrace(line, keyword)

# Función para enviar evento a Dynatrace
def send_event_to_dynatrace(message, keyword):
    headers = {
        "Authorization": f"Api-Token {API_TOKEN}",
        "Content-Type": "application/json"
    }
    payload = {
        "eventType": EVENT_TYPE,
        "title": f"Keyword '{keyword}' detected in log",
        "description": message,
        "entitySelector": f"type(HOST),entityId({ENTITY_ID})",
        "properties": {
            "keyword": keyword,
            "log_message": message
        }
    }
    try:
        response = requests.post(API_ENDPOINT, json=payload, headers=headers)
        if response.status_code == 200 or response.status_code == 202:
            print("Evento enviado exitosamente a Dynatrace")
        else:
            print(f"Error al enviar evento: {response.status_code} - {response.text}")
    except Exception as e:
        print(f"Error de conexión con Dynatrace: {str(e)}")

# Iniciar monitor
if __name__ == "__main__":
    event_handler = LogHandler(LOG_FILE)
    observer = Observer()
    observer.schedule(event_handler, path=LOG_FILE, recursive=False)
    observer.start()
    print(f"Monitoreando el log: {LOG_FILE}")

    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
\`\`\`

---

## **5. Despliegue en Solaris**

### **Instalación de Dependencias**
\`\`\`bash
pkg install python3
pip3 install -r requirements.txt
\`\`\`

### **Permisos**
Asegúrate de que el usuario tenga permisos de lectura en el archivo de log y de conexión a internet para API Dynatrace.

### **Ejecución del Plugin**
\`\`\`bash
python3 log_monitor.py
\`\`\`

---

## **6. Seguridad**
- **Protege el archivo de configuración:** Asegúrate de que `config.ini` tenga permisos adecuados.
- **API Token:** Nunca compartas ni expongas el token públicamente.

---

## **7. Conclusión**
Has creado un plugin eficiente que:
- Monitorea archivos de log.
- Detecta palabras clave.
- Notifica eventos personalizados a Dynatrace.

Si necesitas ajustes o agregar más funcionalidades, ¡avísame!
