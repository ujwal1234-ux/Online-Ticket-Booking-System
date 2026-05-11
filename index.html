import os
import zipfile
from datetime import datetime, timedelta

# Create complete project automatically
project_dir = "ticket_system"
os.makedirs(project_dir, exist_ok=True)
os.chdir(project_dir)

# Create directories
dirs = ["templates", "static/css", "static/js"]
for d in dirs:
    os.makedirs(d, exist_ok=True)

# requirements.txt
with open("requirements.txt", "w") as f:
    f.write("Flask==3.0.3
Flask-SQLAlchemy==3.1.1
Werkzeug==3.0.4
")

# config.py
config = '''import os

class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'your-secret-key-change-in-production'
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or 'sqlite:///ticket_system.db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False
'''
with open("config.py", "w") as f:
    f.write(config)

# models.py (complete)
models_code = '''from flask_sqlalchemy import SQLAlchemy
from werkzeug.security import generate_password_hash, check_password_hash
from datetime import datetime

db = SQLAlchemy()

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password_hash = db.Column(db.String(128), nullable=False)
    is_admin = db.Column(db.Boolean, default=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    
    tickets = db.relationship('Ticket', backref='user', lazy=True)
    
    def set_password(self, password):
        self.password_hash = generate_password_hash(password)
    
    def check_password(self, password):
        return check_password_hash(self.password_hash, password)

class Event(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200), nullable=False)
    description = db.Column(db.Text)
    venue = db.Column(db.String(200), nullable=False)
    date = db.Column(db.DateTime, nullable=False)
    total_tickets = db.Column(db.Integer, nullable=False)
    available_tickets = db.Column(db.Integer, nullable=False)
    price = db.Column(db.Float, nullable=False)
    image_url = db.Column(db.String(300))
    
    tickets = db.relationship('Ticket', backref='event', lazy=True)

class Ticket(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
    event_id = db.Column(db.Integer, db.ForeignKey('event.id'), nullable=False)
    purchase_date = db.Column(db.DateTime, default=datetime.utcnow)
    quantity = db.Column(db.Integer, default=1)
    total_price = db.Column(db.Float)
'''
with open("models.py", "w") as f:
    f.write(models_code)

# COMPLETE app.py
app_code = '''from flask import Flask, render_template, request, redirect, url_for, flash, session
from models import db, User, Event, Ticket
from config import Config
from datetime import datetime, timedelta
import os

app = Flask(__name__)
app.config.from_object(Config)
db.init_app(app)

def init_db():
    with app.app_context():
        db.create_all()
        if Event.query.count() == 0:
            events = [
                Event(title="Rock Concert 2026", venue="Main Arena", 
                      date=datetime.now() + timedelta(days=30), total_tickets=1000,
                      available_tickets=850, price=75.00, 
                      description="Epic rock concert with top bands!"),
                Event(title="Tech Conference", venue="Convention Center",
                      date=datetime.now() + timedelta(days=45), total_tickets=500,
                      available_tickets=400, price=150.00,
                      description="Latest tech innovations and workshops"),
                Event(title="Comedy Night", venue="Comedy Club",
                      date=datetime.now() + timedelta(days=15), total_tickets=200,
                      available_tickets=180, price=35.00,
                      description="Laugh-out-loud comedy show")
            ]
            db.session.bulk_save_objects(events)
            db.session.commit()

@app.route('/')
def index():
    events = Event.query.filter(Event.available_tickets > 0).all()
    return render_template('index.html', events=events)

@app.route('/events')
def events():
    events = Event.query.filter(Event.available_tickets > 0).all()
    return render_template('events.html', events=events)

@app.route('/event/<int:event_id>')
def event_detail(event_id):
    event = Event.query.get_or_404(event_id)
    return render_template('event.html', event=event)

@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form['username']
        email = request.form['email']
        password = request.form['password']
        
        if User.query.filter_by(username=username).first():
            flash('Username already exists')
            return redirect(url_for('register'))
        
        user = User(username=username, email=email)
        user.set_password(password)
        db.session.add(user)
        db.session.commit()
        flash('Registration successful!')
        return redirect(url_for('login'))
    
    return render_template('register.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        user = User.query.filter_by(username=username).first()
        
        if user and user.check_password(password):
            session['user_id'] = user.id
            session['username'] = user.username
            flash('Login successful!')
            return redirect(url_for('index'))
        flash('Invalid credentials')
    
    return render_template('login.html')

@app.route('/logout')
def logout():
    session.clear()
    flash('Logged out successfully')
    return redirect(url_for('index'))

@app.route('/cart', methods=['GET', 'POST'])
def cart():
    if 'user_id' not in session:
        flash('Please login to purchase tickets')
        return redirect(url_for('login'))
    
    if request.method == 'POST':
        event_id = int(request.form['event_id'])
        quantity = int(request.form['quantity'])
        event = Event.query.get(event_id)
        
        if event.available_tickets < quantity:
            flash('Not enough tickets available')
            return redirect(url_for('cart'))
        
        existing_ticket = Ticket.query.filter_by(
            user_id=session['user_id'], event_id=event_id
        ).first()
        
        if existing_ticket:
            existing_ticket.quantity += quantity
            existing_ticket.total_price = existing_ticket.quantity * event.price
        else:
            ticket = Ticket(
                user_id=session['user_id'],
                event_id=event_id,
                quantity=quantity,
                total_price=quantity * event.price
            )
            db.session.add(ticket)
        
        event.available_tickets -= quantity
        db.session.commit()
        flash(f'{quantity} tickets added to cart!')
    
    user_tickets = Ticket.query.filter_by(user_id=session['user_id']).all()
    return render_template('cart.html', tickets=user_tickets)

@app.route('/purchase/<int:ticket_id>')
def purchase(ticket_id):
    if 'user_id' not in session:
        return redirect(url_for('login'))
    
    ticket = Ticket.query.get_or_404(ticket_id)
    if ticket.user_id != session['user_id']:
        flash('Unauthorized access')
        return redirect(url_for('cart'))
    
    flash(f'Payment successful! Purchased {ticket.quantity} tickets for ${ticket.total_price:.2f}')
    return redirect(url_for('cart'))

if __name__ == '__main__':
    init_db()
    app.run(debug=True)
'''
with open("app.py", "w") as f:
    f.write(app_code)

# All HTML templates
templates = {
    "base.html": '''<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Ticket System{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar