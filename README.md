# Lab-9.1

from flask import Flask, jsonify, render_template_string
 
app = Flask(__name__)
 
@app.route('/')
def index():
    return render_template_string('''
        <h1>Simple Calculator API</h1>
        <p>Use the following format for calculations:</p>
        <ul>
            <li><a href="/add/5/3">Add</a></li>
            <li><a href="/sub/5/3">Subtract</a></li>
            <li><a href="/mul/5/3">Multiply</a></li>
            <li><a href="/div/5/3">Divide</a></li>
        </ul>
        <p>Replace 5 and 3 with your own numbers.</p>
    ''')
 
@app.route('/<opt>/<float:a>/<float:b>')
def calculate(opt, a, b):
    if opt == 'add':
        result = a + b
    elif opt == 'sub':
        result = a - b
    elif opt == 'mul':
        result = a * b
    elif opt == 'div':
        if b == 0:
            return jsonify({'error': 'Division by zero is not allowed'}), 400
        result = a / b
    else:
        return jsonify({'error': 'Invalid operator'}), 400
 
    return jsonify({'result': result})
 
if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
