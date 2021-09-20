from flask import Flask, flash, redirect, url_for, render_template,  request
from datetime import datetime
from flask_mysqldb import MySQL


app = Flask(__name__)

app.secret_key = 'clave_secreta'
mysql = MySQL(app)


# Conexion DB

app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = 'rodrigo1306'
app.config['MYSQL_DB'] = 'flask'


# contex processor

@app.context_processor
def date_now():
    return {
        'now': datetime.utcnow()
    }


@app.route('/')
def index():

    edad = 140
    personas = ['rodrigo', 'luis', 'soloma', 'alexandra']
    return render_template('index.html', edad=edad, personas=personas, dato1='valor 1', lista=['12', '23', '556', 'holaquetal'])


@app.route('/informacion')
@app.route('/informacion/<nombre>')
def informacion(nombre=None):

    texto = ''
    if nombre != None:
        texto = f"<h1>Bienvenido {nombre}</h1>"

    return render_template('informacion.html', texto=texto)


@app.route('/contacto')
def contacto():

    return render_template('contacto.html')


@app.route('/lenguajes')
def lenguajes():
    return render_template('lenguajes.html')


@app.route('/crear', methods=['GET', 'POST'])
def crear():

    if request.method == 'POST':

        Marca = request.form['Marca']
        Modelo = request.form['Modelo']
        Precio = request.form['Precio']
        Ciudad = request.form['Ciudad']

        # return f"{Marca} {Modelo} {Precio} {Cuidad}"
        cursor = mysql.connection.cursor()
        cursor.execute(
            f"INSERT INTO flask VALUES(NULL, %s,  %s,  %s,  %s)", (Marca, Modelo, Precio, Ciudad))
        cursor.connection.commit()
        flash('has creado un coche correctamente')

        return redirect(url_for('index'))
    return render_template('crear.html')


@app.route('/coches')
def coches():
    cursor = mysql.connection.cursor()
    cursor.execute("SELECT * FROM flask")
    coches = cursor.fetchall()
    cursor.close()
    return render_template('coches.html', coches=coches)


@app.route('/coche/<coche_id>')
def coche(coche_id):
    cursor = mysql.connection.cursor()
    cursor.execute("SELECT * FROM flask WHERE id =%s", (coche_id))
    coche = cursor.fetchall()
    cursor.close()
    return render_template('coche.html', coche=coche[0])


@app.route('/borrar-coche/<coche_id>')
def borrar_coche(coche_id):
    cursor = mysql.connection.cursor()
    cursor.execute("DELETE FROM flask WHERE id =%s", (coche_id))
    mysql.connection.commit()

    flash('el coche se ha eliminado')

    return redirect(url_for('coches'))


@app.route('/editar-coche/<coche_id>', methods=['GET', 'POST'])
def editar_coche(coche_id):

    if request.method == 'POST':

        Marca = request.form['Marca']
        Modelo = request.form['Modelo']
        Precio = request.form['Precio']
        Ciudad = request.form['Ciudad']

        # return f"{Marca} {Modelo} {Precio} {Cuidad}"
        cursor = mysql.connection.cursor()
        cursor.execute(
            f"UPDATE flask SET Marca =%s, Modelo =%s, Precio =%s, Ciudad =%s WHERE id =%s ",
            (Marca, Modelo, Precio, Ciudad, coche_id))
        cursor.connection.commit()

        flash('has editado un coche correctamente')

        return redirect(url_for('coches'))

    cursor = mysql.connection.cursor()
    cursor.execute("SELECT * FROM flask WHERE id =%s", (coche_id))
    coche = cursor.fetchall()
    cursor.close()
    return render_template('crear.html', coche=coche[0])


if __name__ == '__main__':
    app.run(debug=True)
