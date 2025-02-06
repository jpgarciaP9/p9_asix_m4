 
Aquí tienes la traducción al catalán del tutorial:

---

# **Tutorial Bàsic de Flask**

---

### **1. Configurar un Entorn Virtual (virtualenv)**

Utilitzar un entorn virtual t'ajuda a mantenir les dependències organitzades i separades d'altres projectes.

**1.1. Crear l'entorn virtual:**

Obre el terminal i navega fins a la carpeta on vols crear el projecte. Després, executa:

```bash
python -m venv env
```
Que vol dir : python, executa el mòdul (-m) que es diu venv i crea la carpeta env

Això crearà una carpeta anomenada `env` que conté l'entorn virtual.

**1.2. Activar l'entorn virtual:**

- **Windows:**

  ```bash
  env\Scripts\activate
  ```

- **Linux/macOS:**

  ```bash
  source env/bin/activate
  ```

Veureu que el terminal mostra el nom de l'entorn virtual, indicant que està activat.

---

### **2. Instal·lació de Flask**

Amb l'entorn virtual activat, instal·la Flask:

```bash
pip install Flask
```

Per assegurar-te que Flask està instal·lat:

```bash
pip show Flask
```

---

### **3. Crear el Projecte i Rutes Bàsiques**

**3.1. Estructura del projecte:**

```
el_meu_projecte/
│
├── app.py
└── templates/
```

**3.2. Codi bàsic a `app.py`:**

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hola, món des de Flask!"

if __name__ == '__main__':
    app.run(debug=True)
```

**3.3. Executar l'aplicació:**

```bash
python app.py
```

Obre el navegador a **http://127.0.0.1:5000/** i veuràs *"Hola, món des de Flask!"*.

---

### **4. Rutes amb Paràmetres**

Pots capturar valors de la URL fent servir **placeholders** a les rutes.

```python
@app.route('/salutacio/<nom>')
def salutacio(nom):
    return f"Hola, {nom}!"
```

Si visites **http://127.0.0.1:5000/salutacio/Isabel**, veuràs:

```
Hola, Isabel!
```

**Tipus de paràmetres:**

```python
@app.route('/edat/<int:edat>')
def mostrar_edat(edat):
    return f"Tens {edat} anys."
```

Visita **http://127.0.0.1:5000/edat/25** i veuràs: *Tens 25 anys.*

---

### **5. Templates amb Jinja2**

Flask utilitza **Jinja2** per renderitzar plantilles HTML dinàmiques.

**5.1. Crear la carpeta `templates` i un fitxer `index.html`:**

```
el_meu_projecte/
│
├── app.py
└── templates/
    └── index.html
```

**5.2. Codi a `index.html`:**

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ titol }}</title>
</head>
<body>
    <h1>{{ missatge }}</h1>
    <p>Benvingut a la nostra pàgina web.</p>
</body>
</html>
```

**5.3. Renderitzar la plantilla des de `app.py`:**

```python
from flask import render_template

@app.route('/')
def home():
    return render_template('index.html', titol="Inici", missatge="Hola des de Flask!")
```

Ara, en visitar **http://127.0.0.1:5000/** veuràs el contingut de `index.html` amb dades dinàmiques.

---

### **6. Formularis (GET i POST)**

Flask permet gestionar formularis fent servir els mètodes **GET** i **POST**.

**6.1. Crear un formulari a `form.html`:**

```
el_meu_projecte/
└── templates/
    └── form.html
```

**Codi a `form.html`:**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Formulari</title>
</head>
<body>
    <h1>Introdueix el teu nom</h1>
    <form method="POST">
        <input type="text" name="nom" placeholder="El teu nom">
        <input type="submit" value="Enviar">
    </form>
</body>
</html>
```

**6.2. Ruta per mostrar i processar el formulari a `app.py`:**
Fem servir la mateixa ruta per a GET i POST
- GET: mostra el formulari
- POST: recull els valor dels paràmetres del formulari

```python
from flask import request

@app.route('/form', methods=['GET', 'POST'])
def form():

    # si s'arriba per POST vols dir que les dades han sigut enviades des del formulari
    # cal recollir-les amb request.form.get('nom_del_paràmetre_del_FORM')

    if request.method == 'POST':
        nom = request.form.get('nom')
        # aquí ja fem el qeu volguem, per exemple mostrar un missatge
        return f"<h1>Hola, {nom}!</h1>"

    # aquí només arribarà si no és post
    # com que la ruta només aceptta GET y POST quan el mètoda sigui GET entrarà aquì
    # i es mostrarà el formulari

    return render_template('form.html')
```

### **7.Redirecció**


Es pot redirigir una ruta. Això vol dir que en arribar a un punt de la funció qu gestiona una ruta es pot redirir a l'usuari cap a una altra
Per exemple si hem fet un insert una cegdaa recollits els paràmetres
podem redirigir cap a l'índex
La redireccció no es fa cap a un aruta sinó cap a la funció que procesa aquella ruta
```
from flask import Flask, redirect, url_for

app = Flask(__name__)

@app.route('/')
def home():
return "Aquesta és la pàgina principal."

@app.route('/redirigir')
def redirigir():
return redirect(url_for('home'))  # Redirigeix a la funció `home`

if __name__ == '__main__':
app.run(debug=True)
```

**7.1 Combinació GET-POST + Redirect**

 Suposem que tenin una aplicacio per a insertar y mostrar alumnes
- amb GET es mostra un formulari
- amb POST s'gafen les dades de l'usuari
- amb redirect es redirigeix (per exemple) a la funció que mostra tots els usuaris.
- __Atenció__:la redirecció es fa __cap la funció__ *llistar()* que és la que serveix la ruta insertar_alumne

```python
from flask import request

@app.route('/llistat_alumnes', methods=['GET', 'POST'])
def llistat():
    # codi per mostra el llista d'alumnes

@app.route('/insert_alumne', methods=['GET', 'POST'])
def insert_alumne():

    # Mostrar el formulari d0insercio
    if request.method == 'GET':
        rerueen (render_template('insert_alumne.html')
    if request.method == 'POST':
        nom = request.form.get('nom')
        edat= request.form.get('edat')

        # aqui es faria la inserció (per exmple en una BD
        # codi d'inserció ....
        return  redirect(url_for(' llistat '))  # Redirigeix a la funció `home`

    # aquí només arribarà si no és post
    # com que la ruta només aceptta GET y POST quan el mètoda sigui GET entrarà aquì
    # i es mostrarà el formulari

    return render_template('form.html')
```
---

### **8. Executar i Provar l'Aplicació**

1. Executa l'aplicació:

    ```bash
    python app.py
    ```

2. Visita **http://127.0.0.1:5000/form** al navegador.
3. Escriu el teu nom i fes clic a *Enviar*. Veureu el missatge de salutació personalitzat.

---

### **9. Resum del Tutorial**

- **Entorn virtual:** `python -m venv venv` i activació.
- **Instal·lació de Flask:** `pip install Flask`.
- **Rutes bàsiques:** `@app.route('/')`.
- **Rutes amb paràmetres:** `@app.route('/salutacio/<nom>')`.
- **Templates amb Jinja2:** Carpeta `templates/` i ús de `render_template()`.
- **Redirecció:** `return  redirect(url_for(' funcio '))`
- **Formularis (GET i POST):** `request.form.get('nom')`.

---

Amb això ja tens una base sòlida per treballar amb Flask! Vols afegir alguna cosa més, com ara connexió a una base de dades o autenticació? 🚀
