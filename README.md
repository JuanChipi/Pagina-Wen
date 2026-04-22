# Pagina-Wen
from fastapi import FastAPI
from sqlite3 

db_peliculas = [{"titulo": "Matrix", "genero": "Accion", "puntaje": 5},
                {"titulo": "Esperando la Carroza", "genero": "Comedia", "puntaje": 5}
                ]

def inciar_db():
    conn = sqlite3.connect("letterbox.db")
    cursor = conn.cursor()
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS peliculas (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        titulo TEXT,
        genero TEXT,
        puntaje INTEGER)
    """)
    conn.commit()
    conn.close()

generos = ["Accion","Comedia"]
app = FastAPI()

@app.get("/peliculas")

def obtener_peliculas():
    conn = sqlite3.connect("letterbox.db")
    conn.row_factory = sqlite3.Row
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM peliculas")
    res = [dict(row) for row in cursor.fetchall()]
    conn.close()
    return res

#uvicorn main:app --reload     -> inicia un servidor web local en python

@app.post("/peliculas")
def cargar_una_pelicula(titulo,genero,puntaje):

    if not puntaje.isdigit():
        mensaje_error = "El puntaje tiene que ser un numero entero"
        return mensaje_error
    """
    nueva_pelicula = {"titulo": titulo, "genero": genero, "puntaje": puntaje}
    db_peliculas.append(nueva_pelicula)
    mensaje_retorno = "Pelicula guardada corectamente"
    """
    conn = sqlite3.connect("letterbox.db")
    cursor = conn,cursor()
    cursor.execute("INSERT INTO peliculas(titulo, genero, puntaje)")
    conn.commit()
    conn.close()
    mensaje_retorno = "Pelicula cargada correctamente"
    return mensaje_retorno
