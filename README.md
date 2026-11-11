# Registro de Eventos — React + TypeScript + FastAPI + MySQL

Aplicación full stack para inscribir y administrar participantes de un evento. El frontend maneja el estado con **Context API + `useReducer`** y consume una API REST propia hecha con FastAPI que persiste en MySQL.

## Stack

| Capa | Tecnologías |
|---|---|
| Frontend | React 19, TypeScript, Vite, Context API, `useReducer` |
| Backend | Python, FastAPI, Pydantic, `mysql-connector-python` |
| Base de datos | MySQL |

## Funcionalidades

- Alta de participantes con formulario controlado (nombre, email, edad, país, modalidad, tecnologías, nivel) y aceptación de términos.
- Edición de un participante cargando sus datos en el mismo formulario.
- Baja con confirmación previa.
- Búsqueda por texto y filtros por modalidad y nivel.
- Estado global centralizado (`ParticipantesContext` + `participantesReducer`) y modelo tipado (`Participante`).
- API REST con CRUD completo y respuesta `404` cuando el registro no existe. Las consultas SQL están parametrizadas.

## Estructura

```
.
├── Backend/
│   └── app.py                  # API FastAPI (GET, POST, PUT, DELETE /participantes)
└── Frontend/
    └── src/
        ├── Componentes/        # Formulario, Filtros, ListaParticipantes, TarjetaParticipantes
        ├── context/            # ParticipantesContext (consumo de la API)
        ├── reducers/           # participantesReducer
        └── Models/             # Tipos TypeScript
```

## Endpoints

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/participantes` | Listar participantes |
| POST | `/participantes` | Crear participante |
| PUT | `/participantes/{id}` | Actualizar participante |
| DELETE | `/participantes/{id}` | Eliminar participante |

## Cómo correrlo

### 1. Base de datos

```sql
CREATE DATABASE participantes_db;
USE participantes_db;

CREATE TABLE participantes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL,
    edad INT NOT NULL,
    pais VARCHAR(100) NOT NULL,
    modalidad VARCHAR(50) NOT NULL,
    tecnologias VARCHAR(255),          -- lista separada por comas
    nivel VARCHAR(50) NOT NULL
);
```

### 2. Backend

Ajustar usuario y contraseña de MySQL en la función `get_db()` de `Backend/app.py`.

```bash
cd Backend
pip install fastapi "uvicorn[standard]" mysql-connector-python
uvicorn app:app --reload
```

Documentación interactiva: http://localhost:8000/docs

### 3. Frontend

```bash
cd Frontend
npm install
npm run dev
```

Abrir http://localhost:5173. El frontend espera la API en `http://localhost:8000`.
