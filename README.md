# CodeAlpha
from flask import Flask, request, render_template_string
import sqlite3

app = Flask(__name__)

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form['username']
        password = request.form['password']
        conn = sqlite3.connect("users.db")
        cursor = conn.cursor()
        query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
        cursor.execute(query)
        result = cursor.fetchone()
        if result:
            return "Login successful!"
        else:
            return "Invalid credentials!"
    return render_template_string("<form method='post'>Username: <input name='username'><br>Password: <input name='password'><br><input type='submit'></form>")

@app.route("/data")
def data():
    user_id = request.args.get('id')
    conn = sqlite3.connect("data.db")
    cursor = conn.cursor()
    cursor.execute(f"SELECT * FROM data WHERE id = {user_id}")
    result = cursor.fetchall()
    return str(result)

if __name__ == "__main__":
    app.run(debug=True)
