# Development Setup Command

Quick command to set up both frontend and backend development servers.

## Usage
```bash
# Set up backend
cd backend && pip install -r requirements.txt && python manage.py migrate

# Set up frontend  
cd frontend && npm install

# Start both servers (in separate terminals)
cd backend && python manage.py runserver
cd frontend && npm start
```

## Environment Setup
1. Copy `backend/backend/settingsExample.py` to `backend/backend/settings.py`
2. Configure database settings in `settings.py`
3. Run migrations with `python manage.py migrate`