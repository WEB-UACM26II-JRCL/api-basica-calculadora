# API REST: Calculadora básica con FastAPI

API REST desarrollada en **Python** con **FastAPI** para realizar cuatro operaciones aritméticas: suma, resta, multiplicación y división. El proyecto permite practicar los métodos HTTP `GET` y `POST`, el envío de parámetros y datos JSON, y la consulta de documentación automática.

## Requisitos

- Python 3 instalado.
- `pip` disponible.

## Archivos del proyecto

```text
.
├── main.py           # Aplicación y endpoints
├── requirements.txt  # Dependencias
├── .gitignore        # Archivos excluidos de Git
└── README.md         # Documentación
```

## Instalación

Desde la carpeta del proyecto, se recomienda crear y activar un entorno virtual:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

En Windows, la activación se realiza con `.venv\Scripts\activate` (CMD) o `.venv\Scripts\Activate.ps1` (PowerShell).

Instalar las dependencias:

```bash
pip install -r requirements.txt
```

El archivo `requirements.txt` incluye `fastapi` y `uvicorn`.

## Ejecución

Iniciar el servidor de desarrollo:

```bash
uvicorn main:app --reload
```

La API estará disponible en **http://127.0.0.1:8000**. La opción `--reload` reinicia automáticamente el servidor cuando cambia el código y se recomienda solo para desarrollo.

## Endpoints

| Método | Ruta | Datos de entrada | Descripción | Código de éxito |
|---|---|---|---|---|
| `GET` | `/sumar` | `a` y `b` en la URL | Suma dos números | `200` |
| `GET` | `/restar` | `a` y `b` en la URL | Resta dos números | `200` |
| `POST` | `/multiplicar` | JSON con `a` y `b` | Multiplica dos números | `201` |
| `POST` | `/dividir` | JSON con `a` y `b` | Divide dos números | `200` |

Los valores `a` y `b` son numéricos. FastAPI valida los datos de entrada mediante los tipos definidos en `main.py`.

### GET: suma y resta

Estas operaciones reciben los valores como **parámetros de consulta** (`query parameters`). Pueden probarse directamente en el navegador:

- [Sumar 10 + 5](http://127.0.0.1:8000/sumar?a=10&b=5)
- [Restar 10 - 5](http://127.0.0.1:8000/restar?a=10&b=5)

Ejemplo de respuesta de la suma:

```json
{"resultado": 15.0}
```

### POST: multiplicación y división

Estas operaciones reciben los valores en el **cuerpo de la solicitud**, con formato JSON:

```json
{
  "a": 10,
  "b": 5
}
```

Se pueden probar desde Swagger UI o mediante `curl`:

**Multiplicación:**

```bash
curl -X POST http://127.0.0.1:8000/multiplicar \
  -H 'Content-Type: application/json' \
  -d '{"a":10,"b":5}'
```

Respuesta:

```json
{"resultado": 50.0}
```

**División:**

```bash
curl -X POST http://127.0.0.1:8000/dividir \
  -H 'Content-Type: application/json' \
  -d '{"a":10,"b":5}'
```

Respuesta:

```json
{"resultado": 2.0}
```

**Importante:** escribir una ruta POST directamente en la barra de direcciones del navegador no ejecuta una solicitud POST; para ello se puede utilizar Swagger UI o `curl`.

## Validación y errores

- Si se intenta dividir entre cero, `/dividir` responde con el código HTTP `400 Bad Request` y el mensaje `No se puede dividir entre cero`.
- Si faltan parámetros o se envían valores incompatibles con los tipos esperados, FastAPI responde normalmente con `422 Unprocessable Entity`.

Ejemplo de error al dividir entre cero:

```json
{"detail": "No se puede dividir entre cero"}
```

## Documentación automática

Con el servidor en ejecución, FastAPI proporciona dos interfaces:

- **[Swagger UI](http://127.0.0.1:8000/docs):** documentación interactiva que permite seleccionar un endpoint, ingresar parámetros o JSON y ejecutar solicitudes con **Try it out**.
- **[ReDoc](http://127.0.0.1:8000/redoc):** documentación de consulta organizada por rutas, modelos y respuestas; no incluye el mismo mecanismo interactivo de pruebas.

Para probar una operación POST en Swagger UI: abrir `/docs`, seleccionar la operación, pulsar **Try it out**, ingresar los valores y seleccionar **Execute**.

> **Nota:** las direcciones anteriores utilizan `127.0.0.1` para ejecutar las pruebas en la misma computadora. Si el servidor está en otro equipo, sustituir esa dirección por su IP o nombre de host y configurar el acceso de red correspondiente.

