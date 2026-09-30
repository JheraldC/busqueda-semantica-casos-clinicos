# Búsqueda semántica de casos clínicos con IA

Prototipo académico que recibe una consulta clínica, recupera casos similares mediante embeddings y expone funciones de generación de texto y análisis de imágenes mediante MedGemma. La interfaz permite consultar resultados y su historial.

## Capacidades técnicas

- API con FastAPI y modelos de entrada con Pydantic.
- Embeddings con Sentence Transformers (`all-MiniLM-L6-v2`) y comparación mediante similitud coseno.
- Procesamiento del dataset con pandas.
- Interfaz con React 19, React Router y Tailwind CSS.
- Integración con un servicio de inferencia MedGemma y funciones de procesamiento de respuestas.

El frontend utiliza **Create React App / react-scripts**, según [package.json](frontend/package.json).

## Arquitectura

La interfaz React consulta al backend FastAPI. El backend prepara embeddings de los casos, busca similitudes y, para las funciones generativas, utiliza la configuración de inferencia del código. El servicio complementario está en [API MedGemma](https://github.com/JheraldC/VM-Busqueda-Semantica-con-IA-Casos-Clinicos).

| Ruta | Contenido |
| --- | --- |
| [backend/main.py](backend/main.py) | API, búsqueda semántica y procesamiento de respuestas |
| [backend/dataset_casos_clinicos_OK.csv](backend/dataset_casos_clinicos_OK.csv) | Dataset utilizado por la búsqueda |
| [backend/convertir_xls_csv.py](backend/convertir_xls_csv.py) | Conversión de datos |
| [frontend/src](frontend/src) | Componentes y páginas React |
| [requirements.txt](requirements.txt) | Dependencias Python |

## Instalación local

Crear un entorno Python e instalar dependencias desde la raíz:

```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

El backend utiliza rutas relativas para el dataset y el historial; iniciarlo dentro de `backend`:

```bash
cd backend
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

En otra terminal, desde la raíz:

```bash
cd frontend
npm ci
npm start
```

Interfaz: http://localhost:3000/. Documentación de API: http://127.0.0.1:8000/docs.

La carga inicial descarga el modelo de embeddings. Las funciones generativas requieren acceso al modelo MedGemma y un servicio de inferencia disponible; revisar las direcciones y la selección local/nube en el backend. El código conserva configuración histórica, por lo que no debe suponerse que un servidor externo continúa disponible. El uso local de MedGemma depende de los recursos del equipo y de la configuración del modelo.

## Endpoints presentes

| Método | Ruta | Propósito |
| --- | --- | --- |
| POST | `/buscar` | Consulta estructurada de casos |
| POST | `/buscar_texto` | Consulta en texto libre |
| POST | `/diagnostico_inteligente` | Generación a partir de texto |
| POST | `/diagnostico_imagen` | Procesamiento de imágenes |
| POST | `/similitud_oraciones` | Comparación semántica |
| POST | `/guardarResultado` | Guardado de un resultado |
| GET | `/resultados` | Consulta del historial |

## Documentación y estado

Se conserva el [manual de instalación](Manual%20de%20Instalaci%C3%B3n.docx). La instalación completa, las funciones generativas y los resultados deben validarse en el entorno de destino. Este repositorio no acredita validación clínica: sus salidas son experimentales y el proyecto debe presentarse como prototipo académico.

Próximas mejoras: configuración por entorno, separación de rutas y servicios, pruebas de los endpoints y una demo con casos de ejemplo. No se atribuyen métricas de precisión, tiempos ni disponibilidad que no estén documentadas.
