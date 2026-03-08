
# 🔥 FireRiskAI - Sistema de Predicción de Riesgo de Incendios Forestales

## 📋 Descripción del Proyecto


FireRiskAI es una aplicación web impulsada por inteligencia artificial que combina **clasificación multiclase** y análisis predictivo para abordar un problema real: **la evaluación del riesgo de incendios forestales**. El sistema utiliza un modelo **XGBoost optimizado**, entrenado con el **Forest Cover Type Dataset** del UCI Machine Learning Repository, para **predecir el tipo de cobertura forestal y estimar el nivel de riesgo asociado**.
La visión de FireRiskAI se basa en una **estrategia de datos sólida**, fundamental para el desarrollo de un modelo de *machine learning* de alto rendimiento y confiabilidad, orientado a la toma de decisiones ambientales más precisas y sostenibles.



## 🎯 Características Principales

- **Clasificación de 7 tipos de bosque** con 97.07% de precisión
- **Evaluación de riesgo de incendio** basada en características de inflamabilidad
- **API REST** para predicciones en tiempo real
- **Análisis de features** con nombres reales interpretables
- **Documentación completa** para desarrolladores

## 🚀 Resultados del Modelo

### 📊 Métricas de Rendimiento
| Métrica | Valor | Estado |
|---------|-------|--------|
| **Accuracy** | 97.07% | ✅ Objetivo cumplido (≥97%) |
| **Overfitting** | 2.92% | ✅ Controlado (<5%) |
| **Errores** | 2.93% | ✅ Mínimos |

### 🎯 Tipos de Bosque Clasificados
| Clase | Nombre | Nivel de Riesgo | Score |
|-------|--------|-----------------|-------|
| 0 | Spruce/Fir | LOW | 2 |
| 1 | Lodgepole Pine | HIGH | 8 |
| 2 | Ponderosa Pine | MEDIUM | 5 |
| 3 | Cottonwood/Willow | LOW | 1 |
| 4 | Aspen | MEDIUM | 4 |
| 5 | Douglas-fir | MEDIUM | 6 |
| 6 | Krummholz | HIGH | 9 |

### 🔍 Top 5 Features Más Importantes
1. **Soil_Type37** (6.90%) - Tipo de suelo más determinante
2. **Soil_Type4** (5.41%) - Segundo tipo de suelo más relevante
3. **Soil_Type2** (5.26%) - Tercer tipo de suelo importante
4. **Soil_Type22** (4.84%) - Cuarto tipo de suelo relevante
5. **Wilderness_Area1** (4.20%) - Primera área silvestre

## 🛠️ Instalación y Configuración

### Prerrequisitos
- Python 3.11+
- pip
- virtualenv (recomendado)

### Instalación
```bash
# Clonar el repositorio
git clone <repository-url>
cd Grupo1_Modelos_de_Ensemble

# Crear entorno virtual
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# Instalar dependencias
pip install -r requirements.txt
```

### Estructura del Proyecto
```
Grupo1_Modelos_de_Ensemble/
├── src/
│   ├── api/                 # API REST endpoints
│   ├── evaluation/          # Scripts de evaluación
│   ├── models/              # Comparación de modelos
│   └── utils/               # Utilidades
├── models/                  # Modelos entrenados
│   ├── best_model.pkl      # Modelo XGBoost optimizado
│   ├── scaler.pkl          # Scaler para normalización
│   └── metadata.json       # Metadatos del modelo
├── data/
│   └── processed/          # Archivos generados
│       ├── confusion_matrix.png
│       ├── metrics_per_class.csv
│       └── feature_importance.png
├── docs/                   # Documentación
├── notebooks/              # Jupyter notebooks
└── tests/                  # Tests unitarios
```

## 🚀 Uso del Sistema

### 1. Evaluación del Modelo
```bash
# Ejecutar evaluación completa
python src/evaluation/model_evaluator.py
```

**Archivos generados:**
- `data/processed/confusion_matrix.png` - Matriz de confusión
- `data/processed/metrics_per_class.csv` - Métricas por clase
- `data/processed/feature_importance.png` - Importancia de features

### 2. API REST
```bash
# Iniciar servidor API
uvicorn app:app --reload --host 0.0.0.0 --port 8000
```

**Endpoint de predicción:**
```bash
POST /predict
Content-Type: application/json

{
  "features": [elevation, aspect, slope, ..., wilderness_area4]
}
```

**Respuesta:**
```json
{
  "prediction": 1,
  "class_name": "Lodgepole Pine",
  "confidence": 0.982,
  "risk_level": "HIGH",
  "risk_score": 8
}
```

### 3. Uso Programático
```python
from src.predict import ForestCoverPredictor

# Inicializar predictor
predictor = ForestCoverPredictor()

# Hacer predicción
features = [elevation, aspect, slope, ...]  # 54 features
result = predictor.predict(features)

print(f"Tipo de bosque: {result['class_name']}")
print(f"Confianza: {result['confidence']:.3f}")
```

## 📊 Dataset y Features

### Forest Cover Type Dataset
- **Fuente:** UCI ML Repository (ID: 31)
- **Muestras:** 581,012
- **Features:** 54 (10 continuas + 44 categóricas)
- **Clases:** 7 tipos de cobertura forestal

### Features Principales
- **Elevation** - Elevación del terreno
- **Aspect** - Orientación de la pendiente
- **Slope** - Inclinación de la pendiente
- **Horizontal_Distance_To_Hydrology** - Distancia horizontal al agua
- **Vertical_Distance_To_Hydrology** - Distancia vertical al agua
- **Horizontal_Distance_To_Roadways** - Distancia a carreteras
- **Hillshade_9am/Noon/3pm** - Sombreado en diferentes horas
- **Horizontal_Distance_To_Fire_Points** - Distancia a puntos de fuego
- **Wilderness_Area1-4** - Áreas silvestres (4 tipos)
- **Soil_Type1-40** - Tipos de suelo (40 tipos)

## 🔧 Desarrollo

### Entrenamiento de Modelos
```bash
# Comparación de modelos baseline
python src/models/01_baseline_comparison.py

# Optimización de XGBoost
python src/models/02_full_ensemble_comparison.py
```

### Tests
```bash
# Ejecutar tests
python -m pytest tests/
```

## 📈 Análisis de Rendimiento

### Matriz de Confusión
El modelo muestra excelente rendimiento en todas las clases, con mayor dificultad en clases minoritarias:
- **Spruce/Fir:** 97.35% precisión
- **Lodgepole Pine:** 97.19% precisión
- **Ponderosa Pine:** 96.52% precisión
- **Cottonwood/Willow:** 90.82% precisión (clase minoritaria)
- **Aspen:** 94.06% precisión (clase minoritaria)
- **Douglas-fir:** 94.69% precisión
- **Krummholz:** 97.57% precisión

### Análisis de Errores
- **Total de errores:** 3,406 de 116,203 (2.93%)
- **Confianza promedio en errores:** 74.4%
- **Clases más confundidas:** Cottonwood/Willow y Aspen (clases minoritarias)

## 🎯 Aplicaciones

### Gestión Forestal
- **Identificación de tipos de bosque** para inventarios forestales
- **Evaluación de riesgo de incendio** para planificación preventiva
- **Análisis de vulnerabilidad** de diferentes ecosistemas

### Investigación
- **Estudios ecológicos** sobre distribución de especies
- **Análisis de biodiversidad** en diferentes tipos de suelo
- **Modelado de ecosistemas** forestales

## 📚 Documentación Adicional

- [Evaluación Detallada del Modelo](docs/MODEL_EVALUATION.md)
- [Documentación de Endpoints](docs/README_ENDPOINTS.md)
- [Guía de Desarrollo](docs/DEVELOPMENT.md)

## 🤝 Contribuciones

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver `LICENSE` para más detalles.

## 👥 Equipo

- **Rol 1:** Análisis Exploratorio de Datos (EDA)
- **Rol 2:** Ingeniero/a de Modelos
- **Rol 3:** Análisis de Riesgo de Incendios
- **Rol 4:** Evaluación de Modelos


## 👩‍💻 Contribuyentes

| Nombre | GitHub | LinkedIn |
|--------|--------|----------|
| **[Alfonso Bermúdez Torres]** | [@GHalfbbt](https://github.com/GHalfbbt) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white)]([https://www.linkedin.com/in/alfonsobermudeztorres/]]) |
| **[Bárbara Sánchez Urbano ]** | [@Barbarasanchez11](https://github.com/Barbarasanchez11) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white)]([https://github.com/Barbarasanchez11]) |
| **[Bunty Nanwani]** | [@buntynanwani](https://github.com/buntynanwani) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white)]([https://www.linkedin.com/in/buntynanwani/]) |
| **[Aroa Mateo Gómez]** | [@Arowi95](https://github.com/Arowi95) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white)]([https://www.linkedin.com/in/aroamateogomez/]) |



## 📞 Contacto

Para preguntas o sugerencias, por favor contacta al equipo de desarrollo.

---


<div align="center">

*Desarrollado con ❤️ usando Python y Streamlit*

</div>

